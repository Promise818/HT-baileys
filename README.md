<div align='center'>HT-baileys (Heavstal Tech Edition)</div>

<div align="center">

  <img src="https://raw.githubusercontent.com/Bell575/Upload/main/uploads/1742387351904.png" alt="HT-baileys" width="100%" />

  <br/>
  
  <a href="https://heavstal-tech.vercel.app">
    <img src="https://img.shields.io/badge/Heavstal-Tech-blue?style=for-the-badge&logo=vercel" alt="Heavstal Tech" />
  </a>
  <a href="https://github.com/Promise818/HT-baileys">
    <img src="https://img.shields.io/badge/Version-1.0.0-green?style=for-the-badge" alt="Version" />
  </a>

</div>

## 📖 Table of Contents

- [Important Note](#important-note)
- [Install](#install)
- [Added Features and Improvements](#-added-features-and-improvements)
- [Feature Examples](#feature-examples)
  - [Newsletter Management](#newsletter-management)
  - [Button and Interactive Message Management](#button-and-interactive-message-management)
  - [Send Album Message](#send-album-message)
  - [AI Message Icon Customization](#ai-message-icon-customization)
  - [Custom Pairing Code Generation](#custom-pairing-code-generation)
- [Reporting Issues](#reporting-issues)
- [Notes](#notes)
---

## Important Note

**HT-baileys** is a fork of the legendary Baileys library, engineered by **Heavstal Tech** (Promise818). 
Building upon the foundation of `baileys-pro`, we have implemented specific enhancements for the Heavstal Ecosystem, including custom pairing logic, optimized headers, and improved stability for high-traffic bots.

## Install

Install via NPM:
```bash
npm install @heavstaltech/baileys
```

Use the edge version (latest updates):
```bash
npm install @heavstaltech/baileys@latest
```

Then import the default function in your code:

**TypeScript / ES Modules:**
```ts 
import makeWASocket from '@heavstaltech/baileys'
```

**CommonJS:**
```js
const { default: makeWASocket } = require("@heavstaltech/baileys")
```

## Added Features and Improvements

| Feature                               | Description                                                                                                                               |
| :------------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------- |
| 💬 **Send Messages to Channels**     | Supports sending text and media messages to channels/newsletters.                                                                         |
| 🔘 **Button & Interactive Messages** | Supports sending button messages and interactive messages on WhatsApp Messenger and WhatsApp Business.                                   |
| 🖼️ **Send Album Messages**           | Supports sending multiple images as an album (grouped media message).                                                                     |
| 🤖 **AI Message Icon**               | Customize message appearances with an optional AI icon (`ai: true`).                                                                      |
| 🔑 **Custom Pairing Codes**          | **Heavstal Exclusive:** Custom pairing logic to support branded pairing codes like `HEAV-STAL`.                                           |
| 🛠️ **Anti-Ban Architecture**        | Optimized connection headers to reduce ban rates for heavy users.                                                                         |

## Feature Examples

Here are some examples of features available in **HT-baileys**:

### Newsletter Management

<details>
<summary style="font-weight: bold; cursor: pointer; padding: 8px; border-bottom: 1px solid #eee; margin-bottom: 5px;">Show Examples</summary>
<div style="padding: 10px 15px; background: #f9f9f9; border: 1px solid #eee; border-top: none; border-radius: 0 0 5px 5px;">

- **To get info newsletter**
```ts
const metadata = await sock.newsletterMetadata("invite", "xxxxx")
// or
const metadata = await sock.newsletterMetadata("jid", "abcd@newsletter")
console.log(metadata)
```
- **To update the description of a newsletter**
```ts
await sock.newsletterUpdateDescription("abcd@newsletter", "New Description")
```
- **To create a newsletter**
```ts
const metadata = await sock.newsletterCreate("Heavstal Updates")
console.log(metadata)
```
- **To follow a newsletter**
```ts
await sock.newsletterFollow("abcd@newsletter")
```
- **To send reaction**
```ts
// jid, id message & emoticon
const id = "175"
await sock.newsletterReactMessage("abcd@newsletter", id, "🦅")
```
</div>
</details>

### Button and Interactive Message Management

<details>
<summary style="font-weight: bold; cursor: pointer; padding: 8px; border-bottom: 1px solid #eee; margin-bottom: 5px;">Show Examples</summary>
<div style="padding: 10px 15px; background: #f9f9f9; border: 1px solid #eee; border-top: none; border-radius: 0 0 5px 5px;">

- **To send button with text**
```ts
const buttons = [
  { buttonId: 'id1', buttonText: { displayText: 'Heavstal' }, type: 1 },
  { buttonId: 'id2', buttonText: { displayText: 'Tech' }, type: 1 }
]

const buttonMessage = {
    text: "Welcome to Heavstal Tech",
    footer: 'Powered by HT-baileys',
    buttons,
    headerType: 1
}

await sock.sendMessage(id, buttonMessage, { quoted: null })
```

- **To send interactive message (List)**
```ts
const interactiveButtons = [
  {
    name: "single_select",
    buttonParamsJson: JSON.stringify({
      title: "Select Option",
      sections: [
        {
          title: "Main Menu",
          highlight_label: "Hot",
          rows: [
            { header: "FEATURE", title: "Bot Status", description: "Check status", id: "status" },
            { header: "FEATURE", title: "Dashboard", description: "Go to Web", id: "web" }
          ]
        }
      ]
    })
  }
];

const interactiveMessage = {
    text: "Heavstal Bot Menu",
    footer: "Choose an option below",
    interactiveButtons
};

await sock.sendMessage(id, interactiveMessage, { quoted: null });
```

</div>
</details>

### Send Album Message

<details>
<summary style="font-weight: bold; cursor: pointer; padding: 8px; border-bottom: 1px solid #eee; margin-bottom: 5px;">Show Example</summary>
<div style="padding: 10px 15px; background: #f9f9f9; border: 1px solid #eee; border-top: none; border-radius: 0 0 5px 5px;">

```ts
// Media can be a URL, buffer, or path.
const media = [
  { image: { url: "https://example.com/image.jpg" } },
  { video: { url: "https://example.com/video.mp4" } }
]

await sock.sendMessage(id, { album: media, caption: "Heavstal Album Test" }, { quoted: null })
```

</div>
</details>

### AI Message Icon Customization

<details>
<summary style="font-weight: bold; cursor: pointer; padding: 8px; border-bottom: 1px solid #eee; margin-bottom: 5px;">Show Example</summary>
<div style="padding: 10px 15px; background: #f9f9f9; border: 1px solid #eee; border-top: none; border-radius: 0 0 5px 5px;">

```ts
// To enable the AI icon for a message, simply add the "ai: true" parameter:
await sock.sendMessage(id, { text: "Thinking Process...", ai: true });
```

</div>
</details>

### Custom Pairing Code Generation

<details>
<summary style="font-weight: bold; cursor: pointer; padding: 8px; border-bottom: 1px solid #eee; margin-bottom: 5px;">Show Example</summary>
<div style="padding: 10px 15px; background: #f9f9f9; border: 1px solid #eee; border-top: none; border-radius: 0 0 5px 5px;">

```ts
if(usePairingCode && !sock.authState.creds.registered) {
    const phoneNumber = await question('Enter number:\n');
    
    // Define your custom code (alphanumeric)
    // NOTE: This is strictly for supported versions of the library
    const customPairingCode = "HEAVSTAL"; 
    
    const code = await sock.requestPairingCode(phoneNumber, customPairingCode);
    console.log(`Your Pairing Code: ${code}`);
}
```
</div>
</details>

## Reporting Issues
If you encounter any issues while using **HT-baileys**, please open a [new issue](https://github.com/Promise818/HT-baileys/issues).

## Notes
**Heavstal Tech** acknowledges the work of the original Baileys team and the WhiskeySockets community. This fork is maintained to ensure compatibility with our internal systems.
