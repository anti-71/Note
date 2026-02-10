---
type: 学习安排
date: <% tp.date.now("YYYY-MM-DD") %>
---
# 📅 学习安排 | <% tp.date.now("YYYY-MM-DD") %>
> [!quote] 每日金句
> <% tp.web.daily_quote() %>

---

## ⏳ 倒计时
- **考研/考试倒计时**：还有 `<%* let target = new Date("2026-12-19"); 
  let today = new Date();
  let diff = Math.ceil((target - today) / (1000 * 60 * 60 * 24));
  tR += diff;
%>` 天

---

## 📊 今日学习目标
```button
name 创建学习项
type command
action QuickAdd: 添加学习任务
templater true
customColor #F0EBFF
customTextColor #50416E
```
