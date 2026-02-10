<%*
const callouts = {
    note: '🔵 ✏ Note',
    info: '🔵 ℹ Info',
    todo: '🔵 🔳 Todo',
    tip: '🌐 🔥 Tip / Hint / Important',
    abstract: '🌐 📋 Abstract / Summary / TLDR',
    question: '🟡 ❓ Question / Help / FAQ',
    quote: '🔘 💬 Quote / Cite',
    example: '🟣 📑 Example',
    success: '🟢 ✔ Success / Check / Done',
    warning: '🟠 ⚠ Warning / Caution / Attention',
    failure: '🔴 ❌ Failure / Fail / Missing',
    danger: '🔴 ⚡ Danger / Error',
    bug: '🔴 🐞 Bug',
};

// 1. 选择类型
const type = await tp.system.suggester(Object.values(callouts), Object.keys(callouts), true, 'Select callout type');
if (!type) return; // 防止误触 Esc 导致脚本报错

// 2. 输入标题
const title = await tp.system.prompt('Title:', '', true);

// 3. 处理内容：优先获取选中文本，否则弹出输入框
let content = tp.file.selection();
if (!content) {
    content = await tp.system.prompt('Content:', '', true, true);
}

// 4. 格式化内容：确保每一行前面都有 '>'
const formattedContent = content ? content.split('\n').map(line => `> ${line}`).join('\n') : "> ";

// 5. 输出结果 (移除了 ${fold})
const calloutHead = `> [!${type}] ${title}\n`;
tR += calloutHead + formattedContent;
-%>