# VSCode

---

## plugin
* `vscode-icons` *图标*
* `guides` *铺助缩进线*
* `PostCSS Sorting`
* `stylelint`
* `stylefmt`
* `ESLint`
* `javascript standard format`
* `beautify`
* `Babel ES6/ES7`
* `Debugger for Chrome`
* `Add jsdoc comments`
* `javascript(ES6) code snippets`
* `vue`
* `weex`
* `Reactjs code snippets`
* `React Native Tools`
* `Npm Intellisense`
* `Instant Markdown`
* `Markdown Shortcuts`
* `TextTransform`
* `view in browser`

### UserSetting
``` json
{
    "window.zoomLevel": 0,

    "workbench.colorTheme": "Solarized Dark",
    "workbench.iconTheme": "vscode-icons",

    "files.autoSave": "onWindowChange",
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

    "editor.fontFamily": "'Fira Code'",
    "editor.formatOnSave": true,
    "editor.fontLigatures": true,
    "editor.minimap.enabled": true,
    "editor.renderIndentGuides": false,
    "editor.occurrencesHighlight": false,
    "editor.tabSize": 2,
    "editor.fontSize": 22,
    "editor.minimap.maxColumn": 120,
    "editor.rulers": [
        100
    ],

    "eslint.validate": [
        "javascript",
        "javascriptreact",
        "html",
        "vue"
    ],
    "eslint.options": {
        "plugins": ["html"]
    },

    "search.exclude": {
        "**/node_modules": true,
        "**/bower_components": true,
        "**/dist": true,
        "**/coverage": true,
        "**/doc": true
    },
    "git.confirmSync": false,
}
```