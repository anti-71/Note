<%* 
const date = tp.date.now("YYYY/MM/MM-DD"); 
const filePath = `学习安排/${date}.md`; 
const file = app.vault.getAbstractFileByPath(filePath); 
if (file) { 
app.workspace.getLeaf(false).openFile(file); 
} else { 
new Notice("文件未找到: " + filePath); 
} 
%> 