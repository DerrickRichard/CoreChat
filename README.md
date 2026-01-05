# CoreChat — Professional P2P Messaging & Accountability
[Official Site](https://derrickrichard.github.io/CoreChat/)

## Executive Summary
CoreChat is a high-performance, serverless peer-to-peer messaging framework designed for secure, accountable, and professional communication. By utilizing the **WebRTC protocol**, CoreChat eliminates the need for intermediary servers to store or relay message data. This implementation balances end-to-end privacy with a robust **Local Accountability** system, integrating the CensorCore moderation engine, anti-spam protocols, and persistent session auditing to ensure a productive and safe environment.

## Project Vision and Architecture
The primary objective of CoreChat is to provide a decentralized messaging solution where professional standards are enforced by local persistence. Unlike standard volatile chats, CoreChat preserves session integrity through a **Persistent Audit Log (PAL)**, ensuring that interactions are documented on the participating devices without ever reaching a central database.

### Network Topology and Signaling
CoreChat leverages PeerJS to manage WebRTC complexities. The connection lifecycle includes:
1. **Signaling**: Clients register a unique 6-character ID via a global signaling server to facilitate the initial handshake.
2. **NAT Traversal**: Uses STUN servers to identify public IPs and navigate firewalls for direct connection.
3. **P2P Tunneling**: Once established, a bidirectional DataChannel handles all text and file buffers (up to 2MB) directly between browser memories.

## Moderation & Accountability
CoreChat implements a multi-layered moderation stack to maintain professional standards in a serverless environment.

### CensorCore Moderation Engine
All outgoing content is passed through a pre-transmission hook:
1. **Fuzzy-Logic Validation**: Checks content against weighted dictionaries, detecting "leetspeak" and orthographic obfuscation using Levenshtein distance calculations.
2. **Anti-Spam Protocol**: Rate-limits users to a maximum of 5 messages every 3 seconds to prevent flood-abuse.
3. **Automated Interception**: Prohibited content is discarded before reaching the network layer, preventing the peer from receiving harmful data.

### Persistent Audit Log (PAL)
To ensure accountability, CoreChat maintains a hidden, tamper-resistant log of all activity:
* **Access**: Accessible via the `CTRL + ALT + Q` keyboard shortcut.
* **Tracking**: Records connection timestamps, public IP addresses, file transfer metadata, and moderation violations.
* **Synchronization**: Audit events are synchronized between peers in real-time and saved to `localStorage`, preserving the history even across browser restarts.

## Advanced Feature Specifications

### 1. Rich Media & Communication
* **File & Image Sharing**: Native support for P2P transfers of images and documents up to 2MB with inline previews.
* **Read Receipts**: Visual confirmation (green checkmarks) synced across the DataChannel to track message status.
* **Presence Detection**: Real-time notification when a peer leaves or returns to the chat tab, ensuring engagement transparency.

### 2. Progressive Suspension System
CoreChat features an automated anti-abuse mechanism:
* **Strike Tracking**: Users receive warnings for automated filter hits or manual reports.
* **Timed Suspensions**: After three violations, users are placed in a suspension state (1-5 minutes) managed by local timestamps.
* **Admin Overrides**: A hidden portal (`Alt + Q + W`) allows authorized developers to clear local flags and lift suspensions using a developer key.

## Security and Encryption Standards
* **DTLS/SRTP**: All data transmitted over the DataChannel is encrypted using industry-standard protocols native to WebRTC.
* **Perfect Forward Secrecy**: Keys are negotiated per session; previous sessions remain secure even if future keys are compromised.
* **Local Persistence**: History is stored locally on-device rather than on a remote server, ensuring that data ownership remains with the users while maintaining accountability.

## Technical Requirements
1. **Browser**: Modern evergreen browsers (Chrome 60+, Firefox 55+, Safari 11+, Edge 79+).
2. **Connectivity**: Standard internet access with WebRTC protocol support.
3. **Protocol**: The application must be served over **HTTPS** to allow secure permissions for peer discovery and persistent storage.

---
**Maintained by Derrick Richard**  
*Built with PeerJS, CensorCore, and WebRTC.*
