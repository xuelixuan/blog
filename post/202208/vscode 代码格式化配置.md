# vscode 代码格式化配置

可能是因为插件冲突，所以导致了 vscode 代码格式化出现了问题，重新整理一份 vscode 保存时代码格式化的配置

-   先安装 prettier

```json
{
	"workbench.colorTheme": "Dracula",
	"workbench.iconTheme": "material-icon-theme",
	"workbench.editor.enablePreview": false, // 禁止预览，直接单开文件
	"editor.fontSize": 16,
	"editor.lineHeight": 30,
	"editor.inlineSuggest.enabled": true,
	// 开启自动保存
	"editor.formatOnSave": true,
	"editor.formatOnType": true,
	// prettier 配置
	"editor.defaultFormatter": "esbenp.prettier-vscode", // 设置编辑器的默认格式化工具为 prettier
	"[javascript]": {
		// 根据语言设置其对应的默认格式化工具
		"editor.defaultFormatter": "esbenp.prettier-vscode", // 设置 javascript 的默认格式化工具为 prettier
		"editor.formatOnSave": true // 保存的时候自动格式化
	},
	"prettier.printWidth": 140,
	"prettier.tabWidth": 4,
	"prettier.useTabs": true,
	"prettier.bracketSpacing": true,
	"prettier.jsxBracketSameLine": true,
	"prettier.vueIndentScriptAndStyle": true,

	// 编辑器整体字体放大
	"window.zoomLevel": 1,
	// 自动变更导入路径，需在.jsconfig文件中进行路径配置
	"javascript.preferences.importModuleSpecifier": "non-relative",
	"javascript.updateImportsOnFileMove.enabled": "always",
	"editor.codeActionsOnSave": {
		"source.fixAll": true,
		"addMissingImports": true
	},
	// 开启emmet
	"emmet.includeLanguages": {
		"javascript": "javascriptreact",
		"vue": "vue",
		"postcss": "css",
		"scss": "scss",
		"less": "less",
		"stylus": "stylus",
		"json": "json",
		"jsonc": "jsonc",
		"typescript": "typescript",
		"typescriptreact": "typescriptreact",
		"html": "html",
		"markdown": "markdown"
	},
	"emmet.syntaxProfiles": {
		"javascript": "jsx",
		"javascriptreact": "jsx",
		"vue": "html",
		"typescript": "jsx",
		"typescriptreact": "jsx",
		"html": "html",
		"markdown": "markdown",
		"json": "json",
		"jsonc": "jsonc",
		"postcss": "css"
	}
}
```
