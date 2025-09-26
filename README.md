<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SecureChat - Private Messaging</title>
    <style>
        :root {
            --primary-color: #3498db;
            --secondary-color: #2980b9;
            --background-color: #f5f7fa;
            --message-sent: #3498db;
            --message-received: #ecf0f1;
            --text-color: #333;
            --error-color: #e74c3c;
            --success-color: #2ecc71;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: var(--background-color);
            color: var(--text-color);
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .container {
            width: 100%;
            max-width: 800px;
            height: 90vh;
            background: white;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            display: flex;
            flex-direction: column;
        }

        /* Login Screen */
        .login-screen {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 40px;
            height: 100%;
            text-align: center;
        }

        .logo {
            margin-bottom: 30px;
        }

        .logo h1 {
            color: var(--primary-color);
            font-size: 2.5rem;
            margin-bottom: 10px;
        }

        .logo p {
            color: #7f8c8d;
            font-size: 1rem;
        }

        .login-form {
            width: 100%;
            max-width: 400px;
        }

        .form-group {
            margin-bottom: 20px;
            text-align: left;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #555;
        }

        .form-group input {
            width: 100%;
            padding: 12px 15px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 1rem;
            transition: border 0.3s;
        }

        .form-group input:focus {
            border-color: var(--primary-color);
            outline: none;
        }

        .btn {
            background-color: var(--primary-color);
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: background-color 0.3s;
            width: 100%;
        }

        .btn:hover {
            background-color: var(--secondary-color);
        }

        .error-message {
            color: var(--error-color);
            margin-top: 10px;
            display: none;
        }

        /* Chat Screen */
        .chat-screen {
            display: none;
            flex-direction: column;
            height: 100%;
        }

        .chat-header {
            background-color: var(--primary-color);
            color: white;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .chat-header h2 {
            font-size: 1.2rem;
        }

        .user-info {
            display: flex;
            align-items: center;
        }

        .user-avatar {
            width: 35px;
            height: 35px;
            border-radius: 50%;
            background-color: white;
            color: var(--primary-color);
            display: flex;
            justify-content: center;
            align-items: center;
            font-weight: bold;
            margin-right: 10px;
        }

        .chat-messages {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
        }

        .message {
            max-width: 70%;
            padding: 12px 15px;
            margin-bottom: 15px;
            border-radius: 18px;
            position: relative;
            word-wrap: break-word;
        }

        .message-sent {
            align-self: flex-end;
            background-color: var(--message-sent);
            color: white;
            border-bottom-right-radius: 5px;
        }

        .message-received {
            align-self: flex-start;
            background-color: var(--message-received);
            color: var(--text-color);
            border-bottom-left-radius: 5px;
        }

        .message-sender {
            font-size: 0.8rem;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .message-time {
            font-size: 0.7rem;
            text-align: right;
            margin-top: 5px;
            opacity: 0.7;
        }

        .chat-input {
            display: flex;
            padding: 15px;
            border-top: 1px solid #eee;
        }

        .message-input {
            flex: 1;
            padding: 12px 15px;
            border: 1px solid #ddd;
            border-radius: 25px;
            font-size: 1rem;
            margin-right: 10px;
        }

        .message-input:focus {
            outline: none;
            border-color: var(--primary-color);
        }

        .send-btn {
            background-color: var(--primary-color);
            color: white;
            border: none;
            border-radius: 50%;
            width: 45px;
            height: 45px;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        .send-btn:hover {
            background-color: var(--secondary-color);
        }

        .invite-section {
            padding: 15px 20px;
            background-color: #f8f9fa;
            border-top: 1px solid #eee;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .invite-link {
            flex: 1;
            padding: 8px 12px;
            border: 1px dashed #ccc;
            border-radius: 5px;
            background-color: white;
            margin-right: 10px;
            font-size: 0.9rem;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }

        .copy-btn {
            background-color: #95a5a6;
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 0.9rem;
        }

        /* Responsive Design */
        @media (max-width: 600px) {
            .container {
                height: 100vh;
                border-radius: 0;
            }
            
            .message {
                max-width: 85%;
            }
            
            .chat-header h2 {
                font-size: 1rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Login Screen -->
        <div class="login-screen" id="loginScreen">
            <div class="logo">
                <h1>SecureChat</h1>
                <p>Private, secure messaging for you and your friends</p>
            </div>
            <div class="login-form">
                <div class="form-group">
                    <label for="username">Username</label>
                    <input type="text" id="username" placeholder="Enter your username" required>
                </div>
                <div class="form-group">
                    <label for="password">Password</label>
                    <input type="password" id="password" placeholder="Enter password" required>
                </div>
                <button class="btn" id="loginBtn">Join Chat</button>
                <div class="error-message" id="errorMessage">Invalid credentials. Please try again.</div>
            </div>
        </div>

        <!-- Chat Screen -->
        <div class="chat-screen" id="chatScreen">
            <div class="chat-header">
                <h2>SecureChat Room</h2>
                <div class="user-info">
                    <div class="user-avatar" id="userAvatar">U</div>
                    <span id="currentUsername">User</span>
                </div>
            </div>
            
            <div class="chat-messages" id="chatMessages">
                <!-- Messages will be dynamically added here -->
                <div class="message message-received">
                    <div class="message-sender">System</div>
                    <div class="message-text">Welcome to SecureChat! Start a conversation with your friends.</div>
                    <div class="message-time">Just now</div>
                </div>
            </div>
            
            <div class="invite-section">
                <div class="invite-link" id="inviteLink">Copy this link to invite friends</div>
                <button class="copy-btn" id="copyBtn">Copy</button>
            </div>
            
            <div class="chat-input">
                <input type="text" class="message-input" id="messageInput" placeholder="Type your message...">
                <button class="send-btn" id="sendBtn">
                    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path d="M22 2L11 13" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                        <path d="M22 2L15 22L11 13L2 9L22 2Z" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    </svg>
                </button>
            </div>
        </div>
    </div>

    <script>
        // DOM Elements
        const loginScreen = document.getElementById('loginScreen');
        const chatScreen = document.getElementById('chatScreen');
        const loginBtn = document.getElementById('loginBtn');
        const errorMessage = document.getElementById('errorMessage');
        const usernameInput = document.getElementById('username');
        const passwordInput = document.getElementById('password');
        const currentUsername = document.getElementById('currentUsername');
        const userAvatar = document.getElementById('userAvatar');
        const chatMessages = document.getElementById('chatMessages');
        const messageInput = document.getElementById('messageInput');
        const sendBtn = document.getElementById('sendBtn');
        const inviteLink = document.getElementById('inviteLink');
        const copyBtn = document.getElementById('copyBtn');

        // Application state
        let currentUser = '';
        let messages = [];
        
        // Predefined users (in a real app, this would be handled by a backend)
        const validUsers = {
            'alice': 'password123',
            'bob': 'securepass',
            'charlie': 'mypassword'
        };

        // Initialize the application
        function init() {
            // Set up event listeners
            loginBtn.addEventListener('click', handleLogin);
            sendBtn.addEventListener('click', sendMessage);
            messageInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    sendMessage();
                }
            });
            copyBtn.addEventListener('click', copyInviteLink);
            
            // Generate invite link
            inviteLink.textContent = window.location.href;
        }

        // Handle user login
        function handleLogin() {
            const username = usernameInput.value.trim();
            const password = passwordInput.value;
            
            if (!username || !password) {
                showError('Please enter both username and password');
                return;
            }
            
            // Check credentials (in a real app, this would be a server-side check)
            if (validUsers[username.toLowerCase()] && validUsers[username.toLowerCase()] === password) {
                // Successful login
                currentUser = username;
                currentUsername.textContent = username;
                userAvatar.textContent = username.charAt(0).toUpperCase();
                
                // Switch to chat screen
                loginScreen.style.display = 'none';
                chatScreen.style.display = 'flex';
                
                // Add login message
                addSystemMessage(`${username} joined the chat`);
            } else {
                showError('Invalid username or password');
            }
        }

        // Show error message
        function showError(message) {
            errorMessage.textContent = message;
            errorMessage.style.display = 'block';
            
            // Hide error after 3 seconds
            setTimeout(() => {
                errorMessage.style.display = 'none';
            }, 3000);
        }

        // Send a message
        function sendMessage() {
            const messageText = messageInput.value.trim();
            
            if (!messageText) return;
            
            // Create message object
            const message = {
                id: Date.now(),
                sender: currentUser,
                text: messageText,
                timestamp: new Date()
            };
            
            // Add to messages array
            messages.push(message);
            
            // Display the message
            displayMessage(message);
            
            // In a real app, send to server/other users here
            // simulateReceivingMessage(message);
            
            // Clear input
            messageInput.value = '';
        }

        // Display a message in the chat
        function displayMessage(message) {
            const messageDiv = document.createElement('div');
            messageDiv.className = `message ${message.sender === currentUser ? 'message-sent' : 'message-received'}`;
            
            // If it's not the current user, show sender name
            if (message.sender !== currentUser) {
                const senderDiv = document.createElement('div');
                senderDiv.className = 'message-sender';
                senderDiv.textContent = message.sender;
                messageDiv.appendChild(senderDiv);
            }
            
            const textDiv = document.createElement('div');
            textDiv.className = 'message-text';
            textDiv.textContent = message.text;
            messageDiv.appendChild(textDiv);
            
            const timeDiv = document.createElement('div');
            timeDiv.className = 'message-time';
            timeDiv.textContent = formatTime(message.timestamp);
            messageDiv.appendChild(timeDiv);
            
            chatMessages.appendChild(messageDiv);
            
            // Scroll to bottom
            chatMessages.scrollTop = chatMessages.scrollHeight;
        }

        // Add a system message
        function addSystemMessage(text) {
            const messageDiv = document.createElement('div');
            messageDiv.className = 'message message-received';
            
            const senderDiv = document.createElement('div');
            senderDiv.className = 'message-sender';
            senderDiv.textContent = 'System';
            messageDiv.appendChild(senderDiv);
            
            const textDiv = document.createElement('div');
            textDiv.className = 'message-text';
            textDiv.textContent = text;
            messageDiv.appendChild(textDiv);
            
            const timeDiv = document.createElement('div');
            timeDiv.className = 'message-time';
            timeDiv.textContent = formatTime(new Date());
            messageDiv.appendChild(timeDiv);
            
            chatMessages.appendChild(messageDiv);
            
            // Scroll to bottom
            chatMessages.scrollTop = chatMessages.scrollHeight;
        }

        // Format time for display
        function formatTime(date) {
            return date.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
        }

        // Copy invite link to clipboard
        function copyInviteLink() {
            navigator.clipboard.writeText(inviteLink.textContent)
                .then(() => {
                    // Show feedback
                    const originalText = copyBtn.textContent;
                    copyBtn.textContent = 'Copied!';
                    setTimeout(() => {
                        copyBtn.textContent = originalText;
                    }, 2000);
                })
                .catch(err => {
                    console.error('Failed to copy: ', err);
                });
        }

        // In a real application, this would handle receiving messages from other users
        function simulateReceivingMessage(message) {
            // This is just a simulation - in a real app, messages would come from a server
            setTimeout(() => {
                if (message.sender !== currentUser) {
                    displayMessage(message);
                }
            }, 1000);
        }

        // Initialize the app when the page loads
        window.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
