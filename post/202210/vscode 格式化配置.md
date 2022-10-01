<!--
 * @Descripttion: 
 * @Version: 1.0
 * @Author: Han
 * @Date: 2022-10-01 14:05:08
 * @LastEditors: Han
 * @LastEditTime: 2022-10-01 14:07:05
-->
# vscode 格式化配置

配置前要至少安装4个插件

- ESlint
- KoroFileHeader
- Prettier
- Vetur

安装后，打开vscode配置文件，配置JSON

```json
{
    "workbench.colorTheme": "Ayu Mirage",
    "git.ignoreWindowsGit27Warning": true,
    "editor.lineHeight": 30,
    "debug.console.lineHeight": 16,
    "files.exclude": {
        "**/node_modules": false
    },
    "window.zoomLevel": 0,
    "terminal.integrated.cursorBlinking": true,
    "terminal.integrated.cursorStyle": "line",
    "terminal.integrated.lineHeight": 0,
    "terminal.integrated.fontSize": 16,
    "editor.fontWeight": 500,
    "editor.fontFamily": "Monaco, Source Code Pro, Consolas, 'Courier New', monospace",
    "workbench.iconTheme": "helium-icon-theme",
    "security.workspace.trust.untrustedFiles": "open",
    "tabnine.experimentalAutoImports": true,
    "editor.inlineSuggest.enabled": true,
    "cssrem.rootFontSize": 50,
    "editor.formatOnSave": true,
    "editor.detectIndentation": false,
    // 重新设定tabsize
    "editor.tabSize": 4,
    "markdown-preview-enhanced.mathRenderingOption": "None",
    "editor.minimap.enabled": false,
    "powermode.enabled": true,
    "github.copilot.enable": {
        "*": true,
        "yaml": false,
        "plaintext": false,
        "markdown": false,
        "properties": true
    },
    "vetur.format.options.tabSize": 4,
    "vetur.format.defaultFormatter.html": "js-beautify-html",
    // "vetur.format.defaultFormatter.html": "prettier",
    "vetur.format.defaultFormatterOptions": {
        // vue组件中html代码格式化样式
        "js-beautify-html": {
            // 对属性进行换行。
            // - auto: 仅在超出行长度时才对属性进行换行。
            // - force: 对除第一个属性外的其他每个属性进行换行。
            // - force-aligned: 对除第一个属性外的其他每个属性进行换行，并保持对齐。
            // - force-expand-multiline: 对每个属性进行换行。
            // - aligned-multiple: 当超出折行长度时，将属性进行垂直对齐。
            "wrap_attributes": "auto"
        },
        "prettier": {
            "semi": false,
            "singleQuote": true
        }
    },
    "vetur.validation.template": false,
    //  让函数(名)和后面的括号之间加个空格
    "javascript.format.insertSpaceBeforeFunctionParenthesis": true,
    "workbench.editor.enablePreview": false,
    "fileheader.customMade": {
        "Descripttion": "",
        "Version": "1.0",
        "Author": "Han",
        "Date": "Do not edit",
        "LastEditors": "Han",
        "LastEditTime": "Do not edit"
    },
    // 函数注释
    "fileheader.cursorMode": {
        "description": "",
        "param": "",
        "return": ""
    },
    "fileheader.configObj": {
        "createFileTime": true,
        "language": {
            "languagetest": {
                "head": "/$$",
                "middle": " $ @",
                "end": " $/",
                "functionSymbol": {
                    "head": "/** ",
                    "middle": " * @",
                    "end": " */"
                },
                "functionParams": "js"
            }
        },
        "autoAdd": true,
        "autoAddLine": 100,
        "autoAlready": true,
        "annotationStr": {
            "head": "/*",
            "middle": " * @",
            "end": " */",
            "use": false
        },
        "headInsertLine": {
            "php": 2,
            "sh": 2
        },
        "beforeAnnotation": {
            "文件后缀": "该文件后缀的头部注释之前添加某些内容"
        },
        "afterAnnotation": {
            "文件后缀": "该文件后缀的头部注释之后添加某些内容"
        },
        "specialOptions": {
            "特殊字段": "自定义比如LastEditTime/LastEditors"
        },
        "switch": {
            "newlineAddAnnotation": true
        },
        "supportAutoLanguage": [],
        "prohibitAutoAdd": ["json"],
        "folderBlacklist": ["node_modules", "文件夹禁止自动添加头部注释"],
        "prohibitItemAutoAdd": [
            "项目的全称, 整个项目禁止自动添加头部注释, 可以使用快捷键添加"
        ],
        "moveCursor": true,
        "dateFormat": "YYYY-MM-DD HH:mm:ss",
        "atSymbol": ["@", "@"],
        "atSymbolObj": {
            "文件后缀": ["头部注释@符号", "函数注释@符号"]
        },
        "colon": [": ", ": "],
        "colonObj": {
            "文件后缀": ["头部注释冒号", "函数注释冒号"]
        },
        "filePathColon": "路径分隔符替换",
        "showErrorMessage": false,
        "writeLog": false,
        "wideSame": false,
        "wideNum": 13,
        "functionWideNum": 0,
        "CheckFileChange": false,
        "createHeader": true,
        "useWorker": false,
        "designAddHead": false,
        "headDesignName": "random",
        "headDesign": false,
        "cursorModeInternalAll": {},
        "openFunctionParamsCheck": true,
        "functionParamsShape": ["{", "}"],
        "functionBlankSpaceAll": {},
        "functionTypeSymbol": "*",
        "typeParamOrder": "type param",
        "customHasHeadEnd": {},
        "throttleTime": 60000
    },
    // 添加 vue 支持
    "eslint.validate": ["javascript", "html", "vue"],
    //  去掉代码结尾的分号
    "prettier.semi": false,
    "prettier.tabWidth": 4,
    //  使用单引号替代双引号
    "prettier.singleQuote": true,
    "[jsonc]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[vue]": {
        "editor.defaultFormatter": "octref.vetur"
    },
    // 代码是否按屏幕宽度换行
    "editor.wordWrap": "on",
    // 每次保存的时候将代码按eslint格式进行修复
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    },
    "[javascript]": {
        "editor.defaultFormatter": "vscode.typescript-language-features"
    }
}

```