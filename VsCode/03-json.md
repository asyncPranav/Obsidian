
```json
{
  // ============================================
  // WINDOW & LAYOUT SETTINGS
  // ============================================
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
  
  // NEW - Additional Layout Polish
  "workbench.editor.tabCloseButton": "right", // Close button position
  "workbench.editor.tabSizing": "shrink", // Better tab sizing when many open
  "workbench.tree.indent": 16, // Sidebar tree indentation (default 8)
  "workbench.tree.renderIndentGuides": "always", // Show indent guides in sidebar
  "breadcrumbs.enabled": true, // File path breadcrumbs at top
  "breadcrumbs.symbolPath": "last", // Show only last symbol in breadcrumbs
  "explorer.decorations.badges": true, // Show badges on files (git status, etc)
  "explorer.decorations.colors": true, // Color decorations
  "explorer.compactFolders": true, // Compact single-child folders

  // ============================================
  // EDITOR APPEARANCE
  // ============================================
  "editor.lineHeight": 2,
  "editor.fontLigatures": true,
  "editor.minimap.enabled": false,
  "editor.fontWeight": 450,
  "editor.fontVariations": false,
  "editor.fontFamily": "'JetBrains Mono', 'Dank Mono', Consolas, 'Courier New', monospace",
  "editor.fontSize": 14.7,
  "editor.tabSize": 2,
  "editor.largeFileOptimizations": false,
  
  // NEW - More Editor Polish
  "editor.smoothScrolling": true, // Smooth scrolling animation
  "editor.renderWhitespace": "selection", // Show spaces/tabs only when selected
  "editor.guides.bracketPairs": "active", // Highlight active bracket pairs
  "editor.guides.indentation": true, // Show indentation guides
  "editor.guides.highlightActiveIndentation": true, // Highlight current indent level
  "editor.bracketPairColorization.enabled": true, // Color matching brackets
  "editor.stickyScroll.enabled": true, // Keep function/class names visible when scrolling
  "editor.padding.top": 12, // Space at top of editor
  "editor.padding.bottom": 12, // Space at bottom of editor
  "editor.rulers": [], // Vertical ruler lines (e.g., [80, 120] for columns)
  "editor.wordWrap": "off", // Text wrapping (or "on" if preferred)
  "editor.wrappingIndent": "indent", // How wrapped lines are indented
  "editor.letterSpacing": 0, // Letter spacing (0.5 for more space, -0.5 for tighter)
  "editor.suggest.preview": true, // Preview suggestion before accepting
  "editor.inlineSuggest.enabled": true, // Inline AI suggestions
  "editor.hover.delay": 300, // Hover tooltip delay in ms
  "editor.hover.sticky": true, // Hover stays when moving mouse to it
  "editor.parameterHints.enabled": true, // Show parameter hints

  // ============================================
  // EDITOR CURSOR & SCROLLBAR
  // ============================================
  "editor.cursorSmoothCaretAnimation": "on",
  "editor.cursorBlinking": "expand",
  "editor.cursorWidth": 2, // Cursor thickness (1-6)
  "editor.cursorStyle": "line", // line, block, underline, line-thin, block-outline, underline-thin
  "editor.scrollbar.horizontalScrollbarSize": 8, // Thinner horizontal scrollbar
  "editor.scrollbar.verticalScrollbarSize": 8, // Thinner vertical scrollbar
  "editor.scrollbar.horizontal": "auto", // Auto-hide horizontal scrollbar
  "editor.scrollbar.vertical": "auto", // Auto-hide vertical scrollbar
  "editor.renderLineHighlight": "none", // removes rectangular highlight
  "editor.lineNumbers": "on", // always show absolute line numbers
  "editor.glyphMargin": true, // Space for breakpoints/decorations (true/false)
  "editor.folding": true, // Code folding arrows
  "editor.foldingHighlight": true, // Highlight folded regions
  "editor.showFoldingControls": "mouseover", // Show fold controls on hover only
  
  // NEW - List & Sidebar Scrolling
  "workbench.list.smoothScrolling": true, // Smooth scrolling in sidebar/lists

  // ============================================
  // COLOR CUSTOMIZATIONS
  // ============================================
  "workbench.colorTheme": "Default Dark Modern", // Explicit theme name
  "workbench.colorCustomizations": {
    // Editor Colors
    "editor.background": "#191919",
    "editorCursor.foreground": "#FFD700",
    "editorLineNumber.foreground": "#555555",
    "editorLineNumber.activeForeground": "#FFD700",
    
    // NEW - Additional Color Customizations
    "editorWhitespace.foreground": "#333333", // Whitespace dots/arrows color
    "editorIndentGuide.background1": "#2a2a2a", // Indent guide lines
    "editorIndentGuide.activeBackground1": "#3a3a3a", // Active indent guide
    "editorRuler.foreground": "#2a2a2a", // Ruler lines color
    
    // Bracket Pair Colors (if bracketPairColorization enabled)
    "editorBracketHighlight.foreground1": "#FFD700",
    "editorBracketHighlight.foreground2": "#DA70D6",
    "editorBracketHighlight.foreground3": "#87CEFA",
    
    // Scrollbar
    "scrollbarSlider.background": "#ffffff15",
    "scrollbarSlider.hoverBackground": "#ffffff25",
    "scrollbarSlider.activeBackground": "#ffffff35",
    
    // Sidebar & Panel
    "sideBar.background": "#1a1a1a",
    "sideBar.border": "#2a2a2a",
    "sideBarTitle.foreground": "#cccccc",
    "sideBarSectionHeader.background": "#1f1f1f",
    "sideBarSectionHeader.foreground": "#cccccc",
    "panel.background": "#191919",
    "panel.border": "#2a2a2a",
    "panelTitle.activeForeground": "#FFD700",
    "panelTitle.inactiveForeground": "#888888",
    
    // Terminal
    "terminal.background": "#191919",
    "terminal.foreground": "#cccccc",
    "terminal.border": "#2a2a2a",
    
    // Tabs
    "editorGroupHeader.tabsBackground": "#191919",
    "tab.activeBackground": "#1f1f1f",
    "tab.activeForeground": "#ffffff",
    "tab.inactiveBackground": "#191919",
    "tab.inactiveForeground": "#888888",
    "tab.border": "#191919",
    "tab.activeBorder": "#FFD700", // Underline for active tab
    "tab.activeBorderTop": "#FFD700", // Top border for active tab
    "tab.hoverBackground": "#1f1f1f",
    "tab.unfocusedActiveForeground": "#cccccc",
    
    // Activity Bar (top bar with icons)
    "activityBar.background": "#191919",
    "activityBar.foreground": "#cccccc",
    "activityBar.activeBorder": "#FFD700",
    "activityBar.inactiveForeground": "#666666",
    
    // Status Bar
    "statusBar.background": "#191919",
    "statusBar.foreground": "#888888",
    "statusBar.border": "#2a2a2a",
    "statusBar.noFolderBackground": "#191919",
    
    // Title Bar
    "titleBar.activeBackground": "#191919",
    "titleBar.activeForeground": "#cccccc",
    "titleBar.inactiveBackground": "#191919",
    "titleBar.inactiveForeground": "#666666",
    "titleBar.border": "#2a2a2a",
    
    // Breadcrumbs
    "breadcrumb.background": "#191919",
    "breadcrumb.foreground": "#888888",
    "breadcrumb.focusForeground": "#FFD700",
    "breadcrumb.activeSelectionForeground": "#FFD700",
    
    // Input Fields
    "input.background": "#1f1f1f",
    "input.border": "#2a2a2a",
    "input.foreground": "#cccccc",
    "input.placeholderForeground": "#666666",
    
    // Dropdown
    "dropdown.background": "#1f1f1f",
    "dropdown.border": "#2a2a2a",
    "dropdown.foreground": "#cccccc",
    
    // List & Trees
    "list.activeSelectionBackground": "#2a2a2a",
    "list.activeSelectionForeground": "#FFD700",
    "list.hoverBackground": "#1f1f1f",
    "list.inactiveSelectionBackground": "#1f1f1f",
    "list.focusBackground": "#2a2a2a",
    "list.focusForeground": "#FFD700",
    
    // Selection
    "editor.selectionBackground": "#264f78",
    "editor.selectionHighlightBackground": "#264f7850",
    "editor.findMatchBackground": "#515c6a",
    "editor.findMatchHighlightBackground": "#515c6a50",
    
    // Minimap (if you enable it)
    "minimap.background": "#191919",
    "minimap.selectionHighlight": "#264f7880",
    "minimap.findMatchHighlight": "#515c6a80",
  },
  "glassit.alpha": 220,

  // ============================================
  // FILES & AUTO SAVE
  // ============================================
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 100,
  
  // NEW - File Management
  "files.trimTrailingWhitespace": true, // Remove trailing spaces on save
  "files.insertFinalNewline": true, // Add newline at end of file
  "files.trimFinalNewlines": true, // Remove extra newlines at end
  "files.exclude": { // Hide files from explorer (optional)
    "**/.git": true,
    "**/.DS_Store": true,
    "**/node_modules": true,
  },

  // ============================================
  // CODE FORMATTING
  // ============================================
  "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features",
    "editor.formatOnSave": true, // Auto-format HTML on save
  },
  "[css]": {
    "editor.defaultFormatter": "vscode.css-language-features",
    "editor.formatOnSave": true, // Auto-format CSS on save
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true, // Auto-format JS on save
  },
  "[javascriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true, // Auto-format JSX on save
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true, // Auto-format JSON on save
  },
  "[jsonc]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true, // Auto-format JSONC on save
  },

  // ============================================
  // PRETTIER SETTINGS
  // ============================================
  "prettier.tabWidth": 2,
  "prettier.singleQuote": true, // Use single quotes
  "prettier.semi": true, // Add semicolons
  "prettier.trailingComma": "es5", // Trailing commas where valid in ES5
  "prettier.printWidth": 80, // Line length before wrapping
  "prettier.arrowParens": "avoid", // Avoid parens when possible in arrow functions

  // ============================================
  // JAVASCRIPT SETTINGS
  // ============================================
  "javascript.suggest.completeFunctionCalls": true,
  "javascript.updateImportsOnFileMove.enabled": "never",
  "javascript.inlayHints.parameterNames.enabled": "all", // Show parameter names inline
  "javascript.inlayHints.functionLikeReturnTypes.enabled": true, // Show return types

  // ============================================
  // TERMINAL SETTINGS
  // ============================================
  "terminal.integrated.defaultProfile.windows": "Git Bash",
  "terminal.integrated.shellIntegration.enabled": false,
  "terminal.integrated.suggest.enabled": true,
  "terminal.integrated.copyOnSelection": true,
  "terminal.integrated.cursorBlinking": true,
  "terminal.integrated.cursorStyle": "line",
  "terminal.integrated.cursorStyleInactive": "block",
  "terminal.integrated.fontSize": 13.5,
  "terminal.integrated.fontWeight": "normal",
  "terminal.integrated.padding": 8, // Padding around terminal content
  "terminal.integrated.smoothScrolling": true, // Smooth scrolling in terminal
  "terminal.integrated.lineHeight": 1.2, // Terminal line height
  "terminal.integrated.letterSpacing": 0, // Letter spacing in terminal
  "terminal.integrated.fontFamily": "JetBrains Mono, Consolas, monospace", // Terminal font
  "terminal.integrated.suggest.quickSuggestions": {
    "commands": "on",
    "arguments": "on",
    "unknown": "off",
  },
  "terminal.integrated.fontLigatures.fallbackLigatures": [
    "<--", "<---", "<<-", "<-", "->", "->>", "-->", "--->",
    "<==", "<===", "<<=", "<=", "=>", "=>>", "==>", "===>",
    ">=", ">>=", "<->", "<-->", "<--->", "<---->",
    "<=>", "<==>", "<===>", "<====>",
    "::", ":::", "<", "</", "</>", "/>", ">",
    "==", "!=", "/=", "~=", "<>", "===", "!==", "!===",
    "<:", ":=", "*=", "*+", "<*", "<*>", "*>",
    "<|", "<|>", "|>", "+*", "=*", "=:", ":>",
    "/*", "*/", "+++", "<!--", "<!---",
  ],

  // ============================================
  // SEARCH SETTINGS
  // ============================================
  "search.showLineNumbers": true, // Show line numbers in search results
  "search.smartCase": true, // Smart case sensitivity
  "search.exclude": { // Exclude from search
    "**/node_modules": true,
    "**/dist": true,
    "**/build": true,
  },

  // ============================================
  // EXTENSION SETTINGS - Code Runner
  // ============================================
  "code-runner.runInTerminal": true,
  "code-runner.clearPreviousOutput": true, // Clear before running

  // ============================================
  // EXTENSION SETTINGS - Live Server
  // ============================================
  "liveServer.settings.donotShowInfoMsg": true,
  "liveServer.settings.donotVerifyTags": true,

  // ============================================
  // EXTENSION SETTINGS - Tabnine
  // ============================================
  "tabnine.experimentalAutoImports": true,

  // ============================================
  // EXTENSION SETTINGS - Rainbow Brackets
  // ============================================
  "RainbowBrackets.depreciation-notice": false,

  // ============================================
  // SCREENCAST MODE
  // ============================================
  "screencastMode.verticalOffset": 10,
  "screencastMode.fontSize": 56, // Font size in screencast mode
  "screencastMode.keyboardOptions": {
    "showKeys": true,
    "showKeybindings": true,
    "showCommands": true,
  },

  // ============================================
  // ZEN MODE
  // ============================================
  "zenMode.centerLayout": true, // Center editor in zen mode
  "zenMode.fullScreen": false, // Don't go fullscreen
  "zenMode.hideLineNumbers": false, // Keep line numbers
  "zenMode.restore": true, // Restore zen mode on restart
  "zenMode.silentNotifications": true, // Hide notifications in zen mode
}
```