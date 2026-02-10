<%*
const categories = ["英语", "数学", "408", "Anki", "本科学习"];
const selectedCat = await tp.system.suggester(categories, categories);
if (!selectedCat) return;

let finalContent = "";

if (selectedCat === "英语") {
    const items = [
        { d: "背单词", v: "英语-背单词: 复习   词, 学   词" },
        { d: "外刊", v: "英语-外刊:   篇" },
        { d: "网课", v: "英语-网课:   节" }
    ];
    finalContent = await tp.system.suggester(items.map(i => i.d), items.map(i => i.v));
} else if (selectedCat === "数学") {
    // 新增：选择数学子科目
    const subSubjects = ["高数", "线代", "概率论"];
    const sub = await tp.system.suggester(subSubjects, subSubjects);
    if (!sub) return;

    const items = [
        { d: "做题", type: "task" },
        { d: "网课", v: `数学-${sub}-网课:   节` },
        { d: "错题整理", v: `数学-${sub}-错题整理:   题` }
    ];
    const selected = await tp.system.suggester(items.map(i => i.d), items);
    if (!selected) return;
    
    if (selected.type === "task") {
        const bookName = await tp.system.prompt("请输入书名");
        finalContent = `数学-${sub}-做题: ${bookName}: [章节] (  /  )`;
    } else {
        finalContent = selected.v;
    }
} else if (selectedCat === "408") {
    const subjects = ["数据结构", "计算机组成原理", "操作系统", "计算机网络"];
    const sub = await tp.system.suggester(subjects, subjects);
    if (!sub) return;
    const types = [
        { d: "网课", v: `408-${sub}-网课:   节` },
        { d: "做题", type: "task" }
    ];
    const selected = await tp.system.suggester(types.map(i => i.d), types);
    if (!selected) return;

    if (selected.type === "task") {
        const bookName = await tp.system.prompt("请输入书名");
        finalContent = `408-${sub}-做题: ${bookName}: [章节] (  /  )`;
    } else {
        finalContent = selected.v;
    }
} else if (selectedCat === "Anki") {
    finalContent = "Anki";
} else if (selectedCat === "本科学习") {
    const courseName = await tp.system.prompt("请输入本科科目名称");
    finalContent = `本科学习-${courseName}`;
}

if (finalContent) { 
    return `- [ ] ${finalContent}`; 
}
%>