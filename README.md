<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🤖 Kanishk AI - Advanced Chatbot</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.8.0/styles/atom-one-dark.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.8.0/highlight.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --primary: #667eea;
            --secondary: #764ba2;
            --accent: #f093fb;
            --light: #f7f7f7;
            --dark: #333;
            --darker: #1a1a1a;
            --border: #e0e0e0;
            --success: #4caf50;
            --error: #f44336;
            --warning: #ff9800;
            --info: #2196f3;
        }

        html {
            scroll-behavior: smooth;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            min-height: 100vh;
            padding: 10px;
            transition: all 0.3s ease;
            color: var(--dark);
        }

        body.dark-mode {
            --light: #1a1a1a;
            --dark: #fff;
            --border: #444;
            background: linear-gradient(135deg, #0f0f1e 0%, #1a1a2e 100%);
        }

        /* ===== LOADING SPLASH ===== */
        .splash-screen {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 9999;
            flex-direction: column;
            gap: 20px;
        }

        .splash-screen.hidden {
            display: none;
        }

        .splash-logo {
            font-size: 60px;
            animation: bounce 1s ease-in-out infinite;
        }

        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-20px); }
        }

        .splash-text {
            color: white;
            font-size: 24px;
            font-weight: bold;
            text-align: center;
        }

        .splash-loader {
            width: 40px;
            height: 40px;
            border: 4px solid rgba(255,255,255,0.3);
            border-top: 4px solid white;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        /* ===== AUTH SECTION ===== */
        .auth-container {
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            z-index: 2000;
            opacity: 1;
            visibility: visible;
            transition: opacity 0.3s, visibility 0.3s;
            padding: 20px;
        }

        .auth-container.hidden {
            opacity: 0;
            visibility: hidden;
            pointer-events: none;
        }

        .auth-box {
            background: white;
            padding: 50px 40px;
            border-radius: 20px;
            box-shadow: 0 30px 80px rgba(0,0,0,0.4);
            width: 100%;
            max-width: 450px;
            animation: slideUp 0.5s ease;
        }

        body.dark-mode .auth-box {
            background: var(--darker);
            color: white;
        }

        @keyframes slideUp {
            from {
                transform: translateY(50px);
                opacity: 0;
            }
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }

        .auth-header {
            text-align: center;
            margin-bottom: 30px;
        }

        .auth-header .logo {
            font-size: 50px;
            margin-bottom: 10px;
        }

        .auth-header h2 {
            color: var(--primary);
            font-size: 32px;
            margin-bottom: 5px;
        }

        .auth-header p {
            color: #999;
            font-size: 14px;
        }

        .auth-group {
            margin-bottom: 20px;
        }

        .auth-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: var(--dark);
            font-size: 14px;
        }

        .auth-group input {
            width: 100%;
            padding: 12px 16px;
            border: 2px solid var(--border);
            border-radius: 10px;
            font-size: 14px;
            background: white;
            color: var(--dark);
            transition: all 0.3s;
            font-family: inherit;
        }

        body.dark-mode .auth-group input {
            background: #2a2a2a;
            color: white;
            border-color: #555;
        }

        .auth-group input:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 10px rgba(102, 126, 234, 0.3);
        }

        .auth-group input::placeholder {
            color: #999;
        }

        .auth-btn {
            width: 100%;
            padding: 14px;
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 10px;
            transition: all 0.3s;
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.3);
        }

        .auth-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(102, 126, 234, 0.5);
        }

        .auth-btn:active {
            transform: translateY(0);
        }

        .auth-btn:disabled {
            background: #ccc;
            cursor: not-allowed;
        }

        .auth-error {
            background: #ffebee;
            color: #c62828;
            padding: 12px 16px;
            border-radius: 8px;
            margin-bottom: 15px;
            font-size: 13px;
            border-left: 4px solid #c62828;
            animation: slideDown 0.3s ease;
        }

        @keyframes slideDown {
            from {
                transform: translateY(-10px);
                opacity: 0;
            }
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }

        .auth-success {
            background: #e8f5e9;
            color: #2e7d32;
            padding: 12px 16px;
            border-radius: 8px;
            margin-bottom: 15px;
            font-size: 13px;
            border-left: 4px solid #2e7d32;
            animation: slideDown 0.3s ease;
        }

        .auth-info {
            background: #e3f2fd;
            padding: 12px 16px;
            border-radius: 8px;
            font-size: 12px;
            color: #1565c0;
            margin-bottom: 20px;
            border-left: 4px solid #1565c0;
        }

        .auth-toggle {
            text-align: center;
            color: #666;
            font-size: 14px;
            margin-top: 20px;
        }

        .auth-toggle button {
            background: none;
            border: none;
            color: var(--primary);
            cursor: pointer;
            font-weight: bold;
            text-decoration: underline;
            margin-left: 5px;
        }

        .divider {
            text-align: center;
            margin: 20px 0;
            color: #999;
            font-size: 12px;
        }

        .divider::before,
        .divider::after {
            content: '';
            display: inline-block;
            width: 40%;
            height: 1px;
            background: var(--border);
            vertical-align: middle;
            margin: 0 5px;
        }

        /* ===== SETTINGS MODAL ===== */
        .settings-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0,0,0,0.7);
            z-index: 3000;
            align-items: center;
            justify-content: center;
            animation: fadeIn 0.3s ease;
        }

        .settings-modal.active {
            display: flex;
        }

        .settings-content {
            background: white;
            padding: 30px;
            border-radius: 15px;
            max-width: 500px;
            width: 90%;
            max-height: 80vh;
            overflow-y: auto;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
        }

        body.dark-mode .settings-content {
            background: var(--darker);
            color: white;
        }

        .settings-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 2px solid var(--border);
        }

        .settings-header h2 {
            font-size: 24px;
            color: var(--primary);
        }

        .settings-close {
            background: none;
            border: none;
            font-size: 24px;
            cursor: pointer;
            color: var(--dark);
        }

        .settings-group {
            margin-bottom: 20px;
        }

        .settings-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            font-size: 14px;
        }

        .settings-group input,
        .settings-group select {
            width: 100%;
            padding: 10px;
            border: 2px solid var(--border);
            border-radius: 8px;
            font-size: 14px;
            background: white;
            color: var(--dark);
        }

        body.dark-mode .settings-group input,
        body.dark-mode .settings-group select {
            background: #2a2a2a;
            color: white;
            border-color: #555;
        }

        .settings-group input:focus,
        .settings-group select:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 10px rgba(102, 126, 234, 0.3);
        }

        .settings-warning {
            background: #fff3cd;
            color: #856404;
            padding: 12px;
            border-radius: 8px;
            font-size: 12px;
            border-left: 4px solid #ffc107;
            margin-bottom: 15px;
        }

        .settings-buttons {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-top: 20px;
        }

        .settings-buttons button {
            padding: 10px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.2s;
        }

        .settings-buttons .save-btn {
            background: var(--success);
            color: white;
        }

        .settings-buttons .save-btn:hover {
            background: #388e3c;
        }

        .settings-buttons .cancel-btn {
            background: var(--border);
            color: var(--dark);
        }

        .settings-buttons .cancel-btn:hover {
            background: #ccc;
        }

        /* ===== MAIN WRAPPER ===== */
        .wrapper {
            display: flex;
            height: 100vh;
            gap: 10px;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.3s, visibility 0.3s;
        }

        .wrapper.active {
            opacity: 1;
            visibility: visible;
        }

        /* ===== SIDEBAR ===== */
        .sidebar {
            width: 320px;
            background: white;
            border-radius: 15px;
            display: flex;
            flex-direction: column;
            box-shadow: 0 10px 40px rgba(0,0,0,0.15);
            overflow: hidden;
            transition: all 0.3s;
        }

        body.dark-mode .sidebar {
            background: #2a2a2a;
            color: white;
        }

        .sidebar-header {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            color: white;
            padding: 25px 20px;
            text-align: center;
        }

        .sidebar-logo {
            font-size: 32px;
            margin-bottom: 8px;
        }

        .sidebar-header h2 {
            font-size: 18px;
            margin-bottom: 5px;
            letter-spacing: 0.5px;
        }

        .sidebar-header p {
            font-size: 11px;
            opacity: 0.9;
        }

        .new-chat-btn {
            background: white;
            color: var(--primary);
            border: none;
            padding: 12px 20px;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
            width: calc(100% - 20px);
            font-size: 14px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
            margin: 0 10px;
        }

        .new-chat-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }

        .sidebar-content {
            flex: 1;
            overflow-y: auto;
            padding: 15px;
            background: var(--light);
        }

        body.dark-mode .sidebar-content {
            background: #1a1a1a;
        }

        .chat-item {
            padding: 12px 15px;
            margin-bottom: 8px;
            background: white;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 13px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
            border-left: 4px solid transparent;
        }

        body.dark-mode .chat-item {
            background: #333;
            color: white;
        }

        .chat-item:hover {
            background: #f5f5f5;
            transform: translateX(5px);
        }

        body.dark-mode .chat-item:hover {
            background: #404040;
        }

        .chat-item.active {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%));
            color: white;
            border-left-color: white;
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.3);
        }

        .chat-item-name {
            flex: 1;
            word-break: break-word;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .chat-item .delete {
            background: var(--error);
            color: white;
            border: none;
            padding: 6px 10px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 11px;
            display: none;
            transition: all 0.2s;
            margin-left: 8px;
            flex-shrink: 0;
        }

        .chat-item .delete:hover {
            background: #d32f2f;
            transform: scale(1.1);
        }

        .chat-item:hover .delete {
            display: block;
        }

        .sidebar-footer {
            padding: 15px;
            border-top: 1px solid var(--border);
            background: #f9f9f9;
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(70px, 1fr));
            gap: 8px;
        }

        body.dark-mode .sidebar-footer {
            background: #1a1a1a;
            border-color: #444;
        }

        .sidebar-footer button {
            padding: 8px 12px;
            background: var(--primary);
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 12px;
            transition: all 0.2s;
            font-weight: 500;
        }

        .sidebar-footer button:hover {
            background: var(--secondary);
            transform: translateY(-2px);
        }

        .sidebar-footer button.danger {
            background: var(--error);
        }

        .sidebar-footer button.danger:hover {
            background: #d32f2f;
        }

        .sidebar-footer button.warning {
            background: var(--warning);
        }

        .sidebar-footer button.warning:hover {
            background: #f57c00;
        }

        .user-info {
            grid-column: 1 / -1;
            font-size: 12px;
            color: #666;
            text-align: center;
            padding: 10px 0;
            border-top: 1px solid var(--border);
            margin-top: 10px;
        }

        body.dark-mode .user-info {
            color: #aaa;
            border-color: #444;
        }

        /* ===== MAIN CHAT AREA ===== */
        .main {
            flex: 1;
            display: flex;
            flex-direction: column;
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.15);
            overflow: hidden;
            transition: all 0.3s;
        }

        body.dark-mode .main {
            background: #2a2a2a;
            color: white;
        }

        .chat-header {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%));
            color: white;
            padding: 20px 25px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid rgba(0,0,0,0.1);
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }

        .chat-header-left h1 {
            font-size: 22px;
            margin-bottom: 5px;
            font-weight: 600;
        }

        .chat-header-left p {
            font-size: 12px;
            opacity: 0.9;
            letter-spacing: 0.3px;
        }

        .chat-header-right {
            display: flex;
            gap: 10px;
        }

        .header-btn {
            background: rgba(255,255,255,0.2);
            color: white;
            border: 2px solid white;
            padding: 8px 14px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 12px;
            transition: all 0.3s;
            font-weight: 500;
        }

        .header-btn:hover {
            background: rgba(255,255,255,0.3);
            transform: scale(1.05);
        }

        .chat-messages {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            background: var(--light);
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        body.dark-mode .chat-messages {
            background: #1a1a1a;
        }

        .message {
            display: flex;
            animation: slideIn 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
            max-width: 80%;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(15px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .message.user {
            justify-content: flex-end;
            margin-left: auto;
        }

        .message.user .text {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%));
            color: white;
            border-radius: 18px 18px 4px 18px;
            padding: 14px 18px;
            font-size: 14px;
            box-shadow: 0 3px 10px rgba(102, 126, 234, 0.3);
            word-wrap: break-word;
            line-height: 1.4;
        }

        .message.ai {
            justify-content: flex-start;
        }

        .message.ai .text {
            background: #e0e0e0;
            color: var(--dark);
            border-radius: 18px 18px 18px 4px;
            padding: 14px 18px;
            font-size: 14px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.05);
            word-wrap: break-word;
            line-height: 1.5;
            max-width: 100%;
        }

        body.dark-mode .message.ai .text {
            background: #333;
            color: white;
        }

        .message.system {
            justify-content: center;
            max-width: 100%;
        }

        .message.system .text {
            background: #fff3cd;
            color: #856404;
            border-radius: 12px;
            padding: 12px 18px;
            font-size: 12px;
            border-left: 4px solid #ffc107;
        }

        .message-text {
            position: relative;
        }

        .message-actions {
            display: flex;
            gap: 5px;
            margin-top: 8px;
            opacity: 0;
            transition: opacity 0.2s;
        }

        .message:hover .message-actions {
            opacity: 1;
        }

        .message-actions button {
            background: rgba(0,0,0,0.1);
            border: none;
            color: var(--dark);
            padding: 4px 8px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 11px;
            transition: all 0.2s;
        }

        body.dark-mode .message-actions button {
            background: rgba(255,255,255,0.1);
            color: white;
        }

        .message-actions button:hover {
            background: rgba(0,0,0,0.2);
            transform: scale(1.1);
        }

        /* Markdown Rendering */
        .markdown-content {
            display: block;
        }

        .markdown-content h1,
        .markdown-content h2,
        .markdown-content h3,
        .markdown-content h4,
        .markdown-content h5,
        .markdown-content h6 {
            margin: 10px 0 5px 0;
            font-weight: 600;
        }

        .markdown-content h1 { font-size: 20px; }
        .markdown-content h2 { font-size: 18px; }
        .markdown-content h3 { font-size: 16px; }
        .markdown-content h4 { font-size: 15px; }

        .markdown-content p {
            margin: 5px 0;
        }

        .markdown-content ul,
        .markdown-content ol {
            margin-left: 20px;
            margin-top: 5px;
            margin-bottom: 5px;
        }

        .markdown-content li {
            margin-bottom: 3px;
        }

        .markdown-content code {
            background: rgba(0,0,0,0.1);
            padding: 2px 6px;
            border-radius: 3px;
            font-family: 'Courier New', monospace;
            font-size: 12px;
        }

        body.dark-mode .markdown-content code {
            background: rgba(255,255,255,0.1);
        }

        .markdown-content pre {
            background: #1e1e1e;
            color: #d4d4d4;
            padding: 12px;
            border-radius: 8px;
            overflow-x: auto;
            margin: 10px 0;
            font-family: 'Courier New', monospace;
            font-size: 12px;
        }

        .markdown-content pre code {
            background: none;
            padding: 0;
            color: #d4d4d4;
        }

        .markdown-content blockquote {
            border-left: 4px solid var(--primary);
            padding-left: 10px;
            margin: 10px 0;
            color: #666;
            font-style: italic;
        }

        body.dark-mode .markdown-content blockquote {
            color: #aaa;
        }

        .markdown-content a {
            color: var(--primary);
            text-decoration: none;
        }

        .markdown-content a:hover {
            text-decoration: underline;
        }

        .markdown-content table {
            border-collapse: collapse;
            margin: 10px 0;
            width: 100%;
        }

        .markdown-content th,
        .markdown-content td {
            border: 1px solid var(--border);
            padding: 8px;
            text-align: left;
        }

        .markdown-content th {
            background: var(--light);
            font-weight: 600;
        }

        body.dark-mode .markdown-content table {
            color: white;
        }

        .loading {
            display: flex;
            gap: 6px;
            padding: 14px 18px;
            align-items: center;
        }

        .loading span {
            width: 10px;
            height: 10px;
            background: var(--primary);
            border-radius: 50%;
            animation: pulse 1.4s infinite;
        }

        .loading span:nth-child(2) {
            animation-delay: 0.2s;
        }

        .loading span:nth-child(3) {
            animation-delay: 0.4s;
        }

        @keyframes pulse {
            0%, 80%, 100% {
                opacity: 0.3;
                transform: scale(1);
            }
            40% {
                opacity: 1;
                transform: scale(1.2);
            }
        }

        .typing-indicator {
            font-size: 12px;
            color: #999;
            font-style: italic;
        }

        /* ===== STATS ===== */
        .stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
            gap: 12px;
            padding: 12px 20px;
            background: #f9f9f9;
            font-size: 12px;
            color: #666;
            border-top: 1px solid var(--border);
        }

        body.dark-mode .stats {
            background: #1a1a1a;
            color: #aaa;
            border-color: #444;
        }

        .stats span {
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 8px 12px;
            background: white;
            border-radius: 6px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
        }

        body.dark-mode .stats span {
            background: #2a2a2a;
        }

        .stats span strong {
            color: var(--primary);
            font-size: 14px;
        }

        /* ===== CONTROLS ===== */
        .chat-controls {
            display: flex;
            gap: 10px;
            padding: 12px 20px;
            background: #f9f9f9;
            border-top: 1px solid var(--border);
            flex-wrap: wrap;
            overflow-x: auto;
        }

        body.dark-mode .chat-controls {
            background: #1a1a1a;
            border-color: #444;
        }

        .chat-controls button {
            padding: 8px 14px;
            background: var(--primary);
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 12px;
            transition: all 0.2s;
            font-weight: 500;
            box-shadow: 0 2px 5px rgba(102, 126, 234, 0.2);
            white-space: nowrap;
            flex-shrink: 0;
        }

        .chat-controls button:hover {
            background: var(--secondary);
            transform: translateY(-2px);
            box-shadow: 0 4px 10px rgba(102, 126, 234, 0.3);
        }

        .chat-controls button.danger {
            background: var(--error);
        }

        .chat-controls button.danger:hover {
            background: #d32f2f;
        }

        .chat-controls button.warning {
            background: var(--warning);
        }

        .chat-controls button.warning:hover {
            background: #f57c00;
        }

        /* ===== INPUT AREA ===== */
        .input-area {
            padding: 15px 20px;
            background: white;
            border-top: 2px solid var(--border);
            display: flex;
            gap: 12px;
            align-items: flex-end;
        }

        body.dark-mode .input-area {
            background: #2a2a2a;
            border-color: #444;
        }

        .input-wrapper {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 5px;
        }

        .input-area input {
            flex: 1;
            border: 2px solid var(--border);
            border-radius: 12px;
            padding: 12px 16px;
            font-size: 14px;
            outline: none;
            background: white;
            color: var(--dark);
            transition: all 0.3s;
            font-family: inherit;
            resize: vertical;
            min-height: 44px;
            max-height: 120px;
        }

        body.dark-mode .input-area input {
            background: #333;
            color: white;
            border-color: #555;
        }

        .input-area input:focus {
            border-color: var(--primary);
            box-shadow: 0 0 10px rgba(102, 126, 234, 0.3);
        }

        .input-area input::placeholder {
            color: #999;
        }

        .input-actions {
            display: flex;
            gap: 8px;
            align-items: center;
        }

        .input-area button {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%));
            color: white;
            border: none;
            border-radius: 10px;
            width: 45px;
            height: 45px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            transition: all 0.3s;
            box-shadow: 0 3px 10px rgba(102, 126, 234, 0.3);
            flex-shrink: 0;
        }

        .input-area button:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }

        .input-area button:active {
            transform: translateY(-1px);
        }

        .input-area button:disabled {
            background: #ccc;
            cursor: not-allowed;
            box-shadow: none;
        }

        .char-count {
            font-size: 11px;
            color: #999;
            text-align: right;
        }

        /* ===== EMPTY STATE ===== */
        .empty-state {
            text-align: center;
            color: #999;
            padding: 50px 20px;
            font-size: 14px;
        }

        .empty-state .emoji {
            font-size: 50px;
            margin-bottom: 15px;
            display: block;
        }

        /* ===== SCROLLBAR ===== */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }

        ::-webkit-scrollbar-track {
            background: #f1f1f1;
        }

        body.dark-mode ::-webkit-scrollbar-track {
            background: #1a1a1a;
        }

        ::-webkit-scrollbar-thumb {
            background: var(--primary);
            border-radius: 4px;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: var(--secondary);
        }

        /* ===== RESPONSIVE ===== */
        @media (max-width: 1024px) {
            .sidebar {
                width: 280px;
            }

            .message {
                max-width: 85%;
            }
        }

        @media (max-width: 768px) {
            .wrapper {
                flex-direction: column;
            }

            .sidebar {
                width: 100%;
                height: auto;
                max-height: 180px;
                order: 2;
            }

            .main {
                order: 1;
            }

            .message {
                max-width: 90%;
            }

            .chat-header {
                flex-direction: column;
                gap: 10px;
                text-align: center;
            }

            .chat-header-right {
                width: 100%;
                justify-content: center;
                flex-wrap: wrap;
            }

            .stats {
                grid-template-columns: repeat(2, 1fr);
            }

            .chat-controls {
                flex-direction: column;
            }

            .chat-controls button {
                width: 100%;
            }
        }

        @media (max-width: 480px) {
            .auth-box {
                padding: 30px 20px;
            }

            .sidebar {
                max-height: 150px;
            }

            .message {
                max-width: 95%;
            }

            .input-area {
                padding: 10px 12px;
            }

            .input-area button {
                width: 40px;
                height: 40px;
                font-size: 16px;
            }

            .stats {
                grid-template-columns: 1fr;
                font-size: 11px;
            }

            .chat-header h1 {
                font-size: 18px;
            }
        }

        /* ===== ANIMATIONS ===== */
        .fade-in {
            animation: fadeIn 0.3s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .pulse-bg {
            animation: pulseBg 2s ease-in-out infinite;
        }

        @keyframes pulseBg {
            0%, 100% { background-color: transparent; }
            50% { background-color: rgba(102, 126, 234, 0.1); }
        }

        /* ===== UTILITY CLASSES ===== */
        .hidden {
            display: none !important;
        }

        .loading-overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0,0,0,0.7);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 3500;
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s;
        }

        .loading-overlay.active {
            opacity: 1;
            visibility: visible;
        }

        .spinner {
            border: 4px solid rgba(255,255,255,0.3);
            border-top: 4px solid white;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }
    </style>
