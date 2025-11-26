# 🌍 Real-Time Location Tracker


*Real-time GPS tracking using Node.js, Express, Socket.IO, and Leaflet.js.*

---

## 🎨 Improved UI (Frontend Upgrade)

Your project now includes a modern, clean UI with:

- 🌫️ Glassmorphic status panel  
- 🧭 Modern Leaflet theme  
- 🟢 Live tracking indicator  
- 🗺️ Fullscreen map container  
- 💬 Marker bubbles for users  
- 🟦 Smooth shadows and rounded borders  

### **Updated CSS (style.css)**

```css
body {
  margin: 0;
  padding: 0;
  background: #0f172a;
  font-family: "Poppins", sans-serif;
}

#map {
  height: 100vh;
  width: 100%;
  border-radius: 18px;
  overflow: hidden;
  box-shadow: 0 0 25px rgba(0,0,0,0.4);
}

.status-box {
  position: fixed;
  top: 20px;
  left: 20px;
  padding: 14px 20px;
  border-radius: 14px;
  backdrop-filter: blur(10px);
  background: rgba(255,255,255,0.1);
  color: #fff;
  font-size: 14px;
  border: 1px solid rgba(255,255,255,0.2);
  box-shadow: 0 0 12px rgba(255,255,255,0.2);
}
````

Add inside `<body>`:

```html
<div class="status-box">🟢 Tracking Active</div>
```

---

## 📂 Folder Structure

```
📁 Real-Time-Location-Tracker/
 ├── 📁 public/
 │   ├── 📁 css/
 │   │   └── style.css
 │   ├── 📁 js/
 │   │   └── script.js
 │   └── index.ejs
 ├── 📁 views/
 │   └── index.ejs
 ├── app.js (or server.js)
 ├── package.json
 ├── README.md
 └── .gitignore
```

---

## 🛠️ Technologies Used

| Technology        | Role                      |
| ----------------- | ------------------------- |
| **Node.js**       | Backend runtime           |
| **Express.js**    | Routing & server          |
| **Socket.IO**     | Real-time communication   |
| **Leaflet.js**    | Map rendering             |
| **OpenStreetMap** | Free map tiles            |
| **EJS**           | Server-side view template |

---

## 🚀 Getting Started

### 1️⃣ Install Dependencies

```bash
npm install
```

### 2️⃣ Run Server

```bash
node app.js
```

### 3️⃣ Open Browser

```
http://localhost:3000
```

---

## 📌 Client Code (script.js)

```javascript
const socket = io();

if (navigator.geolocation) {
    navigator.geolocation.watchPosition((position) => {
        const { latitude, longitude } = position.coords;
        socket.emit("send-location", { latitude, longitude });
    }, console.error,
    { enableHighAccuracy: true, timeout: 5000, maximumAge: 0 });
}

const map = L.map("map").setView([0, 0], 16);

L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
    attribution: "World Map"
}).addTo(map);

const markers = {};

socket.on("recieve-location", ({ id, latitude, longitude }) => {
    map.setView([latitude, longitude]);
    if (markers[id]) markers[id].setLatLng([latitude, longitude]);
    else markers[id] = L.marker([latitude, longitude]).addTo(map);
});

socket.on("user-disconnected", (id) => {
    if (markers[id]) {
        map.removeLayer(markers[id]);
        delete markers[id];
    }
});
```

---

## 🖥️ Server Code (app.js / server.js)

```javascript
const express = require('express');
const http = require("http");
const path = require('path');
const socketio = require("socket.io");

const app = express();
const server = http.createServer(app);
const io = socketio(server);

app.set("view engine", "ejs");
app.use(express.static(path.join(__dirname, "public")));

io.on("connection", socket => {
    socket.on("send-location", data =>
        io.emit("recieve-location", { id: socket.id, ...data })
    );

    socket.on("disconnect", () =>
        io.emit("user-disconnected", socket.id)
    );

    console.log("User connected:", socket.id);
});

app.get('/', (req, res) => res.render("index"));

server.listen(3000, () => console.log("Server running on port 3000"));
```

---

## ⭐ Future Improvements

* User login + profile icons
* Route drawing (polyline tracking)
* Speed and distance calculation
* Offline history playback
* Mobile app version (React Native)

---

```

