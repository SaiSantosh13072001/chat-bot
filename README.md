<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chatbot</title>
    <style>
        /* CSS styles */
        .chat-container {
            max-width: flex;
            margin: 0 flex;
        }
        .chat-message {
            border: 1px solid #f8f4f4;
            padding: 10px;
            height: 500px;
            overflow-y: scroll;
        }
        .chat-container input{
            width: 95%;
            padding: 6px;
        }
    </style>
</head>
<body>
    <h1>Hello,<br>
        Upload Documents or Text</h1>   
        <div class="chat-container">
            <div class="chat-messages" id="chat-messages">
                <!-- Display chat messages here -->
            </div>
        <div id="chat-area"></div>
        <input type="text" id="message-input" placeholder="Type your message...">
        <button id="send-button">Send</button>
    </div>

    <form action="" method="post" enctype="application/x-www-form-urlencoded">
        <input type="file" name="file" accept=".txt,.pdf,.doc,.docx">
        <input type="submit" value="Upload">
    </form>


    <script>
        document.addEventListener('DOMContentLoaded', function () {
            var chatArea = document.getElementById('chat-area');
            var messageInput = document.getElementById('message-input');
            var sendButton = document.getElementById('send-button');

            // Function to add message to chat area
            function addMessage(message, sender) {
                var messageElement = document.createElement('div');
                messageElement.className = sender === 'user' ? 'user-message' : 'bot-message';
                messageElement.textContent = message;
                chatArea.appendChild(messageElement);
                // Scroll to bottom
                chatArea.scrollTop = chatArea.scrollHeight;
            }

            // Function to handle user input
            function sendMessage() {
                var message = messageInput.value.trim();
                if (message === '') return;
                addMessage(message, 'user');
                messageInput.value = '';
                // Send message to backend servlet
                fetch('/chatbot', {
                    method: 'POST',
                    body: JSON.stringify({ message: message }),
                    headers: {
                        'Content-Type': 'application/json'
                    }
                })
                .then(response => response.json())
                .then(data => addMessage(data.response, 'bot'))
                .catch(error => console.error('Error:', error));
            }


            // Event listener for send button click
            sendButton.addEventListener('click', sendMessage);

            // Event listener for Enter key press
            messageInput.addEventListener('keypress', function (event) {
                if (event.key === 'Enter') {
                    sendMessage();
                }
            });
        });
    </script>
</body>
</html>
![Chatbot - Google Chrome 16-05-2024 16_22_08](https://github.com/SaiSantosh13072001/chat-bot/assets/153276610/8863277a-d98c-4f70-aeff-28b9dafb5756)

