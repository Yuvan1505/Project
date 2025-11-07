import React, { useState, useEffect, useRef } from "react";
import ReactDOM from "react-dom/client";

function App() {
  const [messages, setMessages] = useState([
    { sender: "John", text: "Hey there!", time: "10:30 AM" },
    { sender: "You", text: "Hello 👋", time: "10:31 AM" },
  ]);
  const [darkMode, setDarkMode] = useState(false);
  const [text, setText] = useState("");

  const contacts = [
    { name: "John", online: true },
    { name: "Emma", online: false },
    { name: "David", online: true },
    { name: "Sarah", online: true },
  ];

  const chatEndRef = useRef(null);

  useEffect(() => {
    chatEndRef.current?.scrollIntoView({ behavior: "smooth" });
  }, [messages]);

  const sendMessage = () => {
    if (text.trim() === "") return;
    const now = new Date();
    const time = now.toLocaleTimeString([], { hour: "2-digit", minute: "2-digit" });
    setMessages([...messages, { sender: "You", text, time }]);
    setText("");
  };

  return (
    <div>
      {/* Inline Styles */}
      <style>{`
        body { margin:0; font-family:Poppins,sans-serif; background:#f5f6fa; }
        .chat-app { display:flex; height:100vh; transition:0.3s; }
        .dark { background:#2f3640; color:white; }
        .sidebar { width:25%; background:#2f3640; color:white; padding:20px; display:flex; flex-direction:column; }
        .sidebar-header { display:flex; justify-content:space-between; align-items:center; }
        .mode-toggle { background:none; border:none; font-size:20px; color:white; cursor:pointer; }
        .sidebar ul { list-style:none; padding:0; margin-top:20px; }
        .sidebar li { padding:10px; border-radius:8px; display:flex; align-items:center; gap:10px; transition:0.3s; }
        .sidebar li:hover { background:#353b48; }
        .status-dot { width:10px; height:10px; border-radius:50%; }
        .online { background:#4cd137; }
        .offline { background:#c23616; }
        .chat-section { flex:1; display:flex; flex-direction:column; background:white; transition:0.3s; }
        .dark .chat-section { background:#353b48; }
        .chat-window { flex:1; padding:20px; overflow-y:auto; background:#ecf0f1; }
        .dark .chat-window { background:#414b57; }
        .message { margin:8px 0; padding:10px 14px; border-radius:12px; max-width:60%; position:relative; }
        .sent { background:#4cd137; color:white; margin-left:auto; }
        .received { background:#dcdde1; color:black; margin-right:auto; }
        .dark .received { background:#718093; }
        .time { font-size:10px; opacity:0.7; display:block; margin-top:4px; text-align:right; }
        .message-input { display:flex; padding:10px; border-top:1px solid #dcdde1; background:white; }
        .dark .message-input { background:#2f3640; border-top:1px solid #718093; }
        .message-input input { flex:1; padding:10px; border:none; border-radius:20px; outline:none; background:#f1f2f6; }
        .dark .message-input input { background:#718093; color:white; }
        .message-input button { background:#0097e6; color:white; border:none; padding:10px 16px; border-radius:20px; margin-left:8px; cursor:pointer; transition:0.3s; }
        .message-input button:hover { background:#40739e; }
      `}</style>

      <div className={`chat-app ${darkMode ? "dark" : ""}`}>
        <div className="sidebar">
          <div className="sidebar-header">
            <h2>Chats</h2>
            <button className="mode-toggle" onClick={() => setDarkMode(!darkMode)}>
              {darkMode ? "☀️" : "🌙"}
            </button>
          </div>
          <ul>
            {contacts.map((c, i) => (
              <li key={i}>
                <span className={`status-dot ${c.online ? "online" : "offline"}`}></span>
                {c.name}
              </li>
            ))}
          </ul>
        </div>

        <div className="chat-section">
          <div className="chat-window">
            {messages.map((msg, i) => (
              <div
                key={i}
                className={`message ${msg.sender === "You" ? "sent" : "received"}`}
              >
                <p>{msg.text}</p>
                <span className="time">{msg.time}</span>
              </div>
            ))}
            <div ref={chatEndRef} />
          </div>

          <div className="message-input">
            <input
              type="text"
              placeholder="Type a message..."
              value={text}
              onChange={(e) => setText(e.target.value)}
              onKeyDown={(e) => e.key === "Enter" && sendMessage()}
            />
            <button onClick={sendMessage}>Send</button>
          </div>
        </div>
      </div>
    </div>
  );
}

// Mount the App
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);
