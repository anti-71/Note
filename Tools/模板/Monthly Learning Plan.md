---
type: 学习安排
date: <% tp.date.now("YYYY-MM-DD") %>
---
# 📅 <% tp.date.now("YYYY-MM") %> 月度学习计划

---

## 📊 每日学习热力图

```dataviewjs
const cur = dv.current();

if (cur && cur.date) {
    const dateObj = new Date(cur.date);
    const year = dateObj.getFullYear();
    const month = String(dateObj.getMonth() + 1).padStart(2, '0');
    const folderPath = `学习安排/${year}/${month}`;
    const daysInMonth = new Date(year, parseInt(month), 0).getDate();
    const dataMap = new Map();

    // 颜色逻辑
    const getColor = (score) => {
        if (!score || score < 20) return "#95a5a6"; 
        if (score < 40) return "#3498db";
        if (score < 60) return "#2ecc71";
        if (score < 80) return "#f1c40f";
        return "#e74c3c";
    };

    // 读取数据
    const pages = dv.pages(`"${folderPath}"`);
    pages.forEach(p => {
        const dayMatch = p.file.name.match(/\d{2}-(\d{2})/);
        if (dayMatch) {
            const dayNum = parseInt(dayMatch[1]);
            const score = p["学习得分"] || 0;
            dataMap.set(dayNum, { score: score.toFixed(1), color: getColor(score) });
        }
    });

    let boxes = "";
    for (let i = 1; i <= daysInMonth; i++) {
        const d = dataMap.get(i) || { score: "0.0", color: "#95a5a6" };
        boxes += `<div style="width:28px;height:28px;background-color:${d.color};border-radius:4px;border:1px solid rgba(0,0,0,0.05);" title="第${i}天 得分: ${d.score}"></div>`;
    }

    const container = `
<div style="display:flex; flex-direction:column; align-items:flex-end; padding:10px 0; width:fit-content; margin:0 auto;">
    <div style="display:grid; grid-template-columns:repeat(10, 1fr); gap:8px;">
        ${boxes}
    </div>
    
    <div style="margin-top:12px; display:flex; align-items:center; gap:8px; font-size:10px; opacity:0.4;">
        <span>Less</span>
        <div style="display:flex; gap:3px;">
            <div style="width:8px; height:8px; background:#95a5a6; border-radius:1px;"></div>
            <div style="width:8px; height:8px; background:#3498db; border-radius:1px;"></div>
            <div style="width:8px; height:8px; background:#2ecc71; border-radius:1px;"></div>
            <div style="width:8px; height:8px; background:#f1c40f; border-radius:1px;"></div>
            <div style="width:8px; height:8px; background:#e74c3c; border-radius:1px;"></div>
        </div>
        <span>More</span>
    </div>
</div>`;

    dv.el("div", container);
}
```

---

## 🔙 未完成任务

```dataview
task
from "学习安排"
where !completed 
and file.name = "<%* 
  const lastMonth = window.moment().subtract(1, 'months').format('MM月');
  tR += lastMonth;
%>"
```

---

## 📈 本月任务


---

## 🔍 本月反思

### 👍 本月核心成就

### 😵 本月核心困难