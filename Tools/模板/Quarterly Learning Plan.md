<%*
// 获取当前日期和季度信息
const date = tp.date.now("YYYY-MM-DD");
const year = tp.date.now("YYYY");
const month = parseInt(tp.date.now("M"));
const quarter = Math.ceil(month / 3);
const quarterNames = ["第一季度 (春)", "第二季度 (夏)", "第三季度 (秋)", "第四季度 (冬)"];
const currentQuarterName = quarterNames[quarter - 1];
%>
# 📅 <% year %> - <% currentQuarterName %> 计划

## 🎯 核心目标
