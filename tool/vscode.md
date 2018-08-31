# VSCode

---

## plugin

* `view in browser` *右键在浏览器中打开*
* `Material Icon Theme` *图标*
* `guides` *铺助缩进线*
* `stylefmt` *代码格式化插件*
* `eslint` *代码规则检查*
* `vetor` *vue插件*
* `auto rename tag` *自动修改另一侧标签*
* `html css support` *扩展css*
* `path intellisense` *路径自动补全*
* `Bracket Pair Colorizer` *彩色括号*
* `HTML Snippets` *智能提示HTML标签，以及标签含义*
* `jQuery Code Snippets` *jquery代码提示*
* `Markdown Preview Enhancedr` *实时预览markdown*
* `markdownlint` *markdown语法纠错*
* `React/Redux/react-router Snippets` *React/Redux/react-router语法智能提示*
* `JavaScript (ES6) code snippets`
* `Debugger for Chrome`

### UserSetting

``` json
{
    "terminal.integrated.shell.windows": "C:\\Windows\\System32\\cmd.exe",
    "terminal.integrated.cursorBlinking": true,
    "terminal.integrated.fontFamily": "monospace",
    "terminal.integrated.fontSize": 16,
    "workbench.colorTheme": "Solarized Light",
    "workbench.iconTheme": "material-icon-theme",
    "files.autoSave": "onWindowChange",
    "files.trimTrailingWhitespace": true,
    "files.associations": {
        "*.es": "javascript",
        "*.es6": "javascript"
    },
    "editor.fontFamily": "'Fira Code'",
    "editor.formatOnSave": true,
    "editor.fontLigatures": true,
    "editor.minimap.enabled": true,
    "editor.renderIndentGuides": false,
    "editor.occurrencesHighlight": false,
    "editor.fontSize": 22,
    "editor.minimap.maxColumn": 120,
    "editor.rulers": [
        100
    ],
    "search.exclude": {
        "**/node_modules": true,
        "**/bower_components": true,
        "**/dist": true,
        "**/coverage": true,
        "**/doc": true
    },
    "git.confirmSync": false,
    //eslint：检查语法
    "eslint.autoFixOnSave": true,
    "eslint.validate": [
        "javascript",
        "javascriptreact",
        "html",
        "jsx",
        "vue",
        {
            "language": "html",
            "autoFix": true
        }
    ],
    "editor.tabSize": 2,
    "explorer.confirmDelete": false
}
```