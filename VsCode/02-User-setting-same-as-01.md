

---


```json
{
  // WINDOW & LAYOUT SETTINGS
  "workbench.sideBar.location": "right",
  "window.zoomLevel": 0,
  "window.density.editorTabHeight": "compact",
  "window.restoreWindows": "all",
  "window.commandCenter": false,
  "window.menuBarVisibility": "compact",
  "workbench.startupEditor": "none",
  "workbench.layoutControl.enabled": false,
  "workbench.activityBar.location": "top",
  "workbench.secondarySideBar.defaultVisibility": "hidden",
  "workbench.editor.showTabs": "multiple",
  "workbench.iconTheme": "material-icon-theme",

  // EDITOR APPEARANCE
  "editor.lineHeight": 2,
  "editor.fontLigatures": true,
  "editor.minimap.enabled": false,
  "editor.fontWeight": 450,
  "editor.fontVariations": false,
  "editor.fontFamily": "'JetBrains Mono', 'Dank Mono', Consolas, 'Courier New', monospace",
  "editor.fontSize": 14.7,
  "editor.tabSize": 2,
  "editor.largeFileOptimizations": false,

  // EDITOR CURSOR & SCROLLBAR
  "editor.cursorStyle": "line",
  "editor.cursorSmoothCaretAnimation": "on",
  "editor.cursorBlinking": "smooth",
  "editor.scrollbar.horizontalScrollbarSize": 10,
  "editor.scrollbar.verticalScrollbarSize": 10,
  "editor.renderLineHighlight": "none",
  "editor.lineNumbers": "on",

  // COLOR CUSTOMIZATIONS
  "workbench.colorCustomizations": {
    "editor.background": "#191919",
    "editorCursor.foreground": "#FFD700",
    "editorLineNumber.foreground": "#555555",
    "editorLineNumber.activeForeground": "#FFD700"
  },
  "glassit.alpha": 220,

  // FILES & AUTO SAVE
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 100,

  // CODE FORMATTING - HTML
  "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features"
  },
  "[css]": {
    "editor.defaultFormatter": "vscode.css-language-features"
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[jsonc]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },

  // PRETTIER SETTINGS
  "prettier.tabWidth": 2,

  // JAVASCRIPT SETTINGS
  "javascript.suggest.completeFunctionCalls": true,
  "javascript.updateImportsOnFileMove.enabled": "never",

  // TERMINAL SETTINGS
  "terminal.integrated.defaultProfile.windows": "Git Bash",
  "terminal.integrated.shellIntegration.enabled": false,
  "terminal.integrated.suggest.enabled": true,
  "terminal.integrated.copyOnSelection": true,
  "terminal.integrated.cursorBlinking": true,
  "terminal.integrated.cursorStyle": "line",
  "terminal.integrated.cursorStyleInactive": "block",
  "terminal.integrated.fontSize": 13.5,
  "terminal.integrated.fontWeight": "normal",
  "terminal.integrated.suggest.quickSuggestions": {
    "commands": "on",
    "arguments": "on",
    "unknown": "off"
  },
  "terminal.integrated.fontLigatures.fallbackLigatures": [
    "<--","<---","<<-","<-","->","->>","-->","--->",
    "<==","<===","<<=","<=","=>","=>>","==>","===>",
    ">=",">>=","<->","<-->","<--->","<---->","<=>",
    "<==>","<===>","<====>", "::",":::","<","</","</>","/>",
    ">","==","!=","/=","~=","<>","===","!==","!===",
    "<:",":=","*=","*+","<*","<*>","*>","<|","<|>",
    "|>", "+*","=*","=:"," :>","/*","*/","+++","<!--","<!---"
  ],

  // EXTENSION SETTINGS - Code Runner
  "code-runner.runInTerminal": true,

  // EXTENSION SETTINGS - Live Server
  "liveServer.settings.donotShowInfoMsg": true,
  "liveServer.settings.donotVerifyTags": true,

  // EXTENSION SETTINGS - Tabnine
  "tabnine.experimentalAutoImports": true,

  // EXTENSION SETTINGS - Rainbow Brackets
  "RainbowBrackets.depreciation-notice": false,

  // SCREENCAST MODE
  "screencastMode.verticalOffset": 10
}

