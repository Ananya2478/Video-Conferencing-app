# 📹 VideoConnect — Real-Time Video Conferencing App

A full-stack video conferencing web application with real-time peer-to-peer video/audio communication, live chat, and user authentication — built with React, Node.js, Socket.IO, and WebRTC.

![Tech Stack](https://img.shields.io/badge/React-18-61DAFB?style=flat&logo=react) ![Node](https://img.shields.io/badge/Node.js-Express-339933?style=flat&logo=node.js) ![Socket.IO](https://img.shields.io/badge/Socket.IO-4.x-010101?style=flat&logo=socket.io) ![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-47A248?style=flat&logo=mongodb)

---

## ✨ Features

- 🔐 **User Authentication** — Register & login with bcrypt-hashed passwords and token-based sessions
- 📹 **Peer-to-Peer Video Calls** — Multi-participant video/audio using WebRTC (no third-party media server required)
- 💬 **Real-Time In-Room Chat** — Socket.IO powered live messaging during calls
- 🎤 **Media Controls** — Toggle mute, camera on/off mid-call
- 🔑 **Meeting Rooms** — Create or join rooms via unique room codes
- 📋 **Meeting History** — All joined meetings saved to MongoDB per user
- 📋 **One-Click Code Copy** — Share room codes instantly from the meeting UI

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18, React Router v6, Socket.IO Client |
| Backend | Node.js, Express.js |
| Real-Time | Socket.IO (signaling), WebRTC (P2P media) |
| Database | MongoDB Atlas via Mongoose |
| Auth | bcrypt (password hashing), crypto (session tokens) |

---

## 📂 Project Structure

```
VideoConferencing/
├── backend/
│   ├── .env                          ← Environment variables
│   ├── package.json
│   └── src/
│       ├── app.js                    ← Express server + MongoDB connection
│       ├── controllers/
│       │   ├── socketManager.js      ← WebRTC signaling (offer/answer/ICE)
│       │   └── users.controllers.js  ← Register, Login, Meeting Activity
│       ├── models/
│       │   ├── user.model.js         ← User schema
│       │   └── meeting.model.js      ← Meeting history schema
│       └── routes/
│           └── users.routes.js       ← REST API routes
└── frontend/
    ├── package.json
    └── src/
        ├── App.js                    ← Route definitions + PrivateRoute guard
        ├── contexts/
        │   └── AuthContext.js        ← Global auth state
        ├── pages/
        │   ├── AuthPage.js           ← Login / Register UI
        │   ├── HomePage.js           ← Dashboard (create/join meeting)
        │   └── MeetingRoom.js        ← Live video call room
        └── styles/
            └── global.css
```

---

## ⚙️ How It Works — WebRTC Signaling Flow

```
User A joins room                  User B joins room
      │                                   │
      ├──── join-room ──► Server ◄──── join-room ──┤
      │                     │                      │
      │◄── existing-users ──┘     user-joined ────►│
      │                                            │
      ├──────────── offer ──────────────────────► │
      │◄─────────── answer ────────────────────── │
      │◄──────────► ICE candidates ──────────────►│
      │                                            │
      └──────────── P2P Media Stream ─────────────┘
                 (WebRTC, no relay server)
```

The Node.js/Socket.IO server acts purely as a **signaling server** — once peers exchange SDP offers/answers and ICE candidates, all audio and video flows directly between browsers.

---

## 🚀 Getting Started

### Prerequisites
- Node.js v18+
- A [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) account (free tier works)

### 1. Clone the repository
```bash
git clone https://github.com/your-username/video-conferencing.git
cd video-conferencing
```

### 2. Configure environment variables
```bash
cd backend
cp .env.example .env
```
Edit `.env` and fill in your values:
```env
MONGO_URI=mongodb+srv://<username>:<password>@cluster0.xxxx.mongodb.net/videoconferencing
FRONTEND_URL=http://localhost:3000
PORT=8000
```

> **MongoDB Atlas setup:** Go to *Network Access* → Add your IP. Go to *Database Access* → confirm your user credentials.

### 3. Start the backend
```bash
cd backend
npm install
npm run dev
```
You should see:
```
✅ MongoDB Connected: cluster0.xxxx.mongodb.net
🚀 Server listening on PORT 8000
```

### 4. Start the frontend
```bash
# in a new terminal
cd frontend
npm install
npm start
```
App opens at **http://localhost:3000**

---

## 🔌 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/v1/users/register` | Create a new account |
| `POST` | `/api/v1/users/login` | Login, returns session token |
| `POST` | `/api/v1/users/add_to_activity` | Save a meeting to history |
| `GET` | `/api/v1/users/get_all_activity` | Fetch meeting history for user |

---

## 🔌 Socket Events

| Event | Direction | Description |
|---|---|---|
| `join-room` | Client → Server | Join a meeting room |
| `existing-users` | Server → Client | List of users already in room |
| `user-joined` | Server → Client | Notify when a new user joins |
| `offer` / `answer` | Client ↔ Server ↔ Client | WebRTC SDP signaling |
| `ice-candidate` | Client ↔ Server ↔ Client | WebRTC ICE exchange |
| `send-message` | Client → Server | Send a chat message |
| `receive-message` | Server → Client | Broadcast chat to room |
| `user-left` | Server → Client | Notify when a participant disconnects |

---

## 🐛 Common Issues

| Error | Fix |
|---|---|
| `Authentication failed` | Check your MongoDB password in `.env` |
| `IP not whitelisted` | Add your IP in Atlas → Network Access |
| `CORS error in browser` | Ensure backend is running on port 8000 |
| `Cannot find module` | Run `npm install` in both `frontend/` and `backend/` |
| Camera/mic not working | Allow browser permissions for camera and microphone |

---

## 🔮 Possible Future Improvements

- [ ] Screen sharing support
- [ ] JWT-based authentication (replace random token)
- [ ] TURN server integration for NAT traversal
- [ ] Recording functionality
- [ ] Room password protection

---

## 📄 License

This project is for educational purposes. MIT License.
