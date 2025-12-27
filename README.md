CoreChat Production Documentation and Technical Specifications
==============================================================
[Official Site](https://derrickrichard.github.io/CoreChat/)

Executive Summary
-----------------

CoreChat is a high-performance, serverless peer-to-peer messaging framework designed for secure, private, and moderated communication. By utilizing the WebRTC protocol, CoreChat eliminates the need for intermediary servers to store or relay message data, ensuring that all conversations remain strictly between the participating endpoints. This implementation integrates the CensorCore moderation engine to provide real-time content filtering and anti-abuse mechanisms at the client level.

Project Vision and Architecture
-------------------------------

The primary objective of CoreChat is to provide a zero-footprint messaging solution where privacy is maintained by the architectural design rather than trust in a service provider. The application functions as a Static Web Application, meaning the entire logic, styling, and networking stack are delivered to the browser and executed locally.

### Network Topology and Signaling

CoreChat leverages PeerJS to manage the complexities of WebRTC. The connection process follows a specific lifecycle:

1.  Signaling: The client connects to a global signaling server to register its unique 6-character ID. This server does not handle message data; it only facilitates the exchange of session descriptions and Interactive Connectivity Establishment candidates.
2.  NAT Traversal: The system utilizes Session Traversal Utilities for NAT servers to identify public IP addresses and navigate through home or corporate firewalls.
3.  Peer-to-Peer Tunneling: Once the handshake is complete, a direct bidirectional DataChannel is established. At this stage, the signaling server is bypassed, and data flows directly from one browser's memory to the other.

CensorCore Moderation Engine
----------------------------

A critical component of the production environment is the integration of the CensorCore Library. This library acts as a local firewall for text content, ensuring that the platform remains safe without requiring a central moderator.

### Content Filtering Logic

The moderation system operates on a pre-transmission hook. When a user attempts to send a message, the following sequence occurs:

1.  The string is passed to the CensorCore validator.
2.  The engine checks the content against a weighted dictionary of prohibited terms.
3.  If a violation is detected, the message is intercepted and discarded before it ever reaches the network layer, preventing the peer from receiving harmful content.

### Fuzzy Match Reporting and Levenshtein Distance

To combat leetspeak or orthographic obfuscation, such as replacing letters with symbols, CoreChat implements a manual reporting system powered by a fuzzy matching algorithm. When a message is flagged by a user:

1.  The system calculates the Levenshtein distance between the flagged text and the prohibited wordlist.
2.  If the similarity index exceeds a predefined threshold, the system confirms the violation.
3.  This allows the software to catch bypassed filters dynamically without requiring constant database updates.

Security and Encryption Standards
---------------------------------

Security in CoreChat is handled at the protocol level through WebRTC's native implementation:

1.  Datagram Transport Layer Security: All data transmitted over the DataChannel is encrypted using DTLS, providing the same level of security as TLS used in standard web browsing.
2.  Perfect Forward Secrecy: Keys are negotiated for each session, ensuring that even if a future key were compromised, previous sessions remain secure.
3.  Local Privacy: Since there is no database, message history is volatile. Refreshing the browser or ending the chat permanently wipes the conversation from the device's random access memory.

Advanced Feature Specifications
-------------------------------

### Progressive Suspension System

CoreChat features an automated anti-abuse system that tracks user behavior through a strike-based mechanism:

1.  Initial Violations: Users receive a local warning and a temporary block on outgoing messages.
2.  Threshold Escalation: After three confirmed violations, either through automated filtering or fuzzy-match reports, the user is placed in a suspension state.
3.  Cooldown Logic: Suspensions are timed, typically ranging from one to three minutes, and managed via timestamps stored in the browser state, preventing immediate reconnection as a bypass.

### Interface and Experience Design

The user interface is constructed using modern CSS3 standards, focusing on a dark-mode aesthetic that reduces eye strain and fits professional software standards:

1.  Responsive Grid: The layout adapts from wide-screen desktop monitors to mobile devices using CSS Flexbox and Grid.
2.  Real-Time Status Monitoring: A dedicated diagnostics pill tracks the state of the signaling server and the active peer connection.
3.  Persistence: User preferences, such as display names and local moderation statistics, are stored in localStorage to provide a seamless experience across sessions.

Developer and Administrative Operations
---------------------------------------

### Administrative Overrides

For quality assurance and development testing, a hidden administrative portal is embedded within the application. This allows developers to reset local states without manually clearing browser caches.

1.  Access Protocol: Users must input the keyboard sequence Alt + Q + W to trigger the authorization prompt.
2.  Authorization: Entry of the Developer Key resets the local flag count and clears any active suspension timestamps.

### Deployment Guidelines

CoreChat is optimized for high-availability static hosting. Because it requires no backend compute, it can be deployed to:

1.  Content Delivery Networks for global low-latency access.
2.  GitHub Pages or GitLab Pages for version-controlled hosting.
3.  Specialized static hosts like Vercel, Netlify, or Cloudflare Pages.

### Maintenance

Maintenance is simplified due to the lack of a database. Updates to the prohibited wordlist or networking logic only require the deployment of a new HTML file. The application is designed to be self-sustaining as long as the PeerJS signaling cloud or a private PeerServer instance is reachable.

Technical Requirements
----------------------

1.  Browser: Modern evergreen browsers including Chrome 60+, Firefox 55+, Safari 11+, and Edge 79+.
2.  Connectivity: Standard internet access with support for WebRTC protocols.
3.  Protocol: The application must be served over HTTPS to allow WebRTC permissions for peer discovery.
