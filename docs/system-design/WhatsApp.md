# Designing Chat Systems: Unified Approach for 1-to-1 and Group Chats

Chat systems are essential components of modern communication platforms, supporting a wide range of use cases from social messaging apps to collaboration tools. When designing a scalable chat system, one must consider functional requirements like supporting **both 1-to-1 chat and group chat** while optimizing for performance, scalability, and simplicity.

In this article, we will discuss the functional requirements of such a system, compare different approaches (unified vs specialized models), and dive into a detailed design using a **unified model** where 1-to-1 chats are treated as a special case of group chats. Additionally, we'll utilize a **key-value store** for storing chat metadata and **Cassandra** for storing chat messages.

## Functional Requirements

To design a scalable and robust chat system, the following functional requirements must be addressed:

1. **1-to-1 Chat**:
   - Real-time messaging between two users.
   - Chat history persists.
   - Media (e.g., images, videos) can be shared.
   - Supports features like read receipts and typing indicators.

2. **Group Chat**:
   - Supports real-time messaging between multiple users.
   - Chat history persists for all participants.
   - Group-specific metadata, such as group name, description, and a list of participants.
   - Supports read receipts, typing indicators, and notifications.

3. **System Requirements**:
   - Efficient handling of high throughput for both read-heavy and write-heavy workloads.
   - Messages must be deleted automatically after 90 days.
   - Separation of 1-to-1 and group chat functionalities while maintaining unified scalability and performance.

## Unified vs Specialized Model: A Comparison

The two primary architectural approaches for supporting both 1-to-1 and group chats are:

| **Feature**                  | **Unified Model**                            | **Specialized Model**                        |
|------------------------------|----------------------------------------------|---------------------------------------------|
| **Architecture**              | Treats 1-to-1 chat as a special case of group chat. | Separate systems for 1-to-1 and group chats. |
| **Codebase Complexity**       | Simpler, consistent codebase.                | More complex due to maintaining two distinct systems. |
| **Scalability**               | Scales uniformly across both chat types.     | Each system must scale independently. |
| **Feature Development**       | Single implementation for features like media sharing, read receipts, etc. | Features must be implemented separately for 1-to-1 and group chats. |
| **Transition from 1-to-1 to Group** | Not possible; 1-to-1 remains separate. | 1-to-1 chat can evolve into a group chat. |
| **Operational Complexity**    | Lower, with a unified system.                | Higher, as two systems need separate management. |
| **Resource Efficiency**       | More efficient overall, with a unified infrastructure. | Less efficient, due to duplicated efforts for similar features. |

## Choosing the Unified Model

Given the comparison, we will proceed with a **unified model**, where **1-to-1 chats are treated as a special case of group chats**. This approach simplifies development, operational complexity, and scalability.

## Key-Value Store for Chat Metadata

To manage the chat metadata, such as the name, description, and participants, we will use a **key-value store** (e.g., **Redis** or **DynamoDB**) to store metadata for each chat session.

- **For 1-to-1 Chats**: The metadata will only include the participants.
- **For Group Chats**: The metadata will include the group name, description, and participants.

### Example Metadata Schema

**1-to-1 Chat Metadata**:

```json
{
  "chat_id": "chat12345",
  "type": "1-to-1",
  "participants": ["userA", "userB"]
}
```

**Group Chat Metadata**:

```json
{
  "chat_id": "group67890",
  "type": "group",
  "name": "Friends Group",
  "description": "Chat with close friends",
  "participants": ["userA", "userB", "userC"]
}
```

## Identifying Chat Type

To differentiate between **1-to-1 chats and group chats**, the `type` field in the metadata will be used. **Once a 1-to-1 chat is initiated, it cannot be converted into a group chat**, keeping the two types distinct.

- **1-to-1 Chats** will be identified with the `type: "1-to-1"` tag in the metadata.
- **Group Chats** will have `type: "group"`.

## Storing Chat Messages in Cassandra

We'll use **Cassandra** to store chat messages for both 1-to-1 and group chats. Cassandra is a distributed NoSQL database that provides high write throughput, low-latency read operations, and automatic data replication across multiple nodes.

## Table Design for Messages in Cassandra

In Cassandra, messages will be stored using the **groupId (chatId)** as the partition key and **timestamp** as the clustering key. This ensures that all messages for a specific chat (1-to-1 or group) are stored together and ordered by time.

```sql
CREATE TABLE IF NOT EXISTS messages (
    groupId TEXT,
    messageId UUID,
    senderId TEXT,
    message TEXT,
    mediaUrl TEXT,
    timestamp TIMESTAMP,
    PRIMARY KEY (groupId, timestamp)
) WITH CLUSTERING ORDER BY (timestamp ASC);
```

- **Partition Key (groupId)**: The `groupId` identifies the chat session. This can be the `chatId` for both 1-to-1 chats and group chats.
- **Clustering Key (timestamp)**: Messages within a chat are clustered by `timestamp`, which allows for efficient retrieval of messages in chronological order.

## Example Message Insertion

Here’s an example of inserting a new message into the `messages` table in Cassandra:

```sql
INSERT INTO messages (groupId, messageId, senderId, message, mediaUrl, timestamp)
VALUES ('group67890', uuid(), 'userA', 'Hello, Group!', NULL, '2024-08-24 12:34:56');
```

This inserts a message from `userA` into the group with `groupId = 'group67890'`.

## Automatic Deletion of Messages after 90 Days

Cassandra supports **Time-to-Live (TTL)** functionality, which allows messages to be automatically deleted after a certain period. For our chat system, we will set a TTL of **90 days** (7,776,000 seconds).

