

以下是 `grep` 命令的详细用法说明，结合你的测试场景（如过滤日志中的“ERROR”关键字），提供可直接操作的命令示例：

---

### **1. `grep` 基础语法**
```bash
grep [选项] "搜索模式" 文件名
```

---

### **2. 测试场景实战命令**
#### **(1) 基础用法：在文件中查找关键字**
```bash
# 在文件 error.log 中搜索包含 "ERROR" 的行
grep "ERROR" /var/log/error.log

# 示例输出：
# 2023-10-01 12:34:56 [ERROR] Database connection failed
# 2023-10-01 12:35:01 [ERROR] User login timeout
```

#### **(2) 常用选项**
| 选项 | 说明 | 测试场景示例 |
|------|------|--------------|
| `-i` | 忽略大小写 | `grep -i "error" app.log`（匹配 Error/ERROR/error） |
| `-n` | 显示行号 | `grep -n "NullPointer" exception.log`（定位问题代码行） |
| `-v` | **反向匹配** | `grep -v "INFO" app.log`（过滤掉所有包含 INFO 的行） |
| `-c` | 统计匹配行数 | `grep -c "WARN" system.log`（统计警告日志数量） |
| `-r` | 递归目录搜索 | `grep -r "Timeout" /var/log/`（搜索整个日志目录） |

---

### **3. 结合你的 Day3 任务**
#### **(1) 过滤日志中的 "ERROR" 关键字**
```bash
# 在文件 /home/user/app.log 中搜索 "ERROR"，显示行号
grep -n "ERROR" /home/user/app.log

# 输出示例：
# 45:2023-10-01 [ERROR] Payment failed: invalid token
# 89:2023-10-01 [ERROR] API response timeout
```

#### **(2) 扩展场景：多文件搜索**
```bash
# 在多个文件中搜索（如 .log 扩展名文件）
grep "ERROR" *.log

# 递归搜索目录（如 /var/log 及其子目录）
grep -r "ERROR" /var/log/
```

---

### **4. 高阶用法（测试工程师必备）**
#### **(1) 正则表达式匹配**
```bash
# 匹配以 "ERROR" 开头的行
grep "^ERROR" app.log

# 匹配包含 "ERROR" 或 "WARN" 的行（-E 启用扩展正则）
grep -E "ERROR|WARN" system.log

# 匹配数字错误码（如 ERR_500 或 ERR_404）
grep "ERR_[0-9]\{3\}" api_errors.log
```

#### **(2) 管道符组合（分析日志）**
```bash
# 统计 "ERROR" 出现次数最多的前3个文件
grep -r "ERROR" /var/log/ | cut -d ':' -f 1 | sort | uniq -c | sort -nr | head -3

# 输出示例：
#   12 /var/log/app/backend.log
#    8 /var/log/db/mysql.log
#    5 /var/log/auth.log
```

---

### **5. 常见问题排查**
- **文件不存在**：检查路径是否正确（使用 `ls` 确认文件存在）  
- **权限不足**：添加 `sudo`（如 `sudo grep "ERROR" /var/log/secure`）  
- **无结果输出**：尝试 `-i` 忽略大小写或调整搜索关键词  

---

### **6. 结合你的学习计划**
明日任务中，你可以这样使用 `grep`：  
```bash
# 在日志中筛选包含 "ERROR" 的行，保存到新文件
grep "ERROR" /var/log/app.log > ~/Desktop/error_list.txt
```

掌握 `grep` 是测试工程师分析日志、定位 Bug 的核心技能！继续加油！ 🚀