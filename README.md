<div align='center'>HT-baileys (Heavstal Tech Edition)</div>

<div align="center">
  <img src="https://raw.githubusercontent.com/Bell575/Upload/main/uploads/1742387351904.png" alt="HT-baileys Banner" width="100%" />

  <br/>
  <br/>

  <a href="https://heavstal-tech.vercel.app">
    <img src="https://img.shields.io/badge/Maintained_By-Heavstal_Tech-blue?style=for-the-badge&logo=vercel&logoColor=white" alt="Heavstal Tech" />
  </a>
  <a href="https://www.npmjs.com/package/@heavstaltech/baileys">
    <img src="https://img.shields.io/badge/NPM-@heavstaltech/baileys-red?style=for-the-badge&logo=npm&logoColor=white" alt="NPM" />
  </a>
  <a href="https://github.com/Promise818/HT-baileys">
    <img src="https://img.shields.io/badge/Version-1.0.0-green?style=for-the-badge&logo=github&logoColor=white" alt="Version" />
  </a>
   <img src="https://img.shields.io/badge/License-MIT-orange?style=for-the-badge" alt="License" />

</div>

---

## 🦅 Introduction

**HT-baileys** is the definitive, high-performance fork of the WhatsApp Web API library, re-engineered by **Heavstal Tech** (Promise818).

While the original Baileys is a masterpiece of reverse engineering, **HT-baileys** is built for **Production-Grade Bots**. We took the stability of the original, integrated the feature-rich enhancements of `baileys-pro`, and injected our own proprietary **Anti-Ban Architecture** and **Custom Pairing Logic**.

### ⚡ Why HT-baileys?

| Feature | Original Baileys | HT-baileys |
| :--- | :---: | :---: |
| **Stability** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ (Optimized for uptime) |
| **Pairing Codes** | Random (ABCD-1234) | **Customizable** (HEAV-STAL) |
| **Anti-Ban** | Standard Headers | **Stealth Headers** (Browser Masquerading) |
| **Newsletters** | Basic Support | **Full Management** (Create, Edit, Follow) |
| **Interactive Msg**| Limited | **Native Flow Support** (Buttons, Lists) |
| **Media Handling** | Standard | **Album Support** (Grouped Media) |

---

## 🚀 Installation

Ensure you have NodeJS (v16+) installed.

### Stable Release (Recommended)
```bash
npm install @heavstaltech/baileys
```

### Edge Release (Latest Features)
```bash
npm install @heavstaltech/baileys@latest
```

---

## 💻 Quick Start

Here is the minimal code to get a socket running with the **Heavstal Configuration**.

```ts
import makeWASocket, { 
    useMultiFileAuthState, 
    DisconnectReason, 
    Browsers 
} from '@heavstaltech/baileys'

async function startHeavstalBot() {
    const { state, saveCreds } = await useMultiFileAuthState('auth_info_baileys')

    const sock = makeWASocket({
        auth: state,
        printQRInTerminal: true, // Set to false if using Pairing Code
        browser: Browsers.macOS("Desktop"), // Masquerade as Desktop to reduce bans
        syncFullHistory: true
    })

    sock.ev.on('connection.update', (update) => {
        const { connection, lastDisconnect } = update
        if(connection === 'close') {
            const shouldReconnect = (lastDisconnect.error)?.output?.statusCode !== DisconnectReason.loggedOut
            console.log('Connection closed. Reconnecting...', shouldReconnect)
            if(shouldReconnect) startHeavstalBot()
        } else if(connection === 'open') {
            console.log('🦅 HEAVSTAL CORE ONLINE')
        }
    })

    sock.ev.on('creds.update', saveCreds)
}

startHeavstalBot()
```

---

## 💎 Exclusive Features

### 1. Custom Pairing Code (The Legend Feature)
Unlike standard libraries that force random codes, HT-baileys allows you to define your own logic for branding.

```javascript
if (!sock.authState.creds.registered) {
    const phoneNumber = "2349000000000"
    
    // The Heavstal Logic: 
    // This allows branding the pairing experience.
    // Note: The code format depends on WhatsApp's current server-side acceptance.
    const customCode = "HEAVSTAL" 
    
    const code = await sock.requestPairingCode(phoneNumber, customCode)
    console.log(`Your Pairing Code: ${code}`)
}
```

### 2. Newsletter (Channel) Dominance
Full control over WhatsApp Channels. Create, edit, and manage them programmatically.

<details>
<summary><b>View Newsletter Examples</b></summary>

```ts
// 1. Create a Channel
const channel = await sock.newsletterCreate("Heavstal Updates", "Official Channel")

// 2. Send Message to Channel
await sock.sendMessage(channel.id, { text: "Hello World 🦅" })

// 3. React to Channel Message
await sock.newsletterReactMessage(channel.id, "MSG_SERVER_ID", "🔥")

// 4. Update Description
await sock.newsletterUpdateDescription(channel.id, "New Description by Bot")
```
</details>

### 3. Interactive & Button Messages
Native flow buttons that work on both Android and iOS.

<details>
<summary><b>View Button Examples</b></summary>

```ts
const interactiveMessage = {
    body: { text: "Choose your path" },
    footer: { text: "Heavstal Tech" },
    header: { title: "Welcome", hasMediaAttachment: false },
    nativeFlowMessage: {
        buttons: [
            {
                name: "single_select",
                buttonParamsJson: JSON.stringify({
                    title: "Menu",
                    sections: [{
                        title: "Options",
                        rows: [
                            { header: "STATUS", title: "System Status", id: "ping" },
                            { header: "OWNER", title: "Contact Dev", id: "owner" }
                        ]
                    }]
                })
            }
        ]
    }
}

await sock.sendMessage(jid, { viewOnceMessage: { message: { interactiveMessage } } })
```
</details>

### 4. Album Messages
Send multiple images or videos as a single grouped bubble.

```ts
const mediaAlbum = [
    { image: { url: "https://heavstal.com/img1.jpg" } },
    { image: { url: "https://heavstal.com/img2.jpg" } },
    { video: { url: "https://heavstal.com/video.mp4" } }
]

await sock.sendMessage(jid, { album: mediaAlbum, caption: "Heavstal Gallery" })
```

---

## 🛠️ Architecture & Debugging

**HT-baileys** uses `Pino` for logging. To see the internal heartbeat of the library (useful for debugging connection issues):

```ts
import P from 'pino'

const sock = makeWASocket({
    logger: P({ level: 'debug' }) // 'info', 'debug', 'trace', 'silent'
})
```

### The Anti-Ban Protocol
We utilize specific browser masquerading configurations (`Browsers.macOS`, `Browsers.ubuntu`) and header optimization to make the bot traffic look indistinguishable from a real WhatsApp Web client.

---

## 🤝 Contributing & Support

We welcome contributions from the community!

- **Issue Tracker:** [Report Bugs](https://github.com/Promise818/HT-baileys/issues)
- **Official Website:** [Heavstal Tech](https://heavstal-tech.vercel.app)
- **Developer:** Promise818

## ⚠️ Disclaimer

This project is an independent fork and is **not** affiliated with, authorized, maintained, sponsored, or endorsed by WhatsApp or any of its affiliates or subsidiaries.

**Use responsibly.** Heavstal Tech is not liable for any account bans resulting from the misuse of this library for spamming or violating WhatsApp's Terms of Service.

---

<div align="center">
  <sub>Built with 🖤 by <a href="https://github.com/Promise818">Promise818</a> | © 2025 Heavstal Tech</sub>
</div>