When inserting a message, we will specify the TTL:

```sql
INSERT INTO messages (groupId, messageId, senderId, message, mediaUrl, timestamp)
VALUES ('group67890', uuid(), 'userA', 'Hello, Group!', NULL, '2024-08-24 12:34:56')
USING TTL 7776000;  -- 90 days in seconds
```

This ensures that the message will be automatically deleted after 90 days.

## Interaction Between User Clients and Backend Using WebSockets

### Client-Side (User Interaction with WebSockets)

Each user connects to the chat backend via a WebSocket connection, establishing real-time communication for sending and receiving messages.

```javascript
const socket = new WebSocket('ws://chat-server-url.com');
socket.onopen = function(event) {
    console.log('WebSocket connection established');
};

socket.onmessage = function(event) {
    const message = JSON.parse(event.data);
    console.log('Message received:', message);
};

socket.onclose = function(event) {
    console.log('WebSocket connection closed');
};
```

## WebSocket Manager with Redis for Connection Tracking

The **WebSocket Manager Application** plays a critical role in keeping track of which users are connected to which WebSocket servers. We use **Redis** as a central store to maintain this mapping.

### Tracking User Connections

- **WebSocket Servers** register user connections when a user connects or disconnects.
- The **WebSocket Manager** updates **Redis** with the user-to-server mappings. For example, the key could be the user ID, and the value is the ID of the WebSocket server to which the user is connected.

Example Redis structure:

```
user_connections:
  userA -> ws_server_1
  userB -> ws_server_2
  userC -> ws_server_1
```

### Flow of Connection Tracking

1. **User Connects to a WebSocket Server**:
   - The WebSocket server establishes the connection and notifies the WebSocket Manager.

2. **WebSocket Manager Updates Redis**:
   - The WebSocket Manager stores the user-to-server mapping in Redis.
   - Example: `SET userA ws_server_1`

3. **User Disconnects**:
   - When the user disconnects, the WebSocket server notifies the WebSocket Manager, which removes the user-to-server mapping from Redis.
   - Example: `DEL userA`

## Message Flow from User to Message Servers and Participants

1. **User Sends a Message**:
   - The user sends a message through the **WebSocket connection** to their connected **WebSocket server**.
   - Example Message:

     ```json
     {
       "type": "message",
       "chat_id": "group67890",
       "sender_id": "userA",
       "message": "Hello, Group!",
       "timestamp": "2024-08-24 12:34:56"
     }
     ```

2. **WebSocket Server Forwards to Message Server**:
   - The WebSocket server forwards the message to the **Message Server** responsible for handling message persistence and routing.
   - The Message Server persists the message in **Cassandra** and starts the process of distributing the message to participants.

3. **Message Server Persists Message in Cassandra**:
   - The **Message Server** inserts the message into the **Cassandra** database under the corresponding `chat_id` (group or 1-to-1 chat) partition.
   - Example Cassandra Insertion:

     ```sql
     INSERT INTO messages (groupId, messageId, senderId, message, mediaUrl, timestamp)
     VALUES ('group67890', uuid(), 'userA', 'Hello, Group!', NULL, '2024-08-24 12:34:56')
     USING TTL 7776000;
     ```

4. **Message Server Identifies Participants**:
   - The Message Server queries the **chat metadata** from the **key-value store** (e.g., Redis or DynamoDB) to retrieve the list of participants for that chat.
   - For group chats, the participants list includes all users in the group.
   - For 1-to-1 chats, the participants are the two users involved in the conversation.

5. **Message Server Checks Online Status Using Redis**:
   - The Message Server checks **Redis** (maintained by the WebSocket Manager) to determine which participants are currently online and connected to WebSocket servers.
   - Example Redis Query:

     ```shell
     GET userB
     ```

   - If the user is online, Redis returns the WebSocket server they are connected to (e.g., `ws_server_2`).

6. **Message Server Forwards Message to Online Participants**:
   - For each participant who is online, the Message Server forwards the message to the appropriate **WebSocket server**.
   - The WebSocket server then delivers the message to the connected client over the existing WebSocket connection.
   - Example of Message Forwarding:

     ```json
     {
       "type": "message",
       "chat_id": "group67890",
       "sender_id": "userA",
       "message": "Hello, Group!",
       "timestamp": "2024-08-24 12:34:56"
     }
     ```

7. **Handling Offline Participants**:
   - If a participant is **offline**, the Message Server does not immediately deliver the message. Instead, it stores the message as undelivered and waits for the participant to reconnect.
   - When the user reconnects, the WebSocket server queries the undelivered messages and forwards them.

## Message Forwarding Flow

1. **User sends a message** to their WebSocket server.
2. **WebSocket server** forwards the message to the Message Server.
3. **Message Server persists the message** in Cassandra.
4. **Message Server retrieves participants** from chat metadata and checks **Redis** for online status.
5. **Message Server forwards the message** to the relevant WebSocket servers based on the user-to-server mappings in Redis.
6. **WebSocket servers deliver the message** to online participants via their open WebSocket connections.

If a participant is **offline**, the Message Server holds the message until the participant reconnects, ensuring eventual delivery once they are back online.

## Conclusion

By combining **WebSockets** for real-time communication, **Redis** for managing user-to-server mappings, and **Cassandra** for message persistence, we can create a scalable, reliable chat system that efficiently handles both **1-to-1 and group chats**. The unified model simplifies development, and WebSockets ensure that participants receive messages in real-time. This architecture ensures high availability, low latency, and the ability to handle large-scale chat systems with millions of users.
