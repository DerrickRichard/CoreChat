# CoreChat | P2P Moderated Messaging

A professional, static-site implementation of a serverless P2P chat client. This project demonstrates direct browser-to-browser communication integrated with the **CensorCore** moderation ecosystem.

## Live Deployment
This application is designed to be hosted as a static site.
- **Environment**: Compatible with GitHub Pages, Vercel, Netlify, or any static file host.
- **Zero Maintenance**: No database or backend logic to manage; the infrastructure is entirely client-side.

## Architecture
The application utilizes a **Decentralized Signaling** model:
1.  **Signaling**: Uses the PeerJS cloud to facilitate the initial handshake between two browsers.
2.  **Data Channel**: Once established, a WebRTC `DataChannel` is opened for direct, encrypted message passing.
3.  **Local State**: All chat history, bans, and identity settings are stored in volatile memory or `localStorage`.

## Moderation Engine (CensorCore)
The site uses a multi-layered approach to content safety:
-   **Pre-flight Filtering**: Outgoing messages are scanned against the `CensorCore` library wordlist before transmission.
-   **Fuzzy Logic Reporting**: A Levenshtein distance algorithm identifies "look-alike" words that attempt to bypass filters.
-   **Automated Suspensions**: A strike-based system that implements time-based messaging locks (1–3 minutes) based on violation frequency.

## Developer Operations
### **Static Configuration**
-   **Main Entry**: `index.html` (contains all styles, logic, and structure).
-   **Dependencies**: Fetched via CDN (PeerJS, CensorCore Library).

## Security & Privacy
-   **No Logs**: Because there is no backend, message logs are impossible to retrieve once a session is closed.
-   **WebRTC Encryption**: All data transmitted via the PeerJS DataChannel is encrypted by default using the underlying WebRTC security protocols (DTLS/SRTP).

---
**Project Maintainer**: [Derrick Richard](https://derrickrichard.github.io/profile)
**Moderation Library | CensorCore**: [CensorCore](https://github.com/DerrickRichard/CensorCore-Library)
