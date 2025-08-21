# MQTT Chat Application

A real-time chat application built using MQTT protocol for message brokering, with EMQX as the MQTT broker and a modern web frontend.

## Prerequisites

Before you begin, ensure you have the following installed on your system:
- Docker and Docker Compose
- Node.js (v14 or higher)
- MQTTX desktop client

## Installation & Setup

### 1. Install MQTTX
Download and install MQTTX from the official website:
- [MQTTX Download Page](https://mqttx.app/)
- Current stable version: 1.11.1 (or newer)

### 2. Start the EMQX Broker
Run the following command to start the MQTT broker using Docker Compose:

```bash
docker compose -f docker-compose-service-emqx.yml up
```

This will start the EMQX MQTT broker on port 8083 (WebSocket) and 1883 (TCP).

### 3. Build and Start the Frontend
In the project root directory, execute:

```bash
# Install dependencies (if needed)
npm install

# Build the project
npm run build

# Start the development server
npm run dev
```

The application will be available at `http://localhost:5173/`

### 4. Configure MQTTX Client
1. Open MQTTX application
2. Create a new connection with the following parameters:
    - **Name**: Any descriptive name (e.g., "Local Chat")
    - **Host**: `ws://localhost:8083/mqtt`
    - **Port**: 8083
    - **Path**: `/mqtt`
    - Keep other settings as default

3. Click "Connect" to establish the connection

### 5. Subscribe to the Chat Topic
1. After connecting, click on "New Subscription"
2. Set the topic to: `chat/topic`
3. Click "Confirm" to subscribe

## Usage

1. Open your browser to `http://localhost:5173/`
2. Open multiple tabs/windows to simulate multiple users (each tab acts as a separate subscriber)
3. In MQTTX, send messages in the following JSON format:
   ```json
   {
     "msg": "Your message here"
   }
   ```
4. Messages will appear in real-time in all subscribed clients (browser tabs and MQTTX)

## Troubleshooting

- Ensure Docker is running before starting the EMQX container
- Verify the MQTTX connection parameters match exactly
- Check that all services are using the same MQTT topic (`chat/topic`)
- If messages aren't appearing, check that the JSON format is correct
