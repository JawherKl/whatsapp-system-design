# WhatsApp System Design

## Overview
This project is an implementation of a WhatsApp-like messaging system using **Node.js**, **PostgreSQL**, **Kafka**, **WebSockets**, and **Firebase/Apple Push Notification Services**. It models users, groups, chats, and messages, while handling real-time messaging, group functionalities, and notification services.

The architecture follows a microservices approach, with separate services for handling users, messaging, notifications, and file uploads.

## Table of Contents
- [Architecture](#architecture)
- [System Components](#system-components)
- [Database Schema](#database-schema)
- [Technologies Used](#technologies-used)
- [Installation](#installation)
- [Usage](#usage)
- [License](#license)

---

## Architecture

![System Design Diagram](whatsapp-advanced-design.png)

The system consists of the following components:

1. **API Gateway**: The entry point for all external requests. It handles user authentication and forwards requests to respective microservices.
   
2. **User Service**: Manages user registration, authentication, and user-related data.
   
3. **Chat Service**: Manages chats, messages, and groups. It interacts with the WebSocket connection for real-time communication and stores chat history in the database.
   
4. **Notification Service**: Responsible for sending push notifications via **Firebase Cloud Messaging (FCM)** and **Apple Push Notification Service (APNS)**.
   
5. **Media Service**: Handles file uploads and serves media content like images, videos, and documents.

6. **Kafka**: Used for message queueing and asynchronous processing of chat events.

7. **WebSocket Connection**: For real-time messaging between users.

## System Components

### 1. **API Gateway**
- **Handles**: HTTP requests (RESTful).
- **Responsibilities**: 
  - Validates incoming requests.
  - Routes requests to appropriate services.
  - Manages authentication and authorization.

### 2. **User Service**
- **Handles**: User registration, authentication, and profile management.
- **Database**: Stores user details in PostgreSQL.
- **Endpoints**:
  - `/register`: User signup.
  - `/login`: User authentication.

### 3. **Chat Service**
- **Handles**: Messaging, group chats, and message status (sent, delivered, read).
- **Database**: Stores chat history in PostgreSQL.
- **Kafka**: Publishes messages to Kafka for real-time delivery.
- **WebSocket**: Ensures real-time communication between users.

### 4. **Notification Service**
- **Handles**: Push notifications for new messages.
- **Technology**: Uses Firebase Cloud Messaging (FCM) and Apple Push Notification Service (APNS).

### 5. **Media Service**
- **Handles**: File uploads (images, documents, etc.).
- **Storage**: Stores files in an object storage service (like AWS S3 or local storage).

## Database Schema

![Database Schema](whatsapp-datamodel.png)

### Main Entities

- **users**: Stores information about users, such as `name`, `phoneNumber`.
- **groups**: Stores group chat details.
- **users_groups**: A junction table connecting users to groups.
- **chats**: Stores chat sessions (one-on-one or group chats).
- **users_chats**: A junction table connecting users to chats.
- **messages**: Stores messages sent in chats, with delivery and read statuses.

## Technologies Used

- **Node.js**: Backend server and API development.
- **Express.js**: Web framework for building APIs.
- **PostgreSQL**: Relational database for storing user, group, chat, and message data.
- **Sequelize**: ORM for PostgreSQL database interaction.
- **Kafka**: Message queue for handling real-time messages and events.
- **Socket.IO**: WebSockets for real-time communication.
- **Firebase Cloud Messaging (FCM)** and **APNS**: For sending notifications to mobile devices.
- **Object Storage**: For storing and serving media files.

## Installation

### Prerequisites
- Node.js
- PostgreSQL
- Kafka (optional for message queuing)
- Firebase or APNS account for notifications

### Steps

1. **Clone the repository**:
   ```bash
   git clone https://github.com/yourusername/whatsapp-system-design.git
   cd whatsapp-system-design
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Configure environment variables**: Create a `.env` file in the root directory with the following:
   ```env
   DATABASE_URL=postgres://username:password@localhost:5432/your_database
   JWT_SECRET=your_jwt_secret
   KAFKA_BROKER=kafka-broker-url
   FCM_SERVER_KEY=your_fcm_key
   APNS_KEY=your_apns_key
   ```

4. **Run the PostgreSQL migrations**:
   ```bash
   npx sequelize-cli db:migrate
   ```

5. **Start the server**:
   ```bash
   npm start
   ```

## Usage

- **Register User**: 
  - `POST /api/register`
  - Payload: `{ "name": "User Name", "phoneNumber": "1234567890" }`

- **Send a Message**: 
  - `POST /api/messages`
  - Payload: `{ "chatID": "chat-uuid", "content": "Hello World", "type": "text" }`

- **WebSocket Communication**:
  - Clients connect via WebSocket for real-time messaging.
  - URL: `ws://your-server-url/ws`

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```
Make sure to update paths like `./path/to/your/architecture-image.png` and `./path/to/your/database-image.png` with the actual paths to your images.
```