</head>
<body>
    <!-- Splash Screen -->
    <div class="splash-screen" id="splashScreen">
        <div class="splash-logo">🤖</div>
        <div class="splash-text">Kanishk AI</div>
        <div class="splash-loader"></div>
    </div>

    <!-- Auth Container -->
    <div class="auth-container" id="authContainer">
        <div class="auth-box">
            <div class="auth-header">
                <div class="logo">🤖</div>
                <h2>Kanishk AI</h2>
                <p>Advanced Chatbot System</p>
            </div>

            <div id="authError"></div>

            <!-- Login Form -->
            <div id="loginForm">
                <div class="auth-info">
                    💡 Demo के लिए कोई भी username/password use कर सकते हो
                </div>

                <div class="auth-group">
                    <label>👤 Username</label>
                    <input type="text" id="loginUsername" placeholder="username डालो" autocomplete="username">
                </div>

                <div class="auth-group">
                    <label>🔐 Password</label>
                    <input type="password" id="loginPassword" placeholder="password डालो" autocomplete="current-password">
                </div>

                <button class="auth-btn" onclick="handleLogin()">🔓 Login करो</button>

                <div class="divider">या</div>

                <div class="auth-toggle">
                    Account नहीं है? <button onclick="toggleAuthForm()">Sign Up करो</button>
                </div>
            </div>

            <!-- Signup Form -->
            <div id="signupForm" style="display: none;">
                <div class="auth-info">
                    💡 नया account बनाओ और chat शुरू करो
                </div>

                <div class="auth-group">
                    <label>👤 Username (3+ characters)</label>
                    <input type="text" id="signupUsername" placeholder="username चुनो" autocomplete="username">
                </div>

                <div class="auth-group">
                    <label>🔐 Password (6+ characters)</label>
                    <input type="password" id="signupPassword" placeholder="password सेट करो" autocomplete="new-password">
                </div>

                <div class="auth-group">
                    <label>🔐 Confirm Password</label>
                    <input type="password" id="signupConfirm" placeholder="फिर से password डालो" autocomplete="new-password">
                </div>

                <button class="auth-btn" onclick="handleSignup()">✨ Sign Up करो</button>

                <div class="divider">या</div>

                <div class="auth-toggle">
                    पहले से account है? <button onclick="toggleAuthForm()">Login करो</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Settings Modal -->
    <div class="settings-modal" id="settingsModal">
        <div class="settings-content">
            <div class="settings-header">
                <h2>⚙️ Settings</h2>
                <button class="settings-close" onclick="closeSettings()">✕</button>
            </div>

            <div class="settings-group">
                <label>🔑 OpenAI API Key</label>
                <div class="settings-warning">
                    ⚠️ यह key secure नहीं है। Production के लिए backend server बनाओ।
                </div>
                <input type="password" id="apiKeyInput" placeholder="sk-...">
            </div>

            <div class="settings-group">
                <label>🤖 AI Model</label>
                <select id="modelSelect">
                    <option value="gpt-3.5-turbo">GPT-3.5 Turbo (Fast)</option>
                    <option value="gpt-4o-mini">GPT-4o Mini (Better)</option>
                    <option value="gpt-4-turbo">GPT-4 Turbo (Best)</option>
                </select>
            </div>

            <div class="settings-group">
                <label>🌡️ Temperature (0-1)</label>
                <input type="number" id="temperatureInput" min="0" max="1" step="0.1" value="0.7">
            </div>

            <div class="settings-group">
                <label>📊 Max Tokens per Response</label>
                <input type="number" id="maxTokensInput" min="100" max="4000" value="2000">
            </div>

            <div class="settings-group">
                <label>💬 Max Context Messages</label>
                <input type="number" id="contextMessagesInput" min="1" max="50" value="15">
            </div>

            <div class="settings-buttons">
                <button class="save-btn" onclick="saveSettings()">💾 Save</button>
                <button class="cancel-btn" onclick="closeSettings()">Cancel</button>
            </div>
        </div>
    </div>

    <!-- Loading Overlay -->
    <div class="loading-overlay" id="loadingOverlay">
        <div class="spinner"></div>
    </div>

    <!-- Main Chat Wrapper -->
    <div class="wrapper" id="chatWrapper">
        <!-- Sidebar -->
        <div class="sidebar">
            <div class="sidebar-header">
                <div class="sidebar-logo">🤖</div>
                <h2>Kanishk AI</h2>
                <p>Advanced Chatbot</p>
            </div>

            <div class="sidebar-content" id="chatsList">
                <div class="empty-state">
                    <div class="emoji">📝</div>
                    <div>कोई chat नहीं है</div>
                </div>
            </div>

            <div class="sidebar-footer">
                <button onclick="startNewChat()" title="New Chat">➕ New</button>
                <button onclick="openSettings()" title="Settings">⚙️ Settings</button>
                <button class="warning" onclick="toggleTheme()" title="Dark/Light">🌙 Theme</button>
                <button class="danger" onclick="logout()" title="Logout">🚪 Logout</button>
                <div class="user-info">
                    <strong id="currentUser">User</strong><br>
                    <small id="chatCount">0 chats</small>
                </div>
            </div>
        </div>

        <!-- Main Chat Area -->
        <div class="main">
            <div class="chat-header">
                <div class="chat-header-left">
                    <h1 id="chatTitle">💬 Kanishk AI Chat</h1>
                    <p id="chatIdDisplay">नई chat शुरू करने के लिए तैयार</p>
                </div>
                <div class="chat-header-right">
                    <button class="header-btn" onclick="openSettings()">⚙️ Settings</button>
                    <button class="header-btn" onclick="toggleTheme()">🌙 Dark Mode</button>
                </div>
            </div>

            <div class="chat-messages" id="chatMessages">
                <div class="empty-state">
                    <div class="emoji">🎯</div>
                    <div>Kanishk AI Chat शुरू करो!</div>
                    <p style="font-size: 12px; margin-top: 8px;">बाएं तरफ "New" दबाओ या existing chat select करो</p>
                </div>
            </div>

            <div class="stats">
                <span>💬 Messages: <strong id="msgCount">0</strong></span>
                <span>⚡ Tokens: <strong id="tokenCount">0</strong></span>
                <span>⏱️ Response: <strong id="responseTime">0ms</strong></span>
                <span>📊 Status: <strong id="status" style="color: #4caf50;">Ready</strong></span>
            </div>

            <div class="chat-controls">
                <button onclick="regenerateResponse()" title="Regenerate Last Response">🔄 Regenerate</button>
                <button onclick="editLastMessage()" title="Edit Last Message">✏️ Edit</button>
                <button onclick="copyLastMessage()" title="Copy Last Message">📋 Copy</button>
                <button onclick="clearChat()" title="Clear All Messages">🗑️ Clear</button>
                <button onclick="exportChat()" title="Download Chat">📥 Export</button>
                <button class="danger" onclick="deleteCurrentChat()" title="Delete Chat">❌ Delete</button>
            </div>

            <div class="input-area">
                <div class="input-wrapper">
                    <input type="text" id="userInput" placeholder="सवाल पूछो... / Ask anything..." onkeypress="handleKeyPress(event)" autocomplete="off" spellcheck="true" maxlength="5000">
                    <div class="char-count"><span id="charCount">0</span>/5000</div>
                </div>
                <div class="input-actions">
                    <button onclick="sendMessage()" id="sendBtn" title="Send (Shift+Enter = new line)">📤</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        // ============ CONSTANTS & CONFIG ============
        const API_BASE = 'https://api.openai.com/v1';
        const MAX_RETRIES = 3;
        const RETRY_DELAY = 1500;
        const TOKEN_LIMIT = 4000;
        const AUTO_SAVE_INTERVAL = 5000;

        // ============ STATE MANAGEMENT ============
        let state = {
            currentUser: null,
            currentChatId: null,
            isLoading: false,
            isDarkMode: localStorage.getItem('darkMode') === 'true',
            apiKey: localStorage.getItem('openai_api_key') || '',
            chats: {},
            users: JSON.parse(localStorage.getItem('users') || '{}'),
            sessionStats: {
                messagesCount: 0,
                tokensUsed: 0,
                totalResponseTime: 0
            },
            settings: {
                model: localStorage.getItem('model') || 'gpt-3.5-turbo',
                temperature: parseFloat(localStorage.getItem('temperature') || '0.7'),
                maxTokens: parseInt(localStorage.getItem('maxTokens') || '2000'),
                contextMessages: parseInt(localStorage.getItem('contextMessages') || '15')
            }
        };

        // ============ INITIALIZATION ============
        document.addEventListener('DOMContentLoaded', () => {
            console.log('🚀 Kanishk AI Initializing...');
            
            setTimeout(() => {
                document.getElementById('splashScreen').classList.add('hidden');
                loadAllData();
                applyTheme();
                checkAuth();
                
                document.getElementById('userInput').addEventListener('input', (e) => {
                    updateCharCount(e.target.value.length);
                });

                setInterval(() => saveAllData(), AUTO_SAVE_INTERVAL);
                console.log('✅ Kanishk AI Ready!');
            }, 2000);
        });

        // ============ AUTHENTICATION ============
        function toggleAuthForm() {
            const loginForm = document.getElementById('loginForm');
            const signupForm = document.getElementById('signupForm');
            
            if (loginForm.style.display === 'none') {
                loginForm.style.display = 'block';
                signupForm.style.display = 'none';
            } else {
                loginForm.style.display = 'none';
                signupForm.style.display = 'block';
            }
        }

        async function handleLogin() {
            const username = document.getElementById('loginUsername').value.trim();
            const password = document.getElementById('loginPassword').value;

            if (!username || !password) {
                showAuthError('❌ Username और password दोनों डालो');
                return;
            }

            if (username.length < 3) {
                showAuthError('❌ Username कम से कम 3 characters का होना चाहिए');
                return;
            }

            try {
                const hashedPass = hashPassword(password);

                if (!state.users[username]) {
                    showAuthError('❌ Username या password गलत है');
                    return;
                }

                if (state.users[username].password !== hashedPass) {
                    showAuthError('❌ Username या password गलत है');
                    return;
                }

                state.currentUser = username;
                localStorage.setItem('currentUser', username);
                
                const chatsKey = `chats_${username}`;
                state.chats = JSON.parse(localStorage.getItem(chatsKey) || '{}');

                document.getElementById('authContainer').classList.add('hidden');
                document.getElementById('chatWrapper').classList.add('active');
                document.getElementById('currentUser').textContent = username;

                console.log(`✅ ${username} logged in successfully`);
                loadChatsUI();
                updateChatCount();
            } catch (error) {
                showAuthError(`❌ Error: ${error.message}`);
            }
        }

        async function handleSignup() {
            const username = document.getElementById('signupUsername').value.trim();
            const password = document.getElementById('signupPassword').value;
            const confirm = document.getElementById('signupConfirm').value;

            if (!username || !password || !confirm) {
                showAuthError('❌ सभी fields fill करो');
                return;
            }

            if (username.length < 3) {
                showAuthError('❌ Username कम से कम 3 characters का होना चाहिए');
                return;
            }

            if (password.length < 6) {
                showAuthError('❌ Password कम से कम 6 characters का होना चाहिए');
                return;
            }

            if (password !== confirm) {
                showAuthError('❌ Passwords match नहीं कर रहे');
                return;
            }

            try {
                if (state.users[username]) {
                    showAuthError('❌ यह username पहले से exist करता है');
                    return;
                }

                const hashedPass = hashPassword(password);
                state.users[username] = {
                    username: username,
                    password: hashedPass,
                    createdAt: new Date().toISOString()
                };

                localStorage.setItem('users', JSON.stringify(state.users));
                
                state.currentUser = username;
                state.chats = {};
                localStorage.setItem('currentUser', username);
                localStorage.setItem(`chats_${username}`, '{}');

                document.getElementById('authContainer').classList.add('hidden');
                document.getElementById('chatWrapper').classList.add('active');
                document.getElementById('currentUser').textContent = username;

                console.log(`✅ Account created for ${username}`);
                loadChatsUI();
            } catch (error) {
                showAuthError(`❌ Error: ${error.message}`);
            }
        }

        function hashPassword(password) {
            let hash = 0;
            for (let i = 0; i < password.length; i++) {
                const char = password.charCodeAt(i);
                hash = ((hash << 5) - hash) + char;
                hash = hash & hash;
            }
            return 'hash_' + Math.abs(hash).toString(16);
        }

        function logout() {
            if (!confirm('क्या तुम logout करना चाहते हो?')) return;

            saveAllData();
            
            state.currentUser = null;
            state.currentChatId = null;
            state.chats = {};
            
            localStorage.removeItem('currentUser');

            document.getElementById('authContainer').classList.remove('hidden');
            document.getElementById('chatWrapper').classList.remove('active');
            document.getElementById('authError').innerHTML = '';
            document.getElementById('loginForm').style.display = 'block';
            document.getElementById('signupForm').style.display = 'none';
            document.getElementById('loginUsername').value = '';
            document.getElementById('loginPassword').value = '';

            console.log('✅ Logged out');
        }

        function checkAuth() {
            const user = localStorage.getItem('currentUser');
            
            if (user && state.users[user]) {
                state.currentUser = user;
                const chatsKey = `chats_${user}`;
                state.chats = JSON.parse(localStorage.getItem(chatsKey) || '{}');
                
                document.getElementById('authContainer').classList.add('hidden');
                document.getElementById('chatWrapper').classList.add('active');
                document.getElementById('currentUser').textContent = user;
                
                loadChatsUI();
                updateChatCount();
            }
        }

        function showAuthError(msg) {
            const errorDiv = document.getElementById('authError');
            errorDiv.innerHTML = `<div class="auth-error">${msg}</div>`;
            setTimeout(() => {
                errorDiv.innerHTML = '';
            }, 5000);
        }

        // ============ SETTINGS MANAGEMENT ============
        function openSettings() {
            document.getElementById('settingsModal').classList.add('active');
            document.getElementById('apiKeyInput').value = state.apiKey;
            document.getElementById('modelSelect').value = state.settings.model;
            document.getElementById('temperatureInput').value = state.settings.temperature;
            document.getElementById('maxTokensInput').value = state.settings.maxTokens;
            document.getElementById('contextMessagesInput').value = state.settings.contextMessages;
        }

        function closeSettings() {
            document.getElementById('settingsModal').classList.remove('active');
        }

        function saveSettings() {
            const apiKey = document.getElementById('apiKeyInput').value.trim();
            const model = document.getElementById('modelSelect').value;
            const temperature = parseFloat(document.getElementById('temperatureInput').value);
            const maxTokens = parseInt(document.getElementById('maxTokensInput').value);
            const contextMessages = parseInt(document.getElementById('contextMessagesInput').value);

            if (!apiKey.startsWith('sk-')) {
                alert('❌ Invalid API key format');
                return;
            }

            state.apiKey = apiKey;
            state.settings.model = model;
            state.settings.temperature = temperature;
            state.settings.maxTokens = maxTokens;
            state.settings.contextMessages = contextMessages;

            localStorage.setItem('openai_api_key', apiKey);
            localStorage.setItem('model', model);
            localStorage.setItem('temperature', temperature);
            localStorage.setItem('maxTokens', maxTokens);
            localStorage.setItem('contextMessages', contextMessages);

            updateStatus('✅ Settings Saved');
            setTimeout(() => updateStatus('Ready'), 2000);
            closeSettings();
        }

        // ============ CHAT MANAGEMENT ============
        function startNewChat() {
            if (!state.currentUser) {
                alert('पहले login करो');
                return;
            }

            const title = prompt('Chat का नाम दो (optional):') || 'Chat - ' + new Date().toLocaleTimeString();
            
            const chatId = `chat_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
            
            state.currentChatId = chatId;
            state.chats[chatId] = {
                id: chatId,
                title: title,
                messages: [],
                created: new Date().toISOString(),
                updated: new Date().toISOString(),
                totalTokens: 0,
                model: state.settings.model
            };

            state.sessionStats.messagesCount = 0;
            state.sessionStats.tokensUsed = 0;
            state.sessionStats.totalResponseTime = 0;

            saveAllData();
            loadChatsUI();
            updateChatCount();
            initializeChat();
            
            console.log(`✅ New chat created: ${chatId}`);
        }

        function selectChat(chatId) {
            if (!state.chats[chatId]) {
                showError('Chat नहीं मिल सकी');
                return;
            }

            state.currentChatId = chatId;
            document.getElementById('chatMessages').innerHTML = '';
            
            const chat = state.chats[chatId];
            state.sessionStats.messagesCount = chat.messages.length;

            if (chat.messages.length > 0) {
                chat.messages.forEach(msg => {
                    addMessage(msg.content, msg.role, false);
                });
            }

            loadChatsUI();
            updateStats();
            document.getElementById('chatTitle').textContent = `💬 ${chat.title}`;
            document.getElementById('chatIdDisplay').textContent = `Chat ID: ${chatId.substr(0, 20)}...`;
            
            console.log(`✅ Chat selected: ${chatId}`);
        }

        function deleteCurrentChat() {
            if (!state.currentChatId) {
                showError('कोई chat select नहीं है');
                return;
            }

            if (!confirm('क्या तुम यह chat permanently delete करना चाहते हो?')) return;

            delete state.chats[state.currentChatId];
            saveAllData();
            loadChatsUI();
            updateChatCount();
            
            state.currentChatId = null;
            document.getElementById('chatMessages').innerHTML = '<div class="empty-state"><div class="emoji">🎯</div><div>Chat शुरू करो!</div></div>';

            console.log('✅ Chat deleted');
        }

        function deleteChat(chatId, event) {
            event.stopPropagation();
            
            if (!confirm('क्या तुम यह chat permanently delete करना चाहते हो?')) return;

            delete state.chats[chatId];
            saveAllData();
            loadChatsUI();
            updateChatCount();

            if (state.currentChatId === chatId) {
                state.currentChatId = null;
                document.getElementById('chatMessages').innerHTML = '<div class="empty-state"><div class="emoji">🎯</div><div>Chat शुरू करो!</div></div>';
            }

            console.log('✅ Chat deleted');
        }

        function initializeChat() {
            document.getElementById('chatMessages').innerHTML = '';
            state.sessionStats.messagesCount = 0;
            updateStats();
            
            addMessage('👋 नमस्ते! मैं Kanishk AI हूँ। मैं आपके सभी सवालों का जवाब दे सकता हूँ। कृपया कोई भी सवाल पूछो!', 'system', false);
            
            if (state.chats[state.currentChatId]) {
                document.getElementById('chatTitle').textContent = `💬 ${state.chats[state.currentChatId].title}`;
            }
        }

        // ============ MESSAGE HANDLING ============
        function addMessage(text, sender = 'ai', save = true) {
            const messagesDiv = document.getElementById('chatMessages');
            const messageEl = document.createElement('div');
            messageEl.className = `message ${sender}`;

            let formattedText = sanitizeInput(text);

            if (sender === 'ai') {
                try {
                    const parsed = marked.parse(formattedText);
                    formattedText = `<div class="markdown-content">${parsed}</div>`;
                } catch (e) {
                    console.warn('Markdown parsing failed:', e);
                }
            } else {
                formattedText = formattedText.replace(/\n/g, '<br>');
            }

            const messageContent = `
                <div class="message-text">
                    <div class="text">${formattedText}</div>
                    <div class="message-actions">
                        <button onclick="copyMessage(this)">📋 Copy</button>
                        ${sender === 'ai' ? `<button onclick="editResponse(this)">🔄 Regenerate</button>` : ''}
                    </div>
                </div>
            `;

            messageEl.innerHTML = messageContent;
            messagesDiv.appendChild(messageEl);
            messagesDiv.scrollTop = messagesDiv.scrollHeight;

            // Highlight code blocks
            messageEl.querySelectorAll('pre code').forEach(block => {
                hljs.highlightElement(block);
            });

            if (sender === 'user' && save && state.currentChatId) {
                state.sessionStats.messagesCount++;
                state.chats[state.currentChatId].messages.push({
                    role: 'user',
                    content: text,
                    timestamp: new Date().toISOString()
                });
            } else if (sender === 'ai' && save && state.currentChatId) {
                state.chats[state.currentChatId].messages.push({
                    role: 'assistant',
                    content: text,
                    timestamp: new Date().toISOString()
                });
            }

            updateStats();
        }

        function showLoading() {
            const messagesDiv = document.getElementById('chatMessages');
            const loadingEl = document.createElement('div');
            loadingEl.className = 'message ai';
            loadingEl.id = 'loading-indicator';
            loadingEl.innerHTML = '<div class="loading"><span></span><span></span><span></span></div>';
            messagesDiv.appendChild(loadingEl);
            messagesDiv.scrollTop = messagesDiv.scrollHeight;
        }

        function removeLoading() {
            const loading = document.getElementById('loading-indicator');
            if (loading) {
                loading.remove();
            }
        }

        // ============ MESSAGE SENDING ============
        async function sendMessage() {
            const input = document.getElementById('userInput');
            const message = input.value.trim();

            if (!message || state.isLoading) return;

            if (!state.currentChatId) {
                showError('पहले नई chat शुरू करो!');
                return;
            }

            if (!state.apiKey) {
                showError('पहले Settings में API key set करो!');
                return;
            }

            state.isLoading = true;
            const startTime = Date.now();
            input.disabled = true;

            addMessage(message, 'user', true);
            input.value = '';
            updateCharCount(0);
            showLoading();
            updateStatus('AI सोच रहा है...');

            try {
                const response = await sendChatMessage(message);

                removeLoading();

                if (response.success) {
                    addMessage(response.text, 'ai', true);
                    
                    const responseTime = Date.now() - startTime;
                    state.sessionStats.totalResponseTime += responseTime;
                    state.sessionStats.tokensUsed += response.tokens || 0;
                    
                    if (state.chats[state.currentChatId]) {
                        state.chats[state.currentChatId].totalTokens = (state.chats[state.currentChatId].totalTokens || 0) + (response.tokens || 0);
                    }
                    
                    document.getElementById('responseTime').textContent = responseTime + 'ms';
                    updateStatus('Ready ✅');
                } else {
                    addMessage(`❌ Error: ${response.error}`, 'ai', false);
                    updateStatus('Error ❌');
                }

                saveAllData();
            } catch (error) {
                removeLoading();
                addMessage(`❌ Connection Error: ${error.message}`, 'ai', false);
                updateStatus('Error ❌');
                console.error('Error:', error);
            } finally {
                state.isLoading = false;
                input.disabled = false;
                input.focus();
            }
        }

        async function sendChatMessage(userMessage) {
            try {
                if (!state.currentChatId || !state.chats[state.currentChatId]) {
                    return { success: false, error: 'Chat not found' };
                }

                const chat = state.chats[state.currentChatId];
                let messages = [];

                const recentMessages = chat.messages.slice(-state.settings.contextMessages);
                messages = recentMessages.map(msg => ({
                    role: msg.role,
                    content: msg.content
                }));

                messages.push({
                    role: 'user',
                    content: userMessage
                });

                const payload = {
                    model: state.settings.model,
                    messages: [
                        {
                            role: 'system',
                            content: `आप Kanishk AI हैं - एक expert, helpful, और friendly AI assistant।

विशेषताएं:
✅ सभी topics पर ज्ञानी जवाब दें
✅ Hindi और English दोनों में बात करें
✅ Code लिखें और समझाएं
✅ समस्याओं का समाधान दें
✅ सच्चा और honest रहें
✅ Clear और detailed explanations दें
✅ उदाहरण से समझाएं
✅ Respectful और professional रहें
✅ Markdown formatting use करें`
                        },
                        ...messages
                    ],
                    temperature: state.settings.temperature,
                    max_tokens: state.settings.maxTokens,
                    top_p: 0.9,
                    frequency_penalty: 0.5,
                    presence_penalty: 0.5
                };

                const response = await fetch(`${API_BASE}/chat/completions`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${state.apiKey}`
                    },
                    body: JSON.stringify(payload)
                });

                if (!response.ok) {
                    const error = await response.json();

                    if (response.status === 401) {
                        return { success: false, error: '🔐 API key invalid है। Settings में ठीक करो।' };
                    } else if (response.status === 429) {
                        return { success: false, error: '⏱️ Rate limit exceeded। कुछ सेकंड रुको।' };
                    } else if (response.status === 500) {
                        return { success: false, error: '🖥️ OpenAI server error। कुछ सेकंड में फिर से try करो।' };
                    } else {
                        return { success: false, error: error.error?.message || 'Unknown error' };
                    }
                }

                const data = await response.json();
                const aiMessage = data.choices[0].message.content;
                const tokensUsed = data.usage.total_tokens;

                return {
                    success: true,
                    text: aiMessage,
                    tokens: tokensUsed
                };

            } catch (error) {
                console.error('API Error:', error);
                return {
                    success: false,
                    error: error.message || 'Network error'
                };
            }
        }

        function handleKeyPress(event) {
            if (event.key === 'Enter' && !event.shiftKey) {
                event.preventDefault();
                sendMessage();
            }
        }

        // ============ MESSAGE UTILITIES ============
        function copyMessage(button) {
            const textElement = button.closest('.message-text').querySelector('.text');
            const text = textElement.innerText;

            navigator.clipboard.writeText(text).then(() => {
                const originalText = button.textContent;
                button.textContent = '✅ Copied!';
                setTimeout(() => {
                    button.textContent = originalText;
                }, 2000);
            }).catch(err => {
                showError('Copy failed: ' + err.message);
            });
        }

        function copyLastMessage() {
            const messages = document.querySelectorAll('.message .text');
            if (messages.length === 0) return;
            
            const lastMessage = messages[messages.length - 1].innerText;
            navigator.clipboard.writeText(lastMessage).then(() => {
                updateStatus('✅ Copied!');
                setTimeout(() => updateStatus('Ready'), 2000);
            }).catch(err => {
                showError('Copy failed');
            });
        }

        function regenerateResponse() {
            if (!state.currentChatId) {
                showError('कोई chat select नहीं है');
                return;
            }

            const messages = state.chats[state.currentChatId].messages;
            if (messages.length < 2) {
                showError('कोई response regenerate करने के लिए नहीं है');
                return;
            }

            const lastUserMessage = messages[messages.length - 2];
            if (lastUserMessage.role !== 'user') {
                showError('Last message user का नहीं है');
                return;
            }

            messages.pop();
            
            const messageElements = document.querySelectorAll('.message');
            if (messageElements.length > 0) {
                messageElements[messageElements.length - 1].remove();
            }

            document.getElementById('userInput').value = lastUserMessage.content;
            sendMessage();
        }

        function editResponse(button) {
            regenerateResponse();
        }

        function editLastMessage() {
            if (!state.currentChatId) return;
            
            const messages = state.chats[state.currentChatId].messages;
            const lastUserMsg = messages.filter(m => m.role === 'user').pop();

            if (!lastUserMsg) {
                showError('कोई message edit करने के लिए नहीं है');
                return;
            }

            document.getElementById('userInput').value = lastUserMsg.content;
            document.getElementById('userInput').focus();
        }

        // ============ CHAT ACTIONS ============
        function clearChat() {
            if (!state.currentChatId) return;
            
            if (!confirm('क्या तुम सभी messages delete करना चाहते हो?')) return;

            document.getElementById('chatMessages').innerHTML = '';
            state.chats[state.currentChatId].messages = [];
            state.sessionStats.messagesCount = 0;
            state.sessionStats.tokensUsed = 0;
            
            updateStats();
            saveAllData();
        }

        function exportChat() {
            if (!state.currentChatId) {
                showError('Export करने के लिए chat नहीं है');
                return;
            }

            const chat = state.chats[state.currentChatId];
            let content = `📌 Chat: ${chat.title}\n`;
            content += `⏰ Created: ${new Date(chat.created).toLocaleString()}\n`;
            content += `📊 Model: ${chat.model}\n`;
            content += `⚡ Tokens Used: ${chat.totalTokens || 0}\n`;
            content += `${'='.repeat(70)}\n\n`;

            chat.messages.forEach(msg => {
                const sender = msg.role === 'user' ? '👤 You' : '🤖 Kanishk AI';
                content += `${sender}:\n${msg.content}\n\n`;
            });

            const dataStr = 'data:text/plain;charset=utf-8,' + encodeURIComponent(content);
            downloadFile(dataStr, `kanishk_chat_${Date.now()}.txt`);
        }

        function exportAllChats() {
            let content = `📌 All Chats Export - Kanishk AI\n`;
            content += `⏰ Exported: ${new Date().toLocaleString()}\n`;
            content += `👤 User: ${state.currentUser}\n`;
            content += `${'='.repeat(70)}\n\n`;

            Object.values(state.chats).forEach((chat, index) => {
                content += `\n${'='.repeat(70)}\n`;
                content += `Chat ${index + 1}: ${chat.title}\n`;
                content += `Created: ${new Date(chat.created).toLocaleString()}\n`;
                content += `Model: ${chat.model}\n`;
                content += `Tokens: ${chat.totalTokens || 0}\n`;
                content += `${'='.repeat(70)}\n\n`;

                chat.messages.forEach(msg => {
                    const sender = msg.role === 'user' ? '👤 You' : '🤖 Kanishk AI';
                    content += `${sender}:\n${msg.content}\n\n`;
                });
            });

            const dataStr = 'data:text/plain;charset=utf-8,' + encodeURIComponent(content);
            downloadFile(dataStr, `kanishk_all_chats_${Date.now()}.txt`);
        }

        function downloadFile(dataStr, filename) {
            const link = document.createElement('a');
            link.href = dataStr;
            link.download = filename;
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        }

        // ============ UI UTILITIES ============
        function toggleTheme() {
            state.isDarkMode = !state.isDarkMode;
            localStorage.setItem('darkMode', state.isDarkMode);
            applyTheme();
        }

        function applyTheme() {
            if (state.isDarkMode) {
                document.body.classList.add('dark-mode');
            } else {
                document.body.classList.remove('dark-mode');
            }
        }

        function updateCharCount(length) {
            document.getElementById('charCount').textContent = length;
        }

        function updateStats() {
            document.getElementById('msgCount').textContent = state.sessionStats.messagesCount;
            if (state.currentChatId && state.chats[state.currentChatId]) {
                document.getElementById('tokenCount').textContent = state.chats[state.currentChatId].totalTokens || 0;
            }
        }

        function updateStatus(text) {
            document.getElementById('status').textContent = text;
        }

        function showError(msg) {
            addMessage(`❌ ${msg}`, 'system', false);
        }

        function updateChatCount() {
            const count = Object.keys(state.chats).length;
            document.getElementById('chatCount').textContent = `${count} ${count === 1 ? 'chat' : 'chats'}`;
        }

        function loadChatsUI() {
            const chatsList = document.getElementById('chatsList');
            const chatIds = Object.keys(state.chats).sort((a, b) => {
                return new Date(state.chats[b].updated) - new Date(state.chats[a].updated);
            });

            if (chatIds.length === 0) {
                chatsList.innerHTML = '<div class="empty-state"><div class="emoji">📝</div><div>कोई chat नहीं है</div></div>';
                return;
            }

            chatsList.innerHTML = '';
            chatIds.forEach(id => {
                const chat = state.chats[id];
                const item = document.createElement('div');
                item.className = `chat-item${id === state.currentChatId ? ' active' : ''}`;
                const chatDate = new Date(chat.updated).toLocaleTimeString();
                item.innerHTML = `
                    <div class="chat-item-name" onclick="selectChat('${id}')" style="cursor: pointer;">
                        <strong>${chat.title.substr(0, 28)}...</strong>
                        <small style="opacity: 0.7; display: block; font-size: 11px;">${chatDate}</small>
                    </div>
                    <button class="delete" onclick="deleteChat('${id}', event)">✕</button>
                `;
                chatsList.appendChild(item);
            });
        }

        // ============ DATA PERSISTENCE ============
        function saveAllData() {
            if (state.currentUser && state.currentChatId && state.chats[state.currentChatId]) {
                state.chats[state.currentChatId].updated = new Date().toISOString();
            }

            if (state.currentUser) {
                localStorage.setItem(`chats_${state.currentUser}`, JSON.stringify(state.chats));
            }
        }

        function loadAllData() {
            try {
                state.users = JSON.parse(localStorage.getItem('users') || '{}');
                
                if (state.currentUser) {
                    const chatsKey = `chats_${state.currentUser}`;
                    state.chats = JSON.parse(localStorage.getItem(chatsKey) || '{}');
                }
            } catch (error) {
                console.error('Error loading data:', error);
                state.chats = {};
            }
        }

        // ============ SECURITY ============
        function sanitizeInput(text) {
            const map = {
                '&': '&amp;',
                '<': '&lt;',
                '>': '&gt;',
                '"': '&quot;',
                "'": '&#039;'
            };
            return text.replace(/[&<>"']/g, m => map[m]);
        }
    </script>
</body>
</html>
