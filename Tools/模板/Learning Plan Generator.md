<%*
// 1. 获取当前触发脚本的这个“宿主文件”
const activeFile = tp.config.target_file;

// 2. 弹出选项
const types = ["季度", "月", "周", "日"];
const type = await tp.system.suggester(types, types);
if (!type) return;

// 3. 时间与路径逻辑 (保持不变)
const now = window.moment();
const year = now.format("YYYY");
const month = now.format("MM");
const weekNum = now.isoWeek();
const quarter = now.quarter();

let fileName, folderPath, templateName;
const baseFolder = "学习安排";
const templateFolder = "Tools/模板";

switch (type) {
    case "季度":
        templateName = "Quarterly Learning Plan";
        folderPath = `${baseFolder}/${year}/计划管理`;
        fileName = `季度Q${quarter}`;
        break;
    case "月":
        templateName = "Monthly Learning Plan";
        folderPath = `${baseFolder}/${year}/计划管理`;
        fileName = `${month}月`;
        break;
    case "周":
        templateName = "Weekly Learning Plan";
        folderPath = `${baseFolder}/${year}/${month}`;
        fileName = `第${now.format("ww")}周`; 
        break;
    case "日":
        templateName = "Daily Learning Plan";
        folderPath = `${baseFolder}/${year}/${month}`;
        fileName = `${month}-${now.format("DD")}`;
        break;
}

const fullPath = `${folderPath}/${fileName}.md`;

// 4. 递归创建文件夹
const folders = folderPath.split('/');
let currentPath = "";
for (const folder of folders) {
    currentPath += (currentPath ? "/" : "") + folder;
    if (!app.vault.getAbstractFileByPath(currentPath)) {
        await app.vault.createFolder(currentPath);
    }
}

// 5. 获取模板内容
const templateFile = app.vault.getAbstractFileByPath(`${templateFolder}/${templateName}.md`);
if (!templateFile) {
    new Notice(`❌ 找不到模板: ${templateFolder}/${templateName}.md`);
    return;
}
const content = await app.vault.read(templateFile);

// 6. 创建目标计划文件
let targetFile = app.vault.getAbstractFileByPath(fullPath);
if (!targetFile) {
    targetFile = await app.vault.create(fullPath, content);
}

// 7. 【关键步骤】处理宿主文件并跳转
// 如果当前文件就是我们刚创建的目标文件，直接渲染即可
// 如果不是（即产生了一个未命名文件），则删除未命名文件，打开目标文件
if (activeFile && activeFile.path !== targetFile.path) {
    // 异步打开目标文件
    await app.workspace.getLeaf().openFile(targetFile);
    // 延迟一瞬删除多余的临时文件
    setTimeout(() => {
        if (activeFile.name.includes("未命名") || activeFile.path.includes("Untitled")) {
            app.vault.trash(activeFile, true);
        }
    }, 500);
}

new Notice(`✅ ${type}计划已就绪`);
%>