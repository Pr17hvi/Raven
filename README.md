# Raven

🌐 https://raven-nu.vercel.app

> **Direct peer-to-peer file sharing powered by WebRTC.**
> **No uploads • No accounts • End-to-end encrypted**

Raven is a browser-based peer-to-peer file sharing application that enables users to securely transfer files directly between devices without relying on cloud storage. It uses **WebRTC DataChannels** for high-speed encrypted transfers, while a lightweight **Socket.IO signaling server** is used only to establish the connection.

---


## ✨ Features

* 🚀 Direct browser-to-browser file transfer
* 🔒 End-to-end encrypted communication (WebRTC)
* 📁 Multiple file transfer support
* 📊 Live transfer progress, speed, and status updates
* 🔑 PIN-based device pairing
* ⚡ Chunked transfer with flow control
* 🌐 Cross-platform (Desktop & Mobile)
* ☁️ No cloud storage or file uploads
* 👤 No login or registration required

---

## 🏗️ Architecture

```text
┌─────────────────┐        ┌────────────────────────┐        ┌─────────────────┐
│     Sender      │        │   Signaling Server     │        │    Receiver     │
│   (Browser)     │        │ (Express + Socket.IO)  │        │    (Browser)    │
└────────┬────────┘        └──────────┬─────────────┘        └────────┬────────┘
         │                            │                               │
         │ Create Session             │                               │
         ├───────────────────────────►│                               │
         │                            │                               │
         │◄───────────────────────────┤                               │
         │        Session PIN         │                               │
         │                            │                               │
         │                            │◄──────────────────────────────┤
         │                            │      Join using PIN           │
         │                            │                               │
         │◄───────────────────────────┤──────────────────────────────►│
         │       Receiver Joined      │                               │
         │                            │                               │
         │──────────── SDP Offer ────────────────────────────────────►│
         │◄────────── SDP Answer ─────────────────────────────────────│
         │──────────── ICE Candidates ───────────────────────────────►│
         │◄────────── ICE Candidates ─────────────────────────────────│
         │                            │                               │
         │══════════ Secure WebRTC DataChannel Established ══════════▶│
         │                            │                               │
         │══════════════════ Direct Encrypted File Transfer ═════════▶│
         │                            │                               │
         │      (Server is no longer involved in file transfer)       │
```

The signaling server is responsible only for:

* Creating transfer sessions
* Exchanging SDP offers and answers
* Relaying ICE candidates

Once the connection is established, all file data is transferred **directly between the two devices**.

---

## ⚙️ Tech Stack

### Frontend

* React
* TypeScript
* Vite
* Tailwind CSS
* WebRTC

### Backend

* Node.js
* Express
* Socket.IO
* TypeScript

---

## 📂 Project Structure

```text
Raven/
├── client/
│   ├── public/
│   └── src/
│       ├── components/
│       │   ├── Landing.tsx
│       │   ├── SenderCard.tsx
│       │   └── ReceiverCard.tsx
│       ├── hooks/
│       │   ├── useSocket.ts
│       │   └── useWebRTC.ts
│       ├── services/
│       │   ├── socket.ts
│       │   ├── socketActions.ts
│       │   ├── socketEvents.ts
│       │   ├── webrtc.ts
│       │   └── transfer/
│       │       ├── assembler.ts
│       │       ├── chunker.ts
│       │       ├── flowControl.ts
│       │       ├── IncomingTransfer.ts
│       │       ├── protocol.ts
│       │       ├── receiver.ts
│       │       ├── sender.ts
│       │       ├── transferManager.ts
│       │       └── TransferState.ts
│       ├── types/
│       ├── App.tsx
│       └── main.tsx
│
├── server/
│   └── src/
│       ├── socket/
│       │   └── signaling.ts
│       ├── sessions/
│       │   └── SessionManager.ts
│       ├── utils/
│       │   └── generatePin.ts
│       └── index.ts
│
├── README.md
└── LICENSE
```

---

## 🚀 Getting Started

### Clone the repository

```bash
git clone https://github.com/roshax7/Raven.git
cd Raven
```

---

### Install dependencies

#### Client

```bash
cd client
npm install
```

#### Server

```bash
cd ../server
npm install
```

---

### Configure Environment Variables

#### Client (`client/.env.local`)

```env
VITE_SERVER_URL=http://YOUR_LOCAL_IP:5717
```

> Replace `YOUR_LOCAL_IP` with your machine's local network IP address (for example, `192.168.x.x`) when testing across multiple devices.

The backend currently uses a fixed list of allowed origins. If you change the frontend URL, update the allowed origins in `server/src/index.ts`.

---

### Run the backend

```bash
cd server
npm run dev
```

---

### Run the frontend

```bash
cd client
npm run dev -- --host
```

The application will be available at:

```text
Frontend: 
    Local: http://localhost:5173
    Network:  http://<YOUR_LOCAL_IP>:5173
Backend  : http://localhost:5717

Use the **Network** URL when accessing Raven from another device (e.g., your phone).
```

---

## 🔄 How It Works

1. The sender selects one or more files.
2. A temporary transfer session is created.
3. A unique six-digit PIN is generated.
4. The receiver joins using the PIN.
5. The signaling server exchanges SDP offers, answers, and ICE candidates.
6. A secure WebRTC DataChannel is established.
7. Files are transferred directly between the two devices.

---

## 🔒 Security

* WebRTC DTLS encryption
* Direct peer-to-peer communication
* Files never pass through the server
* Temporary signaling only
* No user accounts
* No persistent storage

---

## 🚀 Performance

Raven transfers files in chunks with built-in flow control for efficient utilization of the WebRTC DataChannel.

Features include:

* Chunked file transfer
* Buffered transmission
* Transfer progress tracking
* Real-time speed calculation
* Sequential multiple-file transfer

Performance depends on the network and device capabilities.

---

## ⚠️ Known Limitations

- Folder transfer is not yet supported.
- Local development across devices requires updating the frontend server URL to match your machine's LAN IP.
- Transfers require both devices to be connected to networks that allow peer-to-peer communication.

---

## 📄 License

This project is licensed under the **MIT License**.

---

<p align="center">
Built with ❤️ using React, TypeScript, WebRTC and Socket.IO.
</p>
