# EdgeCast Pro

EdgeCast Pro is a secure browser-based Peer-to-Peer (P2P) video streaming platform built using **WebRTC** and **MQTT signaling**. It allows users to broadcast video directly from their browser and lets other users discover and watch those streams in real time.

Unlike traditional streaming services, EdgeCast Pro does not require a central media server. Instead, the video is transmitted **directly between browsers** using WebRTC, making it efficient and lightweight.

---

# Features

- Secure P2P video streaming
- WebRTC real-time media transmission
- MQTT signaling network
- Stream discovery system
- Stream key authentication
- Multiple viewers supported
- Modern dark UI
- Browser-based (no installation required)

---

# Technologies Used

## HTML
Used to build the structure of the application including:
- Sidebar
- Video player
- Control buttons
- Channel list

## CSS
Provides the visual interface including:
- Dark themed UI
- Responsive layout
- Animated live indicators
- Panel and control styling

## JavaScript
Handles all application logic such as:
- WebRTC connection management
- MQTT messaging
- Media streaming
- UI interactions

## WebRTC
WebRTC is responsible for **real-time peer-to-peer video streaming**.

Responsibilities:
- Direct video transmission between browsers
- ICE candidate negotiation
- Secure media transport

This removes the need for a traditional streaming server.

## MQTT

MQTT is used as the **signaling system** that helps peers find each other and establish WebRTC connections.

Responsibilities:
- Discover active streams
- Exchange WebRTC offers and answers
- Send ICE candidates
- Manage peer connection signaling

Broker used:

```
wss://broker.emqx.io:8084/mqtt
```

## STUN Server

A STUN server helps devices behind routers or NAT networks find their public address so peers can connect.

Server used:

```
stun:stun.l.google.com:19302
```

---

# How It Works

The system works in four main stages.

## 1. Network Initialization

When the page loads:

- A unique node ID is generated
- The client connects to the MQTT broker
- The client subscribes to signaling topics

Topics used:

```
edgecast/pro/channels
edgecast/signal/{nodeId}
```

---

## 2. Broadcasting

Steps:

1. User loads a video file.
2. The video starts playing locally.
3. The browser captures the video stream using:

```
video.captureStream()
```

4. The broadcaster announces the stream to the network.

Example broadcast message:

```
{
  casterId: nodeId,
  name: "Pro Stream"
}
```

This appears in the viewer's **Active Streams list**.

---

## 3. Stream Discovery

Viewers subscribe to the global channel topic:

```
edgecast/pro/channels
```

When a broadcaster starts streaming, the channel appears in the sidebar.

Users can click the stream to start viewing.

---

## 4. WebRTC Connection

After a viewer selects a stream:

1. Viewer creates a WebRTC **Offer**
2. Broadcaster receives the offer
3. Broadcaster sends an **Answer**
4. Both peers exchange **ICE candidates**
5. A direct P2P connection is established

Once connected, the video stream flows directly between browsers.

---

# Security System

EdgeCast Pro uses a **Stream Security Key**.

Each signaling message contains the key:

```
payload.securityKey
```

If the viewer key does not match the broadcaster key:

```
Connection request is rejected
```

This prevents unauthorized users from accessing the stream.

---

# Project Architecture

```
Viewer Browser
      |
      | WebRTC Video Stream
      |
Broadcaster Browser

      |
      | MQTT Signaling
      |
MQTT Broker
```

The broker is only used for signaling.  
Video data never passes through the broker.

---

# Use Cases

EdgeCast Pro can be used for:

### Private Video Streaming
Share videos privately without uploading to streaming platforms.

### Classroom Broadcasting
Teachers can stream lessons directly to students.

### Internal Company Streaming
Teams can share presentations or training videos.

### LAN Media Sharing
Stream movies or videos within a local network.

### Decentralized Streaming Systems
Build distributed streaming platforms without centralized servers.

---

# Advantages

- No central streaming server required
- Lower bandwidth cost
- Real-time connections
- Lightweight architecture
- Easy browser deployment

---

# Future Improvements

Possible upgrades include:

- Live webcam streaming
- Chat system
- Stream recording
- Authentication system
- TURN server support
- Adaptive bitrate streaming
- Mobile interface improvements

---

# How To Run

1. Download or clone the project.
2. Open the HTML file in a modern browser.
3. Load a video file.
4. Click **Broadcast**.
5. Share the **stream key** with viewers.

Viewers can open the same page and connect to the stream.

---

# License

This project is open source and intended for learning WebRTC, MQTT signaling, and decentralized streaming architectures.
