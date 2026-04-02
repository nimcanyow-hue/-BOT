𝐌𝐀𝐙𝐊𝐈𝐈𝐍𝐎𝐖# -BOT
// npm install @whiskeysockets/baileys  const { default: makeWASocket, useMultiFileAuthState } = require("@whiskeysockets/baileys")  async function startBot() {   const { state, saveCreds } = await useMultiFileAuthState("session")    const s
𝐌𝐀𝐙𝐊𝐈𝐈𝐍𝐎𝐖// npm install @whiskeysockets/baileys

const { default: makeWASocket, useMultiFileAuthState } = require("@whiskeysockets/baileys")

async function startBot() {
  const { state, saveCreds } = await useMultiFileAuthState("session")

  const sock = makeWASocket({
    auth: state,
    printQRInTerminal: true
  })

  sock.ev.on("creds.update", saveCreds)

  sock.ev.on("messages.upsert", async (m) => {
    const msg = m.messages[0]
    if (!msg.message) return

    const text = msg.message.conversation || msg.message.extendedTextMessage?.text
    const from = msg.key.remoteJid

    // 🔹 Auto Reply
    if (text === "hi") {
      await sock.sendMessage(from, { text: "Welcome nio 👋" })
    }

    // 🔹 Menu
    if (text === ".menu") {
      await sock.sendMessage(from, {
        text: "🤖 BOT MENU\n\n.menu\n.sticker\n.owner\n"
      })
    }

    // 🔹 Anti Delete (simple)
    if (msg.message?.protocolMessage?.type === 0) {
      await sock.sendMessage(from, {
        text: "👀 Qof ayaa fariin tirtiray!"
      })
    }
  })
}

startBot()
