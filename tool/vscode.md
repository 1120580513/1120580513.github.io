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
  "window.zoomLevel": 0,
  "workbench.colorTheme": "Solarized Dark",
  "workbench.iconTheme": "vscode-icons",
  "files.autoSave": "off",
  "files.trimTrailingWhitespace": true,
  "files.associations": {
    "*.es": "javascript",
    "*.es6": "javascript"
  },
  "files.exclude": {
    "**/.git": true,
    "**/.svn": true,
    "**/.DS_Store": true,
    "**/.idea": true
  },
  "editor.formatOnSave": false,
  "editor.fontLigatures": true,
  "editor.minimap.enabled": true,
  "editor.renderIndentGuides": false,
  "editor.occurrencesHighlight": false,
  "editor.tabSize": 2,
  "editor.fontSize": 18,
  "editor.minimap.maxColumn": 120,
  "editor.rulers": [100],
  "eslint.autoFixOnSave": true,
  "eslint.validate": [
    "javascript",
    {
      "language": "vue",
      "autoFix": true
    },
    "html",
    "vue"
  ],
  "prettier.singleQuote": true,
  "prettier.semi": false,
  "prettier.eslintIntegration": true,
  "search.exclude": {
    "**/node_modules": true,
    "**/bower_components": true,
    "**/dist": true,
    "**/coverage": true,
    "**/doc": true
  },
  "git.confirmSync": false,
  "explorer.confirmDelete": false,
  "explorer.confirmDragAndDrop": false,
  "vetur.format.defaultFormatter.html": "js-beautify-html",
  "vetur.format.defaultFormatter.js": "prettier-eslint",
  "emmet.includeLanguages": {
    "javascript" : "javascriptreact"
  },
  "emmet.triggerExpansionOnTab": true
}
```