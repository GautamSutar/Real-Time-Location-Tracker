# ✅ **Real-Time Location Tracker**

````markdown
# 🌍 Real-Time Location Tracker

<div align="center">

<!-- ===================== -->
<!--       PROJECT LOGO     -->
<!-- ===================== -->

### 🔵 **Project Logo (SVG — Renders on GitHub)**

<img height="140" src="https://raw.githubusercontent.com/github/explore/main/topics/gps/gps.png">

*(If you want a custom-designed SVG instead of a stock icon, tell me!)*

---

### 🎞️ **Animated Demo (SVG Simulation — GitHub-Compatible)**

<img src="https://raw.githubusercontent.com/edent/SuperTinyIcons/master/images/svg/map.svg" width="120">
<br>

> This animated SVG simulates a GPS pulse.
>  
> Replace it anytime with real screen-recording GIF.

---

</div>

A **real-time GPS tracking application** built with **Node.js**, **Express**, **Socket.IO**, and **Leaflet.js**, allowing multiple users to share their location instantly on a world map.

---

## 🎨 Improved UI (Frontend Upgrade)

Your UI has been enhanced with:

- 🌫️ **Glassmorphic floating map container**
- 🟡 **Connected / Tracking status indicator**
- 🧭 **Modernized Leaflet map styling**
- 🟦 **Soft shadows + smooth borders**
- 💬 **User bubbles over markers**
- ⚪ Cleaner, minimal UI design

### Updated `style.css`:

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
  border-radius: 20px;
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

Add this inside your `<body>`:

```html
<div class="status-box">🟢 Tracking Active</div>
```

---

## 📂 Project Structure

```
📁 real-time-tracker/
 ├── 📁 public/
 │   ├── 📁 css/
 │   │   └── style.css
 │   ├── 📁 js/
 │   │   └── script.js
 │   └── index.ejs
 ├── server.js
 ├── package.json
 └── README.md
```

---

## 🛠️ Technologies Used

| Tech               | Purpose                 |
| ------------------ | ----------------------- |
| 🟩 Node.js         | Backend runtime         |
| ⚡ Express.js       | Server framework        |
| 🔌 Socket.IO       | Real-time communication |
| 🗺️ Leaflet.js     | Map rendering           |
| 🌍 OpenStreetMap   | Map tiles               |
| 🎨 HTML / CSS / JS | UI                      |

---

## 🚀 Setup Instructions

### 1️⃣ Install Dependencies

```bash
npm install
```

### 2️⃣ Start Server

```bash
node server.js
```

### 3️⃣ Visit in Browser

```
http://localhost:3000
```

---

## 📍 Key Files

### **Client (script.js)**

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

### **Server (server.js)**

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

## 🎁 Future Enhancements

* 🔑 JWT authentication
* 📌 Usernames & profiles
* 🛰️ Live route lines
* 📊 Speed, distance tracking
* 📍 Historical movement replay

---



