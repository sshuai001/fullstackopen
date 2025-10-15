# 0.4: New note diagram

```mermaid
sequenceDiagram
    participant Browser
    participant Server

    %% 用户在 /notes 页面上输入 note 内容，并点击 “保存 / 提交” 按钮
    Browser->>Server: POST /exampleapp/new_note
    activate Server
    Server-->>Browser: 302 Redirect (Location: /exampleapp/notes)
    deactivate Server

    %% 浏览器收到重定向，自动发起新的 GET /notes 请求
    Browser->>Server: GET /exampleapp/notes
    activate Server
    Server-->>Browser: HTML document for /notes
    deactivate Server

    %% 浏览器解析 HTML，识别资源引用，继续请求 CSS, JS
    Browser->>Server: GET /exampleapp/main.css
    activate Server
    Server-->>Browser: CSS file
    deactivate Server

    Browser->>Server: GET /exampleapp/main.js
    activate Server
    Server-->>Browser: JavaScript file
    deactivate Server

    Note right of Browser: 浏览器执行 main.js 的脚本逻辑

    %% JS 脚本中会发起 AJAX 请求获取最新 notes
    Browser->>Server: GET /exampleapp/data.json
    activate Server
    Server-->>Browser: JSON array of notes
    deactivate Server

    Note right of Browser: 脚本接收到 JSON 后，将其渲染到页面 DOM 上
```

# 0.5: Single page app diagram

```mermaid
sequenceDiagram
    participant Browser
    participant Server

    %% User navigates to the SPA URL
    Browser->>Server: GET /exampleapp/spa
    activate Server
    Server-->>Browser: HTML + JS bundle for SPA
    deactivate Server

    %% Browser parses HTML and executes main.js
    Note right of Browser: Browser executes SPA JS logic

    %% SPA JS requests existing notes via AJAX
    Browser->>Server: GET /exampleapp/data.json
    activate Server
    Server-->>Browser: JSON array of notes
    deactivate Server

    Note right of Browser: Browser renders notes dynamically into DOM

    %% User adds a new note
    Browser->>Server: POST /exampleapp/new_note (AJAX)
    activate Server
    Server-->>Browser: JSON array of updated notes
    deactivate Server

    Note right of Browser: SPA updates DOM without full page reload
```

# 0.6: New note in Single page app diagram

```mermaid
sequenceDiagram
    participant Browser
    participant Server

    %% 用户在 SPA 中输入新笔记并点击“保存”
    Browser->>Server: POST /exampleapp/new_note (AJAX)
    activate Server
    Server-->>Browser: JSON array with updated notes
    deactivate Server

    Note right of Browser: SPA updates DOM to show the new note dynamically
```
