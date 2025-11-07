<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Smart Chat Assistant</title>
  <style>
    * {
      box-sizing: border-box;
      font-family: "Poppins", sans-serif;
    }

    body {
      margin: 0;
      padding: 0;
      background: linear-gradient(135deg, #667eea, #764ba2);
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }

    .chat-container {
      width: 420px;
      height: 640px;
      background: #fff;
      border-radius: 25px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
      display: flex;
      flex-direction: column;
      overflow: hidden;
      position: relative;
    }

    .chat-header {
      background: linear-gradient(135deg, #764ba2, #667eea);
      color: #fff;
      padding: 18px;
      text-align: center;
      font-size: 1.3em;
      font-weight: 600;
      letter-spacing: 0.5px;
    }

    .chat-messages {
      flex: 1;
      padding: 15px;
      background: #f7f9fc;
      overflow-y: auto;
      display: flex;
      flex-direction: column;
      gap: 12px;
      scroll-behavior: smooth;
    }

    .message {
      max-width: 80%;
      padding: 10px 15px;
      border-radius: 20px;
      font-size: 0.95em;
      animation: fadeIn 0.3s ease-in;
      line-height: 1.5;
      position: relative;
    }

    .message.user {
      align-self: flex-end;
      background: #667eea;
      color: white;
      border-bottom-right-radius: 5px;
    }

    .message.bot {
      align-self: flex-start;
      background: #e5e5ea;
      color: #000;
      border-bottom-left-radius: 5px;
    }

    .time {
      font-size: 0.7em;
      color: #555;
      margin-top: 2px;
      text-align: right;
    }

    .chat-input {
      display: flex;
      border-top: 1px solid #ddd;
      background: #fff;
    }

    .chat-input input {
      flex: 1;
      border: none;
      outline: none;
      padding: 15px;
      font-size: 1em;
      border-radius: 0;
    }

    .chat-input button {
      background: linear-gradient(135deg, #667eea, #764ba2);
      color: white;
      border: none;
      padding: 15px 25px;
      font-size: 1em;
      cursor: pointer;
      transition: 0.3s;
      border-radius: 0;
    }

    .chat-input button:hover {
      opacity: 0.9;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(5px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .bot-typing {
      font-size: 0.85em;
      color: #999;
      margin-left: 10px;
      animation: blink 1.2s infinite;
    }

    @keyframes blink {
      0% { opacity: 0.3; }
      50% { opacity: 1; }
      100% { opacity: 0.3; }
    }
  </style>
</head>
<body>
  <div class="chat-container">
    <div class="chat-header">💬 Smart Chat Assistant</div>
    <div class="chat-messages" id="chatMessages"></div>
    <div class="chat-input">
      <input type="text" id="userInput" placeholder="Type a message..." />
      <button onclick="sendMessage()">Send</button>
    </div>
  </div>

  <script>
    const chatMessages = document.getElementById("chatMessages");
    const userInput = document.getElementById("userInput");

    function addMessage(text, sender) {
      const msg = document.createElement("div");
      msg.classList.add("message", sender);
      msg.innerHTML = text;

      const time = document.createElement("div");
      time.classList.add("time");
      const now = new Date();
      time.textContent = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });

      msg.appendChild(time);
      chatMessages.appendChild(msg);
      chatMessages.scrollTop = chatMessages.scrollHeight;
    }

    function botReply(userText) {
      const lower = userText.toLowerCase();

      if (lower.includes("hi") || lower.includes("hello")) {
        return "Hello 👋! How can I help you today?";
      } else if (lower.includes("time")) {
        return `The current time is ${new Date().toLocaleTimeString()}.`;
      } else if (lower.includes("date")) {
        return `Today's date is ${new Date().toLocaleDateString()}.`;
      } else if (lower.includes("your name")) {
        return "I'm your Smart Chat Assistant 🤖";
      } else if (lower.includes("help")) {
        return "You can ask me about time, date, or just chat casually!";
      } else if (lower.includes("bye")) {
        return "Goodbye 👋! Have a great day!";
      } else if (lower.includes("joke")) {
        const jokes = [
          "Why don't skeletons fight each other? They don’t have the guts! 💀",
          "What do you call fake spaghetti? An impasta! 🍝",
          "Why did the computer show up at work late? It had a hard drive! 💻"
        ];
        return jokes[Math.floor(Math.random() * jokes.length)];
      } else {
        return "I'm not sure I understand 🤔, but I'm learning every day!";
      }
    }

    function sendMessage() {
      const text = userInput.value.trim();
      if (text === "") return;
      addMessage(text, "user");
      userInput.value = "";

      const typingIndicator = document.createElement("div");
      typingIndicator.classList.add("message", "bot");
      typingIndicator.innerHTML = `<span class='bot-typing'>Assistant is typing...</span>`;
      chatMessages.appendChild(typingIndicator);
      chatMessages.scrollTop = chatMessages.scrollHeight;

      setTimeout(() => {
        typingIndicator.remove();
        const reply = botReply(text);
        addMessage(reply, "bot");
      }, 1000);
    }

    userInput.addEventListener("keypress", (e) => {
      if (e.key === "Enter") sendMessage();
    });

    // Auto greeting
    window.onload = () => {
      addMessage("Hello! 👋 I'm your Smart Chat Assistant. How can I help you?", "bot");
    };
  </script>
</body>
</html>
