

以下是针对测试工程师场景的 **SQL 基础操作指南**，涵盖数据库创建、表结构设计、数据插入及查询的核心语法，并结合测试数据管理需求提供实用示例：

---

### **1. 创建数据库与表（测试环境搭建）**
#### **(1) 创建数据库**
```sql
-- 创建测试专用数据库，指定字符集（避免中文乱码）
CREATE DATABASE test_environment 
DEFAULT CHARACTER SET utf8mb4 
COLLATE utf8mb4_general_ci;
```

#### **(2) 切换数据库**
```sql
USE test_environment;
```

#### **(3) 创建用户表（存储测试账号）**
```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,  -- 用户ID，自增主键
    username VARCHAR(50) NOT NULL UNIQUE,    -- 用户名，唯一约束
    password VARCHAR(100) NOT NULL,          -- 密码（测试环境可明文）
    email VARCHAR(100),                      -- 邮箱（可选字段）
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP  -- 创建时间
);
```

#### **(4) 创建订单表（关联用户）**
```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    product_name VARCHAR(100) NOT NULL,
    amount DECIMAL(10, 2) CHECK (amount > 0),  -- 金额需大于0
    status ENUM('pending', 'completed', 'cancelled') DEFAULT 'pending',
    FOREIGN KEY (user_id) REFERENCES users(user_id)  -- 外键约束
);
```

---

### **2. 插入测试数据**
#### **(1) 插入用户数据**
```sql
-- 插入基础测试账号
INSERT INTO users (username, password, email) 
VALUES 
    ('test_user1', 'password123', 'user1@test.com'),
    ('test_user2', 'abc@123', 'user2@test.com');

-- 插入边界值测试账号（超长用户名）
INSERT INTO users (username, password) 
VALUES ('a_very_long_username_that_exceeds_normal_length_limits_but_needs_to_be_tested', 'securepass');
```

#### **(2) 插入订单数据**
```sql
INSERT INTO orders (user_id, product_name, amount, status)
VALUES 
    (1, 'Laptop', 999.99, 'completed'),
    (1, 'Mouse', 25.50, 'pending'),
    (2, 'Keyboard', 45.00, 'cancelled');
```

---

### **3. 数据查询（测试验证核心）**
#### **(1) 基础查询**
```sql
-- 查询所有用户（测试数据完整性）
SELECT * FROM users;

-- 查询特定字段（验证关键信息）
SELECT username, email FROM users WHERE user_id = 1;
```

#### **(2) 条件查询**
```sql
-- 查询状态为 "pending" 的订单（验证业务流程）
SELECT * FROM orders WHERE status = 'pending';

-- 查询金额大于50的订单（边界值测试）
SELECT * FROM orders WHERE amount > 50;
```

#### **(3) 排序与分页**
```sql
-- 按金额降序排列，分页查看（测试排序逻辑）
SELECT * FROM orders 
ORDER BY amount DESC 
LIMIT 2 OFFSET 0;  -- 第一页，每页2条
```

#### **(4) 连接查询（多表关联验证）**
```sql
-- 查询用户及其订单（验证外键关系）
SELECT u.username, o.product_name, o.amount
FROM users u
INNER JOIN orders o ON u.user_id = o.user_id;
```

---

### **4. 测试场景实战示例**
#### **场景：验证用户登录功能**
```sql
-- 检查用户名和密码是否匹配（登录测试）
SELECT user_id FROM users 
WHERE username = 'test_user1' AND password = 'password123';

-- 预期结果：返回 user_id=1
```

#### **场景：统计用户订单状态**
```sql
-- 统计每个用户的订单状态数量（数据聚合测试）
SELECT 
    u.username,
    SUM(CASE WHEN o.status = 'completed' THEN 1 ELSE 0 END) AS completed_count,
    SUM(CASE WHEN o.status = 'pending' THEN 1 ELSE 0 END) AS pending_count
FROM users u
LEFT JOIN orders o ON u.user_id = o.user_id
GROUP BY u.user_id;
```

---

### **5. 常见问题排查**
- **表不存在**：确认是否已切换数据库（`USE database_name;`）  
- **语法错误**：检查引号、逗号是否闭合，避免保留字（如`order`改为`orders`）  
- **外键约束失败**：确保插入数据前关联表（如`users`）已存在对应记录  

---

### **6. 安全提示（测试环境专用）**
- **敏感数据脱敏**：生产数据导入测试环境时，需对密码、手机号等字段进行掩码处理  
- **权限控制**：测试数据库账号仅授予必要权限（如禁止`DROP TABLE`）  

掌握这些 SQL 操作后，你将能高效构建测试数据集并验证数据逻辑！ 🚀