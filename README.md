<div align='center'>HT-baileys</div>

<div align="center">
  <img src="https://files.catbox.moe/vi3npg.png" alt="HT-baileys Banner" width="100%" />
  <br/><br/>

  <a href="https://heavstal-tech.vercel.app">
    <img src="https://img.shields.io/badge/Maintainer-Heavstal_Tech-007acc?style=for-the-badge&logo=vercel&logoColor=white" alt="Heavstal Tech" />
  </a>
  <a href="https://www.npmjs.com/package/@heavstaltech/baileys">
    <img src="https://img.shields.io/badge/NPM-@heavstaltech/baileys-cb3837?style=for-the-badge&logo=npm&logoColor=white" alt="NPM" />
  </a>
  <a href="https://github.com/Promise818/HT-baileys">
    <img src="https://img.shields.io/badge/Version-1.0.0-2ea44f?style=for-the-badge&logo=github&logoColor=white" alt="Version" />
  </a>
  <a href="https://github.com/Promise818/HT-baileys/blob/master/LICENSE">
    <img src="https://img.shields.io/badge/License-MIT-fab005?style=for-the-badge" alt="License" />
  </a>
</div>

---

## Introduction

**HT-baileys** is an extended fork of the WhatsApp Web API library, maintained by **Heavstal Tech**.

This library builds upon the stability of the original Baileys and the extended features of `baileys-pro`, offering a robust solution for enterprise-grade WhatsApp automation. It includes optimized headers for connection stability, support for custom pairing codes, and native implementation of WhatsApp Channels (Newsletters) and interactive messages.

## Installation

### Stable Release
```bash
npm install @heavstaltech/baileys
```

### Edge Release
```bash
npm install @heavstaltech/baileys@latest
```

---

## Usage

### Basic Connection

The following example demonstrates how to initialize a socket connection using `HT-baileys`.

```ts
import makeWASocket, { 
    useMultiFileAuthState, 
    DisconnectReason, 
    Browsers 
} from '@heavstaltech/baileys'

async function connectToWhatsApp() {
    const { state, saveCreds } = await useMultiFileAuthState('auth_info')

    const sock = makeWASocket({
        auth: state,
        printQRInTerminal: false, // Set to true if using QR scanning
        browser: Browsers.macOS("Desktop"),
        syncFullHistory: true
    })

    sock.ev.on('connection.update', (update) => {
        const { connection, lastDisconnect } = update
        
        if(connection === 'close') {
            const shouldReconnect = (lastDisconnect?.error as any)?.output?.statusCode !== DisconnectReason.loggedOut
            console.log('Connection closed. Reconnecting:', shouldReconnect)
            
            if(shouldReconnect) {
                connectToWhatsApp()
            }
        } else if(connection === 'open') {
            console.log('Connection opened successfully')
        }
    })

    sock.ev.on('creds.update', saveCreds)
}

connectToWhatsApp()
```

---

## Features & Implementation

### 1. Custom Pairing Code
HT-baileys supports the definition of custom alphanumeric pairing codes, allowing for branded connection flows.

```ts
if (!sock.authState.creds.registered) {
    // Ensure the number format is correct (Country Code + Number)
    const phoneNumber = "2349000000000"
    const customCode = "HEAVSTAL" 
    
    setTimeout(async () => {
        const code = await sock.requestPairingCode(phoneNumber, customCode)
        console.log(`Pairing Code: ${code}`)
    }, 3000)
}
```

### 2. Newsletter Management
Provides a full suite of tools for managing WhatsApp Channels (Newsletters).

**Create a Channel**
```ts
const channel = await sock.newsletterCreate("Heavstal Updates", "Official Channel Description")
console.log("Channel Created:", channel.id)
```

**Update Channel Metadata**
```ts
await sock.newsletterUpdateName(channel.id, "Heavstal Tech")
await sock.newsletterUpdateDescription(channel.id, "Official Updates")
```

**Mute/Unmute**
```ts
await sock.newsletterMute(channel.id)
await sock.newsletterUnmute(channel.id)
```

### 3. Interactive Messages (Buttons & Lists)
Native support for interactive message types, including single-select lists and call-to-action buttons.

```ts
const interactiveMessage = {
    body: { text: "Select an option below" },
    footer: { text: "Heavstal Tech" },
    header: { title: "Main Menu", hasMediaAttachment: false },
    nativeFlowMessage: {
        buttons: [
            {
                name: "single_select",
                buttonParamsJson: JSON.stringify({
                    title: "Tap to open",
                    sections: [{
                        title: "Services",
                        rows: [
                            { header: "STATUS", title: "System Status", id: "status_check" },
                            { header: "SUPPORT", title: "Contact Support", id: "contact_support" }
                        ]
                    }]
                })
            }
        ]
    }
}

await sock.sendMessage(jid, { 
    viewOnceMessage: { 
        message: { interactiveMessage } 
    } 
})
```

### 4. Album Messages
Allows sending multiple media assets (Images/Videos) grouped as a single album.

```ts
const mediaContent = [
    { image: { url: "https://example.com/image1.jpg" } },
    { image: { url: "https://example.com/image2.jpg" } },
    { video: { url: "https://example.com/video.mp4" } }
]

await sock.sendMessage(jid, { 
    album: mediaContent, 
    caption: "Media Album" 
})
```

### 5. AI Message Flag
Support for the `ai` property to mark messages generated by automated systems.

```ts
await sock.sendMessage(jid, { 
    text: "Processed by AI", 
    ai: true 
})
```

---

## Disclaimer

**HT-baileys** is an independent project maintained by **Heavstal Tech**. It is not affiliated with, authorized, maintained, sponsored, or endorsed by WhatsApp Inc. or Meta Platforms, Inc.

This software is provided "as is", without warranty of any kind. Users are responsible for ensuring their usage complies with WhatsApp's Terms of Service. **Heavstal Tech** accepts no liability for account restrictions or bans resulting from the use of this library.

---

<div align="center">
  <sub>© 2025 Heavstal Tech. All rights reserved.</sub>
</div>
