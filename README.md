# <div align='center'>Baileys - WhatsApp API</div>

<div align='center'>

![WhatsApp API](https://i.supa.codes/kyWCSZ)

</div>

<p align="center">
  <a href="https://nodejs.org"><img src="https://img.shields.io/badge/Node.js->=20.0.0-green?logo=node.js" alt="Node.js"></a>
  <a href="https://www.npmjs.com/package/naruyaizumi"><img src="https://img.shields.io/npm/v/naruyaizumi?color=blue&label=naruyaizumi&logo=npm" alt="npm version"></a>
</p>

Baileys adalah library TypeScript berbasis WebSocket untuk berinteraksi dengan WhatsApp Web API.

# Penggunaan
Panduan terbaru telah dipublikasikan di https://baileys.wiki.

# Penyangkalan
Proyek ini **tidak berafiliasi, tidak berasosiasi, tidak diotorisasi, tidak didukung, dan tidak memiliki koneksi resmi dengan WhatsApp** maupun anak perusahaan atau afiliasinya.  
Situs resmi WhatsApp dapat ditemukan di whatsapp.com.  
"WhatsApp" serta nama-nama, merek dagang, lambang, dan gambar terkait adalah hak milik terdaftar dari pemilik masing-masing.

Para pengelola Baileys **tidak mendukung penggunaan aplikasi ini untuk praktik yang melanggar Ketentuan Layanan WhatsApp**.  
Kami menyerukan kepada setiap pengguna untuk menggunakan aplikasi ini secara bertanggung jawab dan sesuai tujuan awal pengembangannya.  
Gunakan atas kebijakan pribadi masing-masing. Jangan gunakan untuk spam. Kami tidak mendukung penggunaan untuk stalkerware, pesan massal, ataupun automasi pesan yang bersifat mengganggu.

### Lisensi

Proyek ini menggunakan lisensi [MIT License](https://github.com/WhiskeySockets/Baileys?tab=readme-ov-file#license), dan merupakan karya turunan dari Baileys oleh Rajeh Taher/WhiskeySockets.

Dengan menggunakan proyek ini, Anda dianggap telah menyetujui ketentuan lisensi tersebut.

## Tentang Modifikasi

Proyek ini merupakan hasil **modifikasi besar dari Baileys**, sebuah library open-source WhatsApp Web API yang awalnya ditulis dalam TypeScript dan menggunakan format ECMAScript Module (ESM).

Modifikasi ini difokuskan untuk membuat versi yang **sepenuhnya berbasis JavaScript murni dengan dukungan CommonJS (CJS)**. Dengan pendekatan ini, library menjadi **lebih fleksibel dan mudah diintegrasikan** ke dalam berbagai jenis runtime Node.js tanpa memerlukan proses transpilasi atau konfigurasi tambahan seperti `"type": "module"`.

### Poin Utama Modifikasi:

- **Konversi total dari TypeScript ke JavaScript**, untuk menyederhanakan proses pengembangan, debugging, dan distribusi.
- **Penggunaan format module CommonJS (CJS)** secara konsisten agar dapat digunakan di lingkungan Node.js manapun, termasuk proyek lama.
- **Kompatibel dengan modul-modul ESM modern**, melalui penggunaan dynamic import (`await import()`), tanpa mengorbankan arsitektur utama CJS.
- **Dukungan penuh terhadap tombol interaktif**
- Penyederhanaan berbagai struktur internal seperti manajemen sesi, koneksi, katalog produk, dan optimasi format media.
- File `proto` (WAProto) telah di-*compile* secara statis menjadi JavaScript untuk menghindari dependensi waktu jalan terhadap parser `.proto`.

## Instalasi

Gunakan salah satu manajer paket berikut untuk menginstal versi stabil:

```bash
npm install naruyaizumi
```
```bash
yarn add naruyaizumi
```
```bash
pnpm add naruyaizumi
```
## Informasi

Paket ini `membutuhkan` Node.js versi **20 atau lebih tinggi** untuk berjalan.

Proyek ini secara eksplisit ditujukan untuk lingkungan modern dan tidak mendukung Node versi lama. Dukungan akan selalu mengikuti versi LTS terbaru dari Node.js untuk menjaga performa dan kompatibilitas dengan ekosistem terbaru.

![metadata](https://i.supa.codes/kKcA8Q)

> **Copyright © 2024 - 2025 Naruya Izumi**

[꒰⚘꒱ Admin Contact ꒱⟡](https://linkbio.co/naruyaizumi)


## Menghubungkan Akun

Baileys mendukung koneksi ke WhatsApp melalui API multi-perangkat.  
Kamu bisa menghubungkan akun dengan dua metode utama: **Kode QR** atau **Kode Pairing**.

### Menghubungkan dengan **Kode QR** (ESM)

> [!TIP]  
> Kamu bisa mengatur nama browser yang tampil di perangkat WhatsApp dengan menggunakan konstanta `Browsers`.  
> Lihat daftar nama browser yang tersedia di dokumentasi:  
> [https://baileys.whiskeysockets.io/types/BrowsersMap.html](https://baileys.whiskeysockets.io/types/BrowsersMap.html)

```javascript
import makeWASocket, { Browsers } from 'naruyaizumi'

const sock = makeWASocket({
  browser: Browsers.ubuntu('Safari'),
  printQRInTerminal: true
})
```

Jika koneksi berhasil, kode QR akan muncul di terminal.  
Pindai kode tersebut menggunakan aplikasi WhatsApp di ponsel kamu untuk login.

### Menghubungkan dengan **Kode QR** (CommonJS)

Jika kamu menggunakan Node.js dengan format CommonJS (`require`), gunakan contoh berikut:

```javascript
const { default: makeWASocket, Browsers } = require('naruyaizumi')

const sock = makeWASocket({
  browser: Browsers.ubuntu('Safari'),
  printQRInTerminal: true
})
```

> Pastikan file kamu **tidak menggunakan** `"type": "module"` di `package.json` agar mode CJS dapat berjalan dengan benar.

### Memulai Socket dengan **Kode Pairing**

> [!IMPORTANT]  
> Pairing Code *bukan* API Mobile. Ini adalah metode untuk terhubung ke WhatsApp Web **tanpa memindai kode QR**.  
> Metode ini hanya memungkinkan koneksi dari **satu perangkat saja**.  
> Lihat penjelasan resmi [di sini](https://faq.whatsapp.com/1324084875126592/?cms_platform=web)

Nomor telepon **tidak boleh menggunakan karakter** seperti `+`, `()` atau `-`.  
Gunakan hanya angka dan pastikan menyertakan kode negara.

```javascript
import makeWASocket from 'naruyaizumi'

const sock = makeWASocket({
  // Konfigurasi tambahan dapat disesuaikan di sini
  printQRInTerminal: false // Harus disetel ke false untuk pairing
})

if (!sock.authState.creds.registered) {
  const number = '628XXXXXXXXX'
  const code = await sock.requestPairingCode(number)
  // atau gunakan pairing code kustom:
  // const code = await sock.requestPairingCode(number, 'CODEZUMI')
  console.log(code)
}
```

Setelah pairing code berhasil dibuat, masukkan kode tersebut melalui WhatsApp Web seperti biasa untuk menyelesaikan proses autentikasi.

### Menerima Riwayat Pesan Lengkap

1. Atur opsi `syncFullHistory` ke `true`
2. Secara default, Baileys menggunakan konfigurasi browser Chrome.  
   Jika kamu ingin **meniru koneksi desktop** (dan menerima riwayat pesan yang lebih banyak), gunakan konfigurasi browser seperti contoh di bawah ini.

```javascript
import makeWASocket, { Browsers } from 'naruyaizumi'

const sock = makeWASocket({
  ...otherOpts,
  // Kamu dapat menggunakan Windows, Ubuntu, dll.
  browser: Browsers.ubuntu('Safari'),
  syncFullHistory: true
})
```

> [!NOTE]  
> WhatsApp hanya mengirim riwayat pesan penuh jika koneksi yang digunakan menyerupai **perangkat desktop resmi** (WhatsApp Web).  
> Konfigurasi browser yang tepat sangat berpengaruh pada jumlah riwayat yang dikirim.

## Catatan Penting Mengenai Konfigurasi Socket

### Caching Metadata Grup (Direkomendasikan)

- Jika kamu menggunakan Baileys untuk mengelola grup, sangat disarankan untuk mengatur opsi `cachedGroupMetadata` pada konfigurasi socket kamu.  
- Kamu perlu mengimplementasikan sistem cache sederhana seperti contoh berikut:

```javascript
import makeWASocket from 'naruyaizumi'
import NodeCache from 'node-cache'

const groupCache = new NodeCache({ stdTTL: 5 * 60, useClones: false })

const sock = makeWASocket({
  cachedGroupMetadata: async (jid) => groupCache.get(jid)
})

sock.ev.on('groups.update', async ([event]) => {
  const metadata = await sock.groupMetadata(event.id)
  groupCache.set(event.id, metadata)
})

sock.ev.on('group-participants.update', async (event) => {
  const metadata = await sock.groupMetadata(event.id)
  groupCache.set(event.id, metadata)
})
```

Dengan menggunakan cache ini, Kamu dapat mengurangi jumlah permintaan metadata yang berulang dan mempercepat proses interaksi bot dengan grup secara signifikan.

> [!NOTE]  
> Kamu bisa mengganti `NodeCache` dengan database lain seperti Redis atau in-memory store milik framework kamu sendiri.

## Konfigurasi Lengkap Socket

Kamu dapat mengatur koneksi Baileys dengan memberikan objek `SocketConfig`.  
Berikut adalah daftar lengkap properti yang bisa Kamu sesuaikan beserta penjelasannya:

### Struktur `SocketConfig` (dengan penjelasan)

- `waWebSocketUrl`  
  URL WebSocket yang digunakan untuk terhubung ke WhatsApp (default: `wss://...`)

- `connectTimeoutMs`  
  Waktu maksimal (dalam milidetik) untuk mencoba koneksi sebelum gagal.

- `defaultQueryTimeoutMs`  
  Timeout default untuk permintaan/query. Jika `undefined`, maka tidak ada timeout.

- `keepAliveIntervalMs`  
  Interval ping-pong antar koneksi WebSocket untuk menjaga koneksi tetap hidup.

- `agent` *(opsional)*  
  Proxy agent jika kamu menggunakan koneksi melalui proxy.

- `logger`  
  Logger yang digunakan, biasanya dari `pino`.

- `version`  
  Versi WhatsApp Web yang digunakan untuk koneksi (gunakan `fetchLatestBaileysVersion()` untuk mendapatkan versi terbaru).

- `browser`  
  Konfigurasi browser yang ditiru oleh Baileys (lihat `Browsers.ubuntu()`, `Browsers.macOS()`, dll).

- `fetchAgent` *(opsional)*  
  Agent untuk permintaan upload/download media.

- `printQRInTerminal`  
  Jika `true`, maka kode QR akan dicetak di terminal.

- `emitOwnEvents`  
  Jika `true`, event dari aksi sendiri (seperti mengirim pesan) juga akan dipancarkan oleh event handler.

- `mediaCache` *(opsional)*  
  Cache media agar tidak perlu upload ulang media yang sama.

- `customUploadHosts`  
  Daftar host untuk upload media secara manual.

- `retryRequestDelayMs`  
  Waktu jeda antara retry jika permintaan gagal.

- `qrTimeout` *(opsional)*  
  Waktu tunggu maksimal untuk menampilkan kode QR baru.

- `auth`  
  Objek autentikasi yang digunakan untuk menyimpan dan memuat sesi login WhatsApp.

- `shouldSyncHistoryMessage`  
  Fungsi kontrol untuk memilih jenis riwayat pesan yang ingin disinkronisasi dari perangkat utama.

- `transactionOpts`  
  Opsi untuk manajemen transaksi penyimpanan Signal key.

- `userDevicesCache` *(opsional)*  
  Cache daftar perangkat pengguna.

- `markOnlineOnConnect`  
  Jika `true`, akun akan terlihat online setiap kali koneksi berhasil.  
  Jika `false`, maka WhatsApp di ponsel tetap bisa menerima notifikasi.

- `msgRetryCounterMap` *(opsional)*  
  Penyimpanan jumlah percobaan retry pengiriman pesan.

- `linkPreviewImageThumbnailWidth`  
  Lebar thumbnail gambar pada link preview.

- `syncFullHistory`  
  Jika `true`, Baileys akan meminta riwayat pesan penuh (sangat disarankan untuk akun baru).

- `fireInitQueries`  
  Jika `true`, Baileys akan menjalankan kueri inisialisasi otomatis saat koneksi dimulai.

- `generateHighQualityLinkPreview`  
  Jika `true`, Baileys akan mengunggah thumbnail berkualitas tinggi untuk preview link.

- `options`  
  Opsi tambahan untuk permintaan HTTP/axios (misalnya: timeout, headers, dll).

- `getMessage`  
  Fungsi untuk mengambil ulang isi pesan dari penyimpanan lokal kamu.  
  Digunakan saat pengiriman ulang pesan gagal atau saat dekripsi polling suara.

> [!INFO]  
> Kamu tidak perlu mengatur semua properti di atas.  
> Cukup sesuaikan yang kamu butuhkan, sisanya akan menggunakan nilai default dari Baileys.

## Menangani Event

- Baileys menggunakan pola `EventEmitter` untuk menangani berbagai peristiwa (*event*).  
Seluruh event telah diketik (typed) dengan baik, sehingga editor seperti **VS Code** akan memberikan dukungan Intellisense secara optimal.

> [!IMPORTANT]  
> **Daftar lengkap event tersedia [di sini](https://baileys.whiskeysockets.io/types/BaileysEventMap.html)**.  
> Sangat penting untuk memahami setiap event yang bisa digunakan.

Contoh penggunaan listener untuk menangani pesan masuk:

```javascript
import makeWASocket from 'naruyaizumi'

const sock = makeWASocket()

sock.ev.on('messages.upsert', ({ messages }) => {
  console.log('Pesan diterima:', messages)
})
```

## Menangani Event

Baileys menggunakan sistem **EventEmitter** untuk menangani interaksi WhatsApp secara real-time.  
Semua event yang terjadi saat koneksi aktif akan dipancarkan melalui `sock.ev.on(...)`, dan kamu bisa menangkap serta meresponsnya sesuai kebutuhan bot kamu.

> [!IMPORTANT]  
> **Daftar lengkap event tersedia [di sini](https://baileys.whiskeysockets.io/types/BaileysEventMap.html)**.  
> Disarankan untuk memahami struktur tiap event agar integrasi kamu lebih stabil dan efisien.

### Contoh: Menangani Pesan Masuk

```javascript
sock.ev.on('messages.upsert', async ({ messages, type }) => {
  const msg = messages[0]
  if (!msg.message) return
  console.log('Pesan diterima:', msg.message)
})
```

- `type` bisa bernilai `notify`, `append`, `replace`, atau `remove`.
- Kamu biasanya hanya perlu memproses `type === 'notify'` untuk pesan baru yang masuk.

### Contoh: Menangani Pembaruan Koneksi

```javascript
sock.ev.on('connection.update', ({ connection, lastDisconnect }) => {
  if (connection === 'close') {
    console.log('Koneksi terputus.')
  } else if (connection === 'open') {
    console.log('Terhubung ke WhatsApp!')
  }
})
```

- Event ini sangat penting untuk memantau status koneksi socket.
- Jika `connection === 'close'`, Kamu dapat mencoba reconnect otomatis.

### Contoh: Deteksi Peserta Masuk/Keluar Grup

```javascript
sock.ev.on('group-participants.update', async ({ id, participants, action }) => {
  if (action === 'add') {
    console.log('Anggota baru masuk:', participants)
  } else if (action === 'remove') {
    console.log('Anggota keluar:', participants)
  }
})
```

- `id`: JID grup
- `participants`: array nomor yang terlibat
- `action`: `'add' | 'remove' | 'promote' | 'demote'`

### Contoh: Pembaruan Metadata Grup

```javascript
sock.ev.on('groups.update', async (updates) => {
  for (let group of updates) {
    console.log('Grup diperbarui:', group)
  }
})
```

- Bisa digunakan untuk mendeteksi perubahan nama grup, gambar, deskripsi, dll.

### Tips

- Event `messages.update` digunakan untuk mendeteksi status pesan seperti dibaca, diterima, atau gagal.
- Event `messages.reaction` digunakan untuk menangkap reaksi (emoji) pada pesan kamu.

> [!TIP]  
> Baileys tidak menyimpan cache pesan secara default.  
> Untuk menangani event dengan akurat (seperti retry atau polling), gunakan `getMessage()` bersama `store`.

Jika kamu butuh event tambahan seperti **reaction**, **presence**, atau **call offer**, tinggal tambahkan listener-nya sesuai struktur [BaileysEventMap](https://baileys.whiskeysockets.io/types/BaileysEventMap.html).

## Menyimpan & Memulihkan Sesi

Tentu kamu tidak ingin terus-menerus memindai QR code setiap kali ingin terkoneksi.

Kamu bisa menyimpan kredensial dan menggunakannya kembali saat login berikutnya:

```javascript
import makeWASocket, { useMultiFileAuthState } from 'naruyaizumi'

const { state, saveCreds } = await useMultiFileAuthState('auth_info_baileys')

// Akan menggunakan kredensial yang tersedia untuk koneksi ulang
// Jika kredensial valid ditemukan, akan langsung login tanpa QR
const sock = makeWASocket({ auth: state })

// Fungsi ini akan dipanggil setiap kali kredensial diperbarui
sock.ev.on('creds.update', saveCreds)
```

> [!IMPORTANT]  
> `useMultiFileAuthState` adalah fungsi utilitas untuk menyimpan state autentikasi dalam satu folder.  
> Fungsi ini juga dapat dijadikan dasar untuk menulis sistem autentikasi dan penyimpanan kunci pada database SQL atau NoSQL — sangat direkomendasikan untuk sistem berskala produksi.

### Apa Isi Folder `auth_info_baileys`?

Folder tersebut akan berisi beberapa file `.json` seperti:

- `creds.json` — informasi kredensial utama
- `keys/` — berisi subfile kunci Signal: pre-keys, session, senderKey, dll.

> Folder ini **jangan pernah dimodifikasi atau dihapus secara manual**.  
> Perlakukan seperti file token yang sangat sensitif.

### Apa yang Terjadi Jika Folder Hilang?

Jika folder `auth_info_baileys` dihapus:
- Kamu **tidak bisa login ulang** tanpa memindai QR lagi
- Semua sesi yang aktif akan invalid
- Signal akan membuat ulang semua sesi enkripsi end-to-end

Backup sangat disarankan jika kamu mengelola sesi penting.

### Tips Backup & Restore

- Salin seluruh folder `auth_info_baileys` secara utuh.
- Untuk restore, cukup salin folder kembali ke path yang sama sebelum memulai bot.
- Gunakan `.gitignore` agar folder ini tidak ikut di-push ke GitHub:
  ```
  auth_info_baileys/
  ```

### Menyimpan Berdasarkan ID Pengguna (Multi-Akun)

Jika kamu mengelola banyak sesi pengguna (multi-client), buat direktori penyimpanan berdasarkan ID pengguna:

```javascript
const { state, saveCreds } = await useMultiFileAuthState(`./sessions/${userId}`)
```

Dengan cara ini, kamu bisa memisahkan sesi tiap user tanpa saling bentrok.

> Rekomendasi: kombinasikan dengan database seperti MongoDB/Redis untuk mencatat mapping antara userId dan path session-nya.

### Contoh untuk Memulai

> [!NOTE]  
> Contoh ini juga sudah mencakup penyimpanan kredensial secara otomatis

```javascript
import makeWASocket, { DisconnectReason, useMultiFileAuthState } from 'naruyaizumi'
import { Boom } from '@hapi/boom'

async function connectToWhatsApp () {
  const { state, saveCreds } = await useMultiFileAuthState('./auth_info_baileys')

  const sock = makeWASocket({
    auth: state,
    printQRInTerminal: true
  })

  sock.ev.on('connection.update', (update) => {
    const { connection, lastDisconnect } = update

    if (connection === 'close') {
      const shouldReconnect = (lastDisconnect.error as Boom)?.output?.statusCode !== DisconnectReason.loggedOut
      console.log('Koneksi terputus karena', lastDisconnect.error, ', mencoba sambung ulang:', shouldReconnect)

      if (shouldReconnect) {
        connectToWhatsApp()
      }
    } else if (connection === 'open') {
      console.log('Koneksi berhasil dibuka')
    }
  })

  sock.ev.on('messages.upsert', async (event) => {
    for (const m of event.messages) {
      console.log(JSON.stringify(m, undefined, 2))

      console.log('Membalas ke', m.key.remoteJid)
      await sock.sendMessage(m.key.remoteJid!, { text: 'Hello World' })
    }
  })

  // Menyimpan kredensial setiap kali diperbarui
  sock.ev.on('creds.update', saveCreds)
}

connectToWhatsApp()
```

### Contoh Penggunaan `useSingleFileAuthState` dan `useMongoFileAuthState`

```javascript
import makeWASocket, {
  useSingleFileAuthState,
  useMongoFileAuthState
} from 'naruyaizumi'

// Autentikasi menggunakan file tunggal (Single File Auth)
const { state, saveState } = await useSingleFileAuthState('./auth_info_baileys.json')
const sock = makeWASocket({
  auth: state,
  printQRInTerminal: true
})

sock.ev.on('creds.update', saveState)
```

```javascript
// Autentikasi menggunakan MongoDB
import { MongoClient } from 'mongodb'

const connectAuth = async () => {
  const client = new MongoClient('mongodb://localhost:27017')
  await client.connect()

  console.log('Berhasil terhubung ke MongoDB')

  const collection = client.db('naruyaizumi').collection('sessions')
  const { state, saveCreds } = await useMongoFileAuthState(collection)

  const sock = makeWASocket({
    auth: state,
    printQRInTerminal: true
  })

  sock.ev.on('creds.update', saveCreds)
}

connectAuth()
```

> [!IMPORTANT]  
> Dalam event `messages.upsert`, sangat disarankan menggunakan perulangan `for (const message of event.messages)` untuk menangani semua pesan dalam array secara individual.  
> Hal ini mencegah pesan terlewat dan memudahkan logging/debugging.

> [!TIP]  
> Kamu bisa menggabungkan pendekatan penyimpanan sesi (`MultiFile`, `SingleFile`, atau `MongoDB`) dengan sistem login berbasis ID pengguna, sehingga mendukung banyak akun secara paralel.

### Mendekripsi Suara Polling

Secara default, suara polling di WhatsApp dienkripsi dan diproses melalui event `messages.update`.

```javascript
import pino from 'pino'
import {
  makeInMemoryStore,
  getAggregateVotesInPollMessage
} from 'naruyaizumi'

const logger = pino({ timestamp: () => `,"time":"${new Date().toJSON()}"` }).child({ class: 'NaruyaIzumi' })
logger.level = 'fatal'
const store = makeInMemoryStore({ logger })

async function getMessage(key) {
  if (store) {
    const msg = await store.loadMessage(key.remoteJid, key.id)
    return msg?.message
  }
  return {
    conversation: 'Polling Tidak Ditemukan'
  }
}

sock.ev.on('messages.update', async (chatUpdate) => {
  for (const { key, update } of chatUpdate) {
    if (update.pollUpdates && key.fromMe) {
      const pollCreation = await getMessage(key)
      if (pollCreation) {
        const pollUpdate = await getAggregateVotesInPollMessage({
          message: pollCreation,
          pollUpdates: update.pollUpdates
        })

        const toCmd = pollUpdate.filter(v => v.voters.length !== 0)[0]?.name
        if (!toCmd) return

        console.log('Pilihan terpilih:', toCmd)
        // Tambahkan aksi lanjutan di sini
      }
    }
  }
})
```

### Penjelasan

- **`store.loadMessage(jid, id)`** digunakan untuk mengambil ulang isi pesan polling (karena hasil polling hanya berisi update, bukan isi awal).
- **`getAggregateVotesInPollMessage()`** menggabungkan seluruh `pollUpdates` dan menghasilkan daftar suara lengkap.
- Sangat penting menggunakan `getMessage()` yang valid. Jika kamu tidak menyimpan store, dekripsi suara tidak akan berhasil.

> [!TIP]  
> Untuk polling publik, kamu tidak perlu key khusus.  
> Tapi untuk polling private (atau jika polling berasal dari orang lain), pastikan kamu menyimpan pesan awalnya menggunakan store atau log pesan masuk.

### Ringkasan Event Saat Koneksi Pertama

1. Saat socket pertama kali terkoneksi, event `connection.update` akan dipicu.  
   Biasanya status koneksi akan masuk ke `'open'` atau `'close'`.
2. Setelah itu, WhatsApp akan mengirimkan riwayat pesan (history chat) melalui event:  
   **`messaging-history.set`**
3. Riwayat tersebut berisi pesan-pesan dari sesi sebelumnya, termasuk polling yang belum terjawab.

> [!IMPORTANT]  
> Untuk menangani polling, kamu **wajib mengatur `getMessage()`** di konfigurasi `makeWASocket()`.  
> Ini memastikan Baileys bisa mendekripsi hasil polling dengan benar.

```javascript
const sock = makeWASocket({
  auth: state,
  getMessage: async (key) => await getMessage(key)
})
```

## Mengimplementasikan Data Store

Baileys tidak menyediakan sistem penyimpanan (*storage*) bawaan untuk chat, kontak, atau pesan.  
Namun, tersedia implementasi sederhana menggunakan **in-memory store**. Store ini akan memantau pembaruan chat, pesan baru, dan perubahan lainnya agar data kamu tetap mutakhir.

> [!IMPORTANT]  
> Sangat disarankan untuk membangun sistem penyimpanan sendiri.  
> Menyimpan seluruh riwayat chat di RAM akan memakan memori yang besar dan tidak efisien untuk jangka panjang.

---

### Contoh Penggunaan Store

```javascript
import makeWASocket, { makeInMemoryStore } from 'naruyaizumi'

// Store akan menyimpan data koneksi WhatsApp dalam memori
const store = makeInMemoryStore({})

// Membaca data dari file (jika tersedia)
store.readFromFile('./baileys_store.json')

// Menyimpan state ke file setiap 10 detik
setInterval(() => {
  store.writeToFile('./baileys_store.json')
}, 10_000)

const sock = makeWASocket({})

// Store akan mulai mendengarkan event dari socket ini
// Jika socket diganti, store masih bisa digunakan ulang
store.bind(sock.ev)

sock.ev.on('chats.upsert', () => {
  // Akses semua chat tersimpan
  console.log('Data chat diterima:', store.chats.all())
})

sock.ev.on('contacts.upsert', () => {
  // Akses semua kontak tersimpan
  console.log('Kontak diperbarui:', Object.values(store.contacts))
})
```

### Fitur Store

- Menyimpan chat, pesan, dan kontak sementara di memori.
- Mendukung pembacaan dan penulisan dari/ke file JSON.
- Bisa digunakan bersama beberapa koneksi (socket) sekaligus.
- Tersedia fungsi `loadMessages`, `loadMessage`, dan `loadMessageFromContent`.

### Kelebihan

- Cepat dan ringan untuk penggunaan kecil-menengah.
- Ideal untuk penggunaan lokal, testing, atau bot personal.

### Kekurangan

- Data hilang saat proses dihentikan jika tidak ditulis ke file.
- Tidak cocok untuk data skala besar (ribuan pesan atau kontak).
- Tidak mendukung query kompleks (karena berbasis object literal di RAM).

### Rekomendasi Produksi

Untuk sistem besar atau multi-user:

- Gunakan database seperti:
  - MongoDB (untuk struktur fleksibel dan load besar)
  - Redis (untuk cache cepat)
  - PostgreSQL (untuk struktur relasional)
- Sinkronkan event seperti `messages.upsert`, `chats.upsert`, dan `contacts.upsert` ke penyimpanan permanen.
- Gunakan store hanya sebagai cache atau layer middleware sementara.

> [!TIP]  
> Store ini sangat berguna untuk keperluan seperti:
> - Menyimpan polling
> - Retry pesan
> - Melacak status kontak dan grup
> - Menyediakan command `.listchat`, `.listgroup`, dll. dengan data real-time

Jika kamu menggunakan custom `getMessage()`, store ini juga dapat dijadikan referensi lokal untuk mendekripsi polling dan mengirim ulang pesan.

## Penjelasan Tentang WhatsApp ID

- `id` atau biasa disebut juga `jid` adalah **identitas WhatsApp** dari seseorang atau grup yang menjadi tujuan pengiriman pesan.  
- Format ID harus sesuai dengan jenis akun tujuan:

### Jenis Format ID

- **Pengguna Pribadi (User):**  
  Format:  
  ```
  [kode negara][nomor telepon]@s.whatsapp.net
  ```  
  Contoh:  
  ```
  628XXXXXXXXX@s.whatsapp.net
  ```

- **Grup WhatsApp:**  
  Format:  
  ```
  [timestamp grup dibuat]-[random id]@g.us
  ```  
  Contoh:  
  ```
  1234567890-987654321@g.us
  ```

- **Broadcast (Daftar Siaran):**  
  Format:  
  ```
  [timestamp]@broadcast
  ```  
  Contoh:
  ```
  1685539347@broadcast
  ```

- **Status (Story):**  
  Format:  
  ```
  status@broadcast
  ```

> [!TIP]  
> Kamu bisa mendapatkan `jid` dari pesan masuk lewat `m.key.remoteJid` atau dari respon event seperti `groupParticipantsUpdate`, `messages.upsert`, dll.

> [!CAUTION]  
> Jangan mengubah format ID secara manual tanpa validasi. Salah format bisa menyebabkan pesan tidak terkirim atau error `bad jid`.

## Fungsi Utilitas (Utility Functions)

Baileys menyediakan beberapa fungsi utilitas penting yang sangat membantu saat mengembangkan bot:

- **`getContentType(message)`**  
  Mengembalikan jenis konten dari pesan (misalnya: `imageMessage`, `conversation`, `buttonsMessage`, dll).

- **`getDevice(jid)`**  
  Mengembalikan jenis perangkat yang digunakan pengirim (jika tersedia), contoh: Android, iPhone, Web.

- **`makeCacheableSignalKeyStore(authState)`**  
  Membungkus SignalKeyStore menjadi versi yang lebih efisien dan bisa di-cache, untuk performa autentikasi yang lebih cepat.

- **`downloadContentFromMessage(message, type)`**  
  Mengunduh media dari pesan (seperti gambar, video, dokumen).  
  `type` bisa berupa `'image'`, `'video'`, `'audio'`, `'document'`, dll.

  Contoh penggunaan:
  ```javascript
  const stream = await downloadContentFromMessage(msg.imageMessage, 'image')
  const buffer = Buffer.concat([])
  for await (const chunk of stream) buffer.push(chunk)
  ```

> [!NOTE]  
> Sebagian besar fungsi utilitas tidak dipanggil otomatis — Kamu harus menggunakannya sesuai kebutuhan, terutama saat menangani pesan media, format jid, atau decrypt konten.

## Mengirim Pesan

- Semua jenis pesan dapat dikirim menggunakan **satu fungsi saja**, yaitu `sendMessage()`.  
- Lihat daftar jenis pesan yang didukung [di sini](https://baileys.whiskeysockets.io/types/AnyMessageContent.html)  
- Dan semua opsi pengiriman pesan [di sini](https://baileys.whiskeysockets.io/types/MiscMessageGenerationOptions.html)

Contoh:

```javascript
const jid = '628XXXXXXXXX@s.whatsapp.net' // tujuan
const content = { text: 'Halo, ini pesan dari bot!' } // isi pesan
const options = { quoted: null } // opsi tambahan (misalnya: balasan)

await sock.sendMessage(jid, content, options)
```

### Pesan Non-Media

#### Pesan Teks
```javascript
await sock.sendMessage(jid, { text: 'Halo dunia' })
```

#### Pesan Balasan (Quote)
```javascript
await sock.sendMessage(jid, { text: 'Ini balasan pesan kamu' }, { quoted: message })
```

#### Mention Pengguna (Tag)
Gunakan `@nomor` dalam teks dan sertakan `mentions` di payload.
```javascript
await sock.sendMessage(
  jid,
  {
    text: '@628XXXXXXXXX Hai Naruya!',
    mentions: ['628XXXXXXXXX@s.whatsapp.net']
  }
)
```

#### Meneruskan Pesan (Forward)
Butuh objek pesan (`WAMessage`). Bisa didapat dari store atau pesan sebelumnya.
```javascript
const msg = getMessageFromStore() // Kamu buat sendiri sesuai struktur
await sock.sendMessage(jid, { forward: msg, force: true })
```

### Pesan Media

#### Gambar
```javascript
await sock.sendMessage(jid, {
  image: fs.readFileSync('./gambar.jpg'),
  caption: 'Ini gambar keren!'
})
```

#### Video
```javascript
await sock.sendMessage(jid, {
  video: fs.readFileSync('./video.mp4'),
  caption: 'Tonton ini!'
})
```

#### Dokumen
```javascript
await sock.sendMessage(jid, {
  document: fs.readFileSync('./laporan.pdf'),
  fileName: 'Laporan.pdf',
  mimetype: 'application/pdf'
})
```

#### Audio / Voice Note
```javascript
await sock.sendMessage(jid, {
  audio: fs.readFileSync('./suara.mp3'),
  mimetype: 'audio/mp4',
  ptt: true // jika ingin dikirim sebagai voice note
})
```

### Pesan Interaktif

#### Tombol Teks (Buttons)
```javascript
await sock.sendMessage(jid, {
  text: 'Pilih salah satu:',
  buttons: [
    { buttonId: 'btn_1', buttonText: { displayText: 'Tombol 1' }, type: 1 },
    { buttonId: 'btn_2', buttonText: { displayText: 'Tombol 2' }, type: 1 }
  ],
  footer: 'Contoh footer'
})
```

#### Daftar (List Message)
```javascript
await sock.sendMessage(jid, {
  text: 'Pilih dari daftar berikut:',
  footer: 'Contoh footer',
  title: 'Judul Daftar',
  buttonText: 'Buka List',
  sections: [
    {
      title: 'Menu 1',
      rows: [
        { title: 'Opsi A', rowId: 'pilih_a' },
        { title: 'Opsi B', rowId: 'pilih_b' }
      ]
    },
    {
      title: 'Menu 2',
      rows: [
        { title: 'Opsi C', rowId: 'pilih_c' }
      ]
    }
  ]
})
```

> [!TIP]  
> Kamu bisa menggabungkan semua jenis pesan dengan opsi tambahan seperti `quoted`, `mentions`, `ephemeralExpiration`, dan lainnya untuk membuat interaksi bot yang lebih kaya dan interaktif.

#### Lokasi Biasa
```javascript
await sock.sendMessage(
  jid,
  {
    location: {
      degreesLatitude: -6.200000,
      degreesLongitude: 106.816666
    }
  }
)
```

#### Lokasi Langsung (Live Location)
```javascript
await sock.sendMessage(
  jid,
  {
    location: {
      degreesLatitude: -6.200000,
      degreesLongitude: 106.816666
    },
    live: true
  }
)
```

#### Kirim Kontak (vCard)
```javascript
const vcard =
  'BEGIN:VCARD\n' +
  'VERSION:3.0\n' +
  'FN:Naruya Izumi\n' +
  'ORG:ZERO DEV;\n' +
  'TEL;type=CELL;type=VOICE;waid=628XXXXXXXXX:+62 831-4366-3697\n' +
  'END:VCARD'

await sock.sendMessage(
  jid,
  {
    contacts: {
      displayName: 'Naruya Izumi',
      contacts: [{ vcard }]
    }
  }
)
```

#### Pesan Reaksi (Reaction Message)

- Kamu perlu mengirimkan `key` dari pesan yang ingin diberikan reaksi.  
  `key` bisa diambil dari [store](#mengimplementasikan-data-store) atau menggunakan [WAMessageKey](https://baileys.whiskeysockets.io/types/WAMessageKey.html).

```javascript
await sock.sendMessage(
  jid,
  {
    react: {
      text: '🔥', // gunakan string kosong '' untuk menghapus reaksi
      key: message.key
    }
  }
)
```

#### Pin Pesan (Pin Message)

- Kamu juga perlu memberikan `key` dari pesan yang ingin dipin.  
  Kamu dapat mengatur durasi pin berdasarkan waktu dalam detik.

| Durasi | Detik        |
|--------|--------------|
| 24 jam | 86.400       |
| 7 hari | 604.800      |
| 30 hari| 2.592.000    |

```javascript
await sock.sendMessage(
  jid,
  {
    pin: {
      type: 1, // 1 untuk pin, 2 untuk unpin
      time: 86400,
      key: message.key
    }
  }
)
```

### Menandai Pesan (Keep Message)

- Untuk menyimpan pesan tertentu agar tidak terhapus otomatis.

```javascript
await sock.sendMessage(
  jid,
  {
    keep: {
      key: message.key,
      type: 1 // 1 = simpan, 2 = batalkan simpan
    }
  }
)
```

#### Pesan Polling (Poll Message)

- Kirim polling ke grup atau kontak pribadi. Dapat menentukan apakah polling bersifat publik (announcement group).

```javascript
await sock.sendMessage(
  jid,
  {
    poll: {
      name: 'Polling Hari Ini',
      values: ['Opsi A', 'Opsi B', 'Opsi C'],
      selectableCount: 1,
      toAnnouncementGroup: false
    }
  }
)
```

#### Pesan Hasil Polling (Poll Result)

- Kirim hasil polling secara manual jika dibutuhkan. Cocok untuk sistem polling terintegrasi.

```javascript
await sock.sendMessage(
  jid,
  {
    pollResult: {
      name: 'Hasil Polling',
      values: [
        ['Opsi A', 120],
        ['Opsi B', 350],
        ['Opsi C', 75]
      ]
    }
  }
)
```

### Pesan Panggilan (Call Message)

- Digunakan untuk mengirim notifikasi panggilan, bisa suara atau video.

```javascript
await sock.sendMessage(
  jid,
  {
    call: {
      name: 'Hay',
      type: 1 // 1 = suara, 2 = video
    }
  }
)
```

### Pesan Event (Event Message)

- Cocok untuk mengumumkan acara atau undangan dengan detail lokasi dan waktu.

```javascript
await sock.sendMessage(
  jid,
  {
    event: {
      isCanceled: false, // true jika dibatalkan
      name: 'Liburan Bareng!',
      description: 'Siapa yang mau ikut?', 
      location: {
        degreesLatitude: 24.121231,
        degreesLongitude: 55.1121221,
        name: 'Pantai Sanur'
      },
      startTime: 1715000000, 
      endTime: 1715086400, 
      extraGuestsAllowed: true // apakah boleh bawa tamu
    }
  }
)
```

### Pesan Pemesanan (Order Message)

- Digunakan untuk menampilkan detail pemesanan dari katalog bisnis WhatsApp.

```javascript
await sock.sendMessage(
  jid,
  {
    order: {
      orderId: '574XXX',
      thumbnail: 'your_thumbnail', 
      itemCount: 3,
      status: 'INQUIRY', // atau ACCEPTED / DECLINED
      surface: 'CATALOG',
      message: 'Deskripsi pesanan',
      orderTitle: 'Judul Pesanan',
      sellerJid: '628xxx@s.whatsapp.net',
      token: 'your_token',
      totalAmount1000: '150000',
      totalCurrencyCode: 'IDR'
    }
  }
)
```

### Pesan Produk (Product Message)

- Menampilkan detail produk dari katalog bisnis.

```javascript
await sock.sendMessage(
  jid,
  {
    product: {
      productImage: { 
        url: 'https://your-image.url/image.jpg'
      },
      productId: 'PRD-001', 
      title: 'Produk Spesial',
      description: 'Deskripsi lengkap produk kamu di sini', 
      currencyCode: 'IDR', 
      priceAmount1000: '50000', 
      retailerId: 'store-izumi', // opsional
      url: 'https://linkproduk.com', // opsional
      productImageCount: 1, 
      firstImageId: 'img-001', // opsional
      salePriceAmount1000: '45000', 
      signedUrl: 'https://your.signed.url' // opsional
    },
    businessOwnerJid: '628xxx@s.whatsapp.net'
  }
)
```

### Pesan Pembayaran (Payment Message)

- Digunakan untuk mengirimkan informasi pembayaran, cocok untuk chatbot belanja.

```javascript
await sock.sendMessage(
  jid,
  {
    payment: {
      note: 'Hi!',
      currency: 'IDR',
      offset: 0,
      amount: '10000',
      expiry: 0,
      from: '628xxxx@s.whatsapp.net',
      image: {
        placeholderArgb: '#222222', 
        textArgb: '#FFFFFF',  
        subtextArgb: '#AAAAAA'
      }
    }
  }
)
```

#### Pesan Undangan Pembayaran (Payment Invite Message)

- Digunakan untuk mengundang pengguna lain melakukan pembayaran.

```javascript
await sock.sendMessage(
  jid, 
  { 
    paymentInvite: {
      type: 1, // 1 = request, 2 = accept, 3 = decline (sesuaikan sesuai konteks)
      expiry: 0
    }
  }
)
```

### Pesan Undangan Admin Channel (Admin Invite Message)

- Meminta pengguna untuk menjadi admin di saluran (newsletter) kamu.

```javascript
await sock.sendMessage(
  jid,
  {
    adminInvite: {
      jid: '123xxx@newsletter',
      name: 'Channel Naruya',
      caption: 'Tolong jadi admin channel saya ya!',
      expiration: 86400, // dalam detik (24 jam)
      jpegThumbnail: Buffer // opsional, bisa berupa buffer gambar
    }
  }
)
```

### Undangan Grup WhatsApp (Group Invite Message)

- Mengirim undangan ke grup tertentu menggunakan kode undangan.

```javascript
await sock.sendMessage(
  jid,
  {
    groupInvite: {
      jid: '123xxx@g.us',
      name: 'Grup Dev Naruya',
      caption: 'Ayo gabung ke grup WhatsApp kami!',
      code: 'ABCD1234', // kode undangan grup
      expiration: 86400,
      jpegThumbnail: Buffer // opsional
    }
  }
)
```

### Pesan Bagikan Nomor Telepon (Share Phone Number)

- Mengirim permintaan eksplisit untuk membagikan nomor telepon pengguna.

```javascript
await sock.sendMessage(
  jid,
  {
    sharePhoneNumber: {}
  }
)
```

### Pesan Permintaan Nomor Telepon (Request Phone Number)

- Meminta pengguna untuk membagikan nomor telepon mereka secara langsung.

```javascript
await sock.sendMessage(
  jid,
  {
    requestPhoneNumber: {}
  }
)
```

### Pesan Balasan Tombol (Button Reply Message)

- Digunakan untuk merespons interaksi tombol yang diklik pengguna. Tipe pesan dibedakan berdasarkan jenis tombol yang digunakan.

#### Tombol Tipe List
```javascript
await sock.sendMessage(
  jid,
  {
    buttonReply: {
      name: 'Hai', 
      description: 'Deskripsi pilihan', 
      rowId: 'pilihan_1'
    }, 
    type: 'list'
  }
)
```

#### Tombol Tipe Plain
```javascript
await sock.sendMessage(
  jid,
  {
    buttonReply: {
      displayText: 'Halo', 
      id: 'plain_id'
    }, 
    type: 'plain'
  }
)
```

#### Tombol Tipe Template
```javascript
await sock.sendMessage(
  jid,
  {
    buttonReply: {
      displayText: 'Pilih Saya', 
      id: 'template_id', 
      index: 1
    }, 
    type: 'template'
  }
)
```

#### Tombol Tipe Interactive (Native Flow)
```javascript
await sock.sendMessage(
  jid,
  {
    buttonReply: {
      body: 'Mau pilih yang mana?', 
      nativeFlows: {
        name: 'menu_options', 
        paramsJson: JSON.stringify({ id: 'menu_1', description: 'Deskripsi interaktif' }),
        version: 1 // bisa juga 2 atau 3
      }
    }, 
    type: 'interactive'
  }
)
```

### Pesan dengan Tombol (Buttons Message)

- Pesan biasa yang disertai hingga **3 tombol** untuk respon cepat.

```javascript
await sock.sendMessage(
  jid,
  {
    text: 'Ini adalah pesan tombol!',
    caption: 'Gunakan jika memakai gambar/video',
    footer: 'Salam dari Naruya Izumi!',
    buttons: [
      { 
        buttonId: 'btn1', 
        buttonText: { displayText: 'Tombol 1' }
      },
      { 
        buttonId: 'btn2', 
        buttonText: { displayText: 'Tombol 2' }
      },
      { 
        buttonId: 'btn3', 
        buttonText: { displayText: 'Tombol 3' }
      }
    ]
  }
)
```

### Pesan List Tombol (Buttons List Message)

- Hanya bisa digunakan di **chat pribadi**, bukan grup.

```javascript
await sock.sendMessage(
  jid,
  {
    text: 'Ini adalah daftar pilihan!',
    footer: 'Dipersembahkan oleh Naruya Izumi',
    title: 'Judul Daftar Pilihan',
    buttonText: 'Klik untuk melihat opsi',
    sections: [
      {
        title: 'Bagian 1',
        rows: [
          { title: 'Opsi 1', rowId: 'opsi1' },
          { title: 'Opsi 2', rowId: 'opsi2', description: 'Deskripsi Opsi 2' }
        ]
      },
      {
        title: 'Bagian 2',
        rows: [
          { title: 'Opsi 3', rowId: 'opsi3' },
          { title: 'Opsi 4', rowId: 'opsi4', description: 'Deskripsi Opsi 4' }
        ]
      }
    ]
  }
)
```

### Pesan Daftar Produk dengan Tombol (Buttons Product List Message)

- Hanya dapat digunakan di **chat pribadi**, bukan grup.  
- Menampilkan daftar produk dari katalog bisnis WhatsApp kamu.

```javascript
await sock.sendMessage(
  jid,
  {
    text: 'Ini adalah daftar produk!',
    footer: 'Dikirim oleh Naruya Izumi',
    title: 'Pilih Produk Unggulan',
    buttonText: 'Lihat Daftar Produk',
    productList: [
      {
        title: 'Kategori Produk Utama',
        products: [
          { productId: '1234' },
          { productId: '5678' }
        ]
      }
    ],
    businessOwnerJid: '628xxx@s.whatsapp.net',
    thumbnail: 'https://example.jpg' // atau buffer gambar
  }
)
```

### Pesan Kartu dengan Tombol (Buttons Cards Message)

- Menampilkan beberapa kartu (card) interaktif dengan gambar atau video + tombol.

```javascript
await sock.sendMessage(
  jid,
  {
    text: 'Isi Utama Pesan',
    title: 'Judul Utama',
    subtile: 'Subjudul Opsional',
    footer: 'Footer Pesan',

    cards: [
      {
        image: { url: 'https://example.jpg' }, // bisa juga Buffer
        title: 'Judul Kartu',
        body: 'Isi Konten Kartu',
        footer: 'Footer Kartu',
        buttons: [
          {
            name: 'quick_reply',
            buttonParamsJson: JSON.stringify({
              display_text: 'Tombol Cepat',
              id: 'ID_TOMBOL_1'
            })
          },
          {
            name: 'cta_url',
            buttonParamsJson: JSON.stringify({
              display_text: 'Kunjungi Website',
              url: 'https://www.example.com'
            })
          }
        ]
      },
      {
        video: { url: 'https://example.mp4' }, // bisa juga Buffer video
        title: 'Judul Kartu Video',
        body: 'Deskripsi Konten',
        footer: 'Footer Kartu',
        buttons: [
          {
            name: 'quick_reply',
            buttonParamsJson: JSON.stringify({
              display_text: 'Respon Cepat',
              id: 'ID_TOMBOL_2'
            })
          },
          {
            name: 'cta_url',
            buttonParamsJson: JSON.stringify({
              display_text: 'Lihat Selengkapnya',
              url: 'https://www.example.com'
            })
          }
        ]
      }
    ]
  }
)
```

### Pesan Tombol Template (Buttons Template Message)

- Menampilkan tombol dengan tipe URL, panggilan, atau tombol balasan cepat.

```javascript
await sock.sendMessage(
  jid,
  {
    text: 'Ini adalah pesan template tombol!',
    footer: 'Dikirim oleh Naruya Izumi',
    templateButtons: [
      {
        index: 1,
        urlButton: {
          displayText: 'Ikuti Channel',
          url: 'https://whatsapp.com/channel/0029Vag9VSI2ZjCocqa2lB1y'
        }
      },
      {
        index: 2,
        callButton: {
          displayText: 'Hubungi Saya!',
          phoneNumber: '628xxxx'
        }
      },
      {
        index: 3,
        quickReplyButton: {
          displayText: 'Balas Cepat',
          id: 'id-button-reply'
        }
      }
    ]
  }
)
```

### Pesan Tombol Interaktif (Interactive Buttons)

- Mendukung berbagai jenis tombol dan dapat digunakan dengan media.

```javascript
await sock.sendMessage(
  jid,
  {
    text: 'Ini pesan interaktif!',
    title: 'Hai!',
    subtitle: 'Subjudul di sini',
    footer: 'Dikirim oleh Naruya Izumi',
    interactiveButtons: [
      {
        name: 'quick_reply',
        buttonParamsJson: JSON.stringify({
          display_text: 'Klik Aku!',
          id: 'id_kamu'
        })
      },
      {
        name: 'cta_url',
        buttonParamsJson: JSON.stringify({
          display_text: 'Kunjungi Channel',
          url: 'https://whatsapp.com/channel/0029Vag9VSI2ZjCocqa2lB1y',
          merchant_url: 'https://whatsapp.com/channel/0029Vag9VSI2ZjCocqa2lB1y'
        })
      },
      {
        name: 'cta_copy',
        buttonParamsJson: JSON.stringify({
          display_text: 'Salin Link',
          copy_code: 'https://whatsapp.com/channel/0029Vag9VSI2ZjCocqa2lB1y'
        })
      },
      {
        name: 'cta_call',
        buttonParamsJson: JSON.stringify({
          display_text: 'Telepon Saya',
          phone_number: '628xxxx'
        })
      },
      {
        name: 'single_select',
        buttonParamsJson: JSON.stringify({
          title: 'Pilih Opsi',
          sections: [
            {
              title: 'Pilihan Utama',
              highlight_label: 'Rekomendasi',
              rows: [
                {
                  header: 'Header 1',
                  title: 'Opsi 1',
                  description: 'Deskripsi 1',
                  id: 'id1'
                },
                {
                  header: 'Header 2',
                  title: 'Opsi 2',
                  description: 'Deskripsi 2',
                  id: 'id2'
                }
              ]
            }
          ]
        })
      }
    ]
  }
)
```

#### Versi dengan Media

##### Gambar
```javascript
await sock.sendMessage(
  jid,
  {
    image: { url: 'https://example.jpg' },
    caption: 'Isi Pesan',
    title: 'Judul',
    subtitle: 'Subjudul',
    footer: 'Footer',
    interactiveButtons: [ /* tombol seperti di atas */ ],
    hasMediaAttachment: false
  }
)
```

##### Video
```javascript
await sock.sendMessage(
  jid,
  {
    video: { url: 'https://example.mp4' },
    caption: 'Isi Video',
    title: 'Judul',
    subtitle: 'Subjudul',
    footer: 'Footer',
    interactiveButtons: [ /* tombol seperti di atas */ ],
    hasMediaAttachment: false
  }
)
```

##### Dokumen
```javascript
await sock.sendMessage(
  jid,
  {
    document: { url: 'https://example.jpg' },
    mimetype: 'image/jpeg',
    jpegThumbnail: await sock.resize('https://example.jpg', 320, 320),
    caption: 'Isi Dokumen',
    title: 'Judul',
    subtitle: 'Subjudul',
    footer: 'Footer',
    interactiveButtons: [ /* tombol seperti di atas */ ],
    hasMediaAttachment: false
  }
)
```

##### Lokasi
```javascript
await sock.sendMessage(
  jid,
  {
    location: {
      degreesLatitude: -6.2,
      degreesLongitude: 106.8,
      name: 'Naruya HQ'
    },
    caption: 'Ayo ke sini!',
    title: 'Lokasi Tujuan',
    subtitle: 'Subjudul Lokasi',
    footer: 'Peta lokasi',
    interactiveButtons: [ /* tombol seperti di atas */ ],
    hasMediaAttachment: false
  }
)
```

##### Produk
```javascript
await sock.sendMessage(
  jid,
  {
    product: {
      productImage: { url: 'https://example.jpg' },
      productId: '836xxx',
      title: 'Produk Pilihan',
      description: 'Deskripsi produk terbaik',
      currencyCode: 'IDR',
      priceAmount1000: '283000',
      retailerId: 'NaruyaStore',
      url: 'https://example.com',
      productImageCount: 1
    },
    businessOwnerJid: '628xxx@s.whatsapp.net',
    caption: 'Produk baru tersedia!',
    title: 'Nama Produk',
    subtitle: 'Subjudul Produk',
    footer: 'Info Produk',
    interactiveButtons: [ /* tombol seperti di atas */ ],
    hasMediaAttachment: false
  }
)
```

### Mention Status (Status Mentions Message)

- Digunakan untuk membuat status WhatsApp yang menyebut seseorang secara langsung.

```javascript
await sock.sendStatusMentions(
  jid, 
  {
    image: {
      url: 'https://example.com.jpg'
    }, 
    caption: 'Halo dari Naruya!'
  }
)
```

### Pesan Album (Send Album Message)

- Mengirim beberapa gambar atau video sebagai album (sekuens media). Bisa pakai `Buffer` atau URL.

```javascript
await sock.sendAlbumMessage(
  jid,
  [
    {
      image: { url: 'https://example.jpg' }, 
      caption: 'Gambar 1'
    },
    {
      image: Buffer, 
      caption: 'Gambar 2'
    },
    {
      video: { url: 'https://example.mp4' }, 
      caption: 'Video 1'
    }, 
    {
      video: Buffer, 
      caption: 'Video 2'
    }
  ],
  { 
    quoted: message, // opsional, untuk membalas pesan
    delay: 2000 // jeda antar media (ms)
  }
)
```

### Pesan Toko (Shop Message)

- Digunakan untuk mengarahkan pengguna ke katalog atau produk dalam fitur bisnis WhatsApp.

#### Teks Saja
```javascript
await sock.sendMessage(
  jid, 
  {      
    text: 'Body pesan',
    title: 'Judul Toko', 
    subtitle: 'Subjudul', 
    footer: 'Powered by Naruya',
    shop: {
      surface: 1,
      id: 'https://example.com'
    }, 
    viewOnce: true
  }
)
```

#### Gambar
```javascript
await sock.sendMessage(
  jid, 
  { 
    image: { url: 'https://example.jpg' },
    caption: 'Deskripsi produk',
    title: 'Judul',
    subtitle: 'Subjudul',
    footer: 'Footer',
    shop: {
      surface: 1,
      id: 'https://example.com'
    },
    hasMediaAttachment: false,
    viewOnce: true
  }
)
```

#### Video
```javascript
await sock.sendMessage(
  jid, 
  { 
    video: { url: 'https://example.mp4' },
    caption: 'Tonton videonya!',
    title: 'Judul Video',
    subtitle: 'Subjudul',
    footer: 'Footer',
    shop: {
      surface: 1,
      id: 'https://example.com'
    },
    hasMediaAttachment: false,
    viewOnce: true
  }
)
```

#### Dokumen
```javascript
await sock.sendMessage(
  jid, 
  {
    document: { url: 'https://example.jpg' },
    mimetype: 'image/jpeg',
    jpegThumbnail: await sock.resize('https://example.jpg', 320, 320),
    caption: 'Lampiran dokumen',
    title: 'Judul',
    subtitle: 'Subjudul',
    footer: 'Footer',
    shop: {
      surface: 1,
      id: 'https://example.com'
    },
    hasMediaAttachment: false,
    viewOnce: true
  }
)
```

#### Lokasi
```javascript
await sock.sendMessage(
  jid, 
  { 
    location: {
      degreesLatitude: -6.2000, 
      degreesLongitude: 106.8166,
      name: 'Lokasi Toko'
    },
    caption: 'Lihat lokasi kami!',
    title: 'Judul Lokasi',
    subtitle: 'Subjudul',
    footer: 'Peta lokasi',
    shop: {
      surface: 1,
      id: 'https://example.com'
    },
    hasMediaAttachment: false,
    viewOnce: true
  }
)
```

#### Produk
```javascript
await sock.sendMessage(
  jid,
  {
    product: {
      productImage: { url: 'https://example.jpg' },
      productId: '836xxx',
      title: 'Nama Produk',
      description: 'Deskripsi produk menarik',
      currencyCode: 'IDR',
      priceAmount1000: '283000',
      retailerId: 'NaruyaStore',
      url: 'https://example.com',
      productImageCount: 1
    },
    businessOwnerJid: '628xxx@s.whatsapp.net',
    caption: 'Lihat produk unggulan kami!',
    title: 'Judul Produk',
    subtitle: 'Subjudul Produk',
    footer: 'Info produk',
    shop: {
      surface: 1,
      id: 'https://example.com'
    },
    hasMediaAttachment: false,
    viewOnce: true
  }
)
```

### Pesan Koleksi (Collection Message)

- Fitur ini digunakan untuk menampilkan koleksi katalog dari bisnis tertentu di WhatsApp.

#### Teks Saja
```javascript
await sock.sendMessage(
  jid, 
  {
    text: 'Isi pesan',
    title: 'Judul Koleksi',
    subtitle: 'Subjudul',
    footer: 'Dari Naruya Izumi',
    collection: {
      bizJid: '628xxx@s.whatsapp.net', 
      id: 'https://example.com', 
      version: 1
    },
    viewOnce: true
  }
)
```

#### Gambar
```javascript
await sock.sendMessage(
  jid, 
  { 
    image: { url: 'https://example.jpg' },
    caption: 'Koleksi Gambar',
    title: 'Judul Koleksi',
    subtitle: 'Subjudul',
    footer: 'Katalog Naruya',
    collection: {
      bizJid: '628xxx@s.whatsapp.net', 
      id: 'https://example.com',
      version: 1
    },
    hasMediaAttachment: false,
    viewOnce: true
  }
)
```

#### Video
```javascript
await sock.sendMessage(
  jid, 
  {
    video: { url: 'https://example.mp4' },
    caption: 'Koleksi Video',
    title: 'Judul Video',
    subtitle: 'Subjudul',
    footer: 'Video Katalog',
    collection: {
      bizJid: '628xxx@s.whatsapp.net', 
      id: 'https://example.com',
      version: 1
    },
    hasMediaAttachment: false,
    viewOnce: true
  }
)
```

#### Dokumen
```javascript
await sock.sendMessage(
  jid, 
  {
    document: { url: 'https://example.jpg' },
    mimetype: 'image/jpeg',
    jpegThumbnail: await sock.resize('https://example.jpg', 320, 320),
    caption: 'Dokumen Katalog',
    title: 'Judul Dokumen',
    subtitle: 'Subjudul',
    footer: 'Lampiran Koleksi',
    collection: {
      bizJid: '628xxx@s.whatsapp.net',
      id: 'https://example.com',
      version: 1
    },
    hasMediaAttachment: false,
    viewOnce: true
  }
)
```

#### Lokasi
```javascript
await sock.sendMessage(
  jid, 
  {
    location: {
      degreesLatitude: -6.2, 
      degreesLongitude: 106.8,
      name: 'Lokasi Bisnis'
    },
    caption: 'Lihat lokasi koleksi',
    title: 'Judul Lokasi',
    subtitle: 'Subjudul',
    footer: 'Lokasi Katalog',
    collection: {
      bizJid: '628xxx@s.whatsapp.net',
      id: 'https://example.com',
      version: 1
    },
    hasMediaAttachment: false,
    viewOnce: true
  }
)
```

#### Produk
```javascript
await sock.sendMessage(
  jid,
  {
    product: {
      productImage: { url: 'https://example.jpg' },
      productId: '836xxx',
      title: 'Nama Produk',
      description: 'Deskripsi produk',
      currencyCode: 'IDR',
      priceAmount1000: '283000',
      retailerId: 'NaruyaStore',
      url: 'https://example.com',
      productImageCount: 1
    },
    businessOwnerJid: '628xxx@s.whatsapp.net',
    caption: 'Koleksi Produk',
    title: 'Judul',
    subtitle: 'Subjudul',
    footer: 'Produk Katalog',
    collection: {
      bizJid: '628xxx@s.whatsapp.net',
      id: 'https://example.com',
      version: 1
    },
    hasMediaAttachment: false,
    viewOnce: true
  }
)
```

### Mengirim Pesan dengan Pratinjau Link (Link Preview)

1. Secara default, WhatsApp Web tidak menampilkan pratinjau link.
2. Namun, Baileys menyediakan fungsi pembangkit preview link otomatis.
3. Untuk mengaktifkannya, install dulu dependensinya dengan:  
   ```bash
   yarn add link-preview-js
   ```
4. Contoh kirim pesan dengan pratinjau link:
```javascript
await sock.sendMessage(
  jid,
  {
    text: 'Hai! Ini dikirim dari https://github.com/whiskeysockets/baileys'
  }
)
```

### Pesan Media (Media Messages)

Mengirim media (gambar, video, audio, stiker) jauh lebih efisien dengan Baileys.

> [!NOTE]  
> Kamu bisa menggunakan `Buffer`, `{ stream }`, atau `{ url }`.  
> Lihat lebih lengkap di [dokumentasi media](https://baileys.whiskeysockets.io/types/WAMediaUpload.html)

> [!TIP]  
> Gunakan **stream** atau **url langsung** agar lebih hemat memori.

#### Pesan GIF (video pendek)

> WhatsApp tidak mendukung file `.gif`, maka harus dikirim dalam bentuk `.mp4` dengan flag `gifPlayback: true`

```javascript
await sock.sendMessage(
  jid,
  {
    video: fs.readFileSync('Media/ma_gif.mp4'),
    caption: 'Halo dari GIF!',
    gifPlayback: true
  }
)
```

#### Pesan Video
```javascript
await sock.sendMessage(
  jid,
  {
    video: { url: './Media/ma_video.mp4' },
    caption: 'Ini videonya'
  }
)
```

#### Pesan Video PTV (Picture to Video / video bulat WA)

```javascript
await sock.sendMessage(
  jid,
  {
    video: { url: './Media/ma_video.mp4' },
    ptv: true
  }
)
```

#### Pesan Audio

> Agar audio kompatibel di semua perangkat, sebaiknya gunakan `ffmpeg` dengan pengaturan berikut:

```bash
ffmpeg -i input.mp4 -avoid_negative_ts make_zero -ac 1 output.ogg
```

```javascript
await sock.sendMessage(
  jid,
  {
    audio: { url: './Media/audio.ogg' },
    mimetype: 'audio/ogg; codecs=opus'
  }
)
```

#### Pesan Gambar

```javascript
await sock.sendMessage(
  jid,
  {
    image: { url: './Media/ma_img.png' },
    caption: 'Halo dari gambar!'
  }
)
```

#### Pesan View Once

> Fitur **View Once** memungkinkan media hanya bisa dilihat satu kali.

```javascript
await sock.sendMessage(
  jid,
  {
    image: { url: './Media/ma_img.png' },
    viewOnce: true,
    caption: 'Media hanya bisa dilihat sekali'
  }
)
```

## Memodifikasi Pesan

### Menghapus Pesan (Untuk Semua Orang)

- Digunakan untuk menarik pesan yang sudah dikirim (delete for everyone).

```javascript
const msg = await sock.sendMessage(jid, { text: 'Halo dunia' })
await sock.sendMessage(jid, { delete: msg.key })
```

> **Catatan:**  
> Untuk menghapus pesan **hanya untuk diri sendiri**, gunakan `chatModify` (lihat bagian [Modifikasi Chat](#modifying-chats)).

### Mengedit Pesan

- Kamu dapat mengedit isi pesan yang telah dikirim sebelumnya, selama masih berada dalam konteks yang diizinkan oleh WhatsApp.

```javascript
await sock.sendMessage(jid, {
  text: 'Teks yang sudah diperbarui di sini',
  edit: response.key
})
```

## Memanipulasi Pesan Media

### Menambahkan Thumbnail pada Media

- Thumbnail (gambar pratinjau) untuk **gambar** dan **stiker** bisa dihasilkan secara otomatis jika kamu menambahkan salah satu dari dependency berikut:

```bash
yarn add jimp
# atau
yarn add sharp
```

- Untuk **video**, kamu juga bisa menghasilkan thumbnail otomatis, tapi pastikan kamu sudah install `ffmpeg` di sistem kamu.

> Contoh penggunaan otomatis biasanya tidak perlu kamu atur manual — Baileys akan meng-generate thumbnail bila dependensi sudah tersedia.

### Mengunduh Media dari Pesan (Downloading Media Messages)

Jika kamu ingin menyimpan media yang diterima dari pengguna:

```javascript
import { createWriteStream } from 'fs'
import { downloadMediaMessage, getContentType } from 'naruyaizumi'

sock.ev.on('messages.upsert', async ({ messages }) => {
let m = messages[0]
if (!m.message) return // jika tidak ada media atau isi pesan

let messageType = getContentType(m.message) // deteksi tipe pesan (image, video, audio, dll)

if (messageType === 'imageMessage') {
let stream = await downloadMediaMessage(
m,
'stream', // bisa juga 'buffer' kalau ingin langsung di-handle tanpa file
{},
{
logger,
reuploadRequest: sock.updateMediaMessage // agar bisa reupload jika file sudah tidak ada
}
)

let file = createWriteStream('./downloaded-image.jpeg')
stream.pipe(file)
}
})
```

### Re-upload Media ke WhatsApp

Jika media sudah dihapus dari server WhatsApp, kamu bisa minta perangkat pengirim untuk melakukan *reupload*:

```javascript
await sock.updateMediaMessage(msg)
```

> Fitur ini penting saat media gagal diunduh karena sudah tidak tersedia di server WhatsApp.

## Menolak Panggilan (Reject Call)

- Kamu bisa mendapatkan `callId` dan `callFrom` dari event `call`.

```javascript
await sock.rejectCall(callId, callFrom)
```

## Mengirim Status ke Chat (Send States in Chat)

### Menandai Pesan Dibaca (Reading Messages)

- Kamu harus menandai pesan satu per satu menggunakan key dari `WAMessage`.
- Tidak bisa menandai seluruh chat sebagai terbaca secara langsung seperti di WhatsApp Web.

```javascript
const key = {
remoteJid: '628xxx@s.whatsapp.net',
fromMe: false,
id: 'ABCDEF123456'
}

// bisa juga array untuk banyak pesan sekaligus
await sock.readMessages([key])
```

> Kamu bisa mendapatkan `messageID` dari:
```javascript
let messageID = message.key.id
```

### Memperbarui Status Kehadiran (Update Presence)

- Status `presence` bisa berupa:  
  `available`, `unavailable`, `composing`, `recording`, `paused`, dll.  
  [Lihat daftar lengkapnya di sini](https://baileys.whiskeysockets.io/types/WAPresence.html)

```javascript
await sock.sendPresenceUpdate('available', jid) // online
await sock.sendPresenceUpdate('composing', jid) // mengetik
await sock.sendPresenceUpdate('unavailable', jid) // offline
```

> **Catatan:**  
> Jika kamu menggunakan WhatsApp Desktop secara bersamaan, maka WA tidak akan mengirim notifikasi ke perangkat lain.  
> Kalau kamu ingin tetap terima notifikasi di HP, kamu bisa set status bot jadi offline:
```javascript
await sock.sendPresenceUpdate('unavailable')
```

## Memodifikasi Chat (Modifying Chats)

WhatsApp menggunakan komunikasi terenkripsi untuk memperbarui status chat atau aplikasi. Beberapa fitur modifikasi sudah didukung oleh Baileys, dan bisa kamu kirim seperti di bawah ini.

> **PERINGATAN:**  
> Jika kamu salah menggunakan modifikasi ini (misal kirim data invalid), WhatsApp bisa **logout semua perangkat** dan kamu harus scan ulang QR.

### Mengarsipkan Chat (Archive)

```javascript
let lastMsgInChat = await getLastMessageInChat(jid) // kamu buat fungsi ini sendiri
await sock.chatModify({ archive: true, lastMessages: [lastMsgInChat] }, jid)
```

### Membisukan / Mengaktifkan Notifikasi (Mute / Unmute)

| Durasi    | Milidetik       |
|-----------|------------------|
| Hapus     | `null`           |
| 8 Jam     | `86400000`       |
| 7 Hari    | `604800000`      |

```javascript
await sock.chatModify({ mute: 8 * 60 * 60 * 1000 }, jid) // bisukan 8 jam
await sock.chatModify({ mute: null }, jid) // aktifkan kembali notifikasi
```

### Tandai Sebagai Terbaca / Belum Dibaca

```javascript
let lastMsgInChat = await getLastMessageInChat(jid)
await sock.chatModify({ markRead: false, lastMessages: [lastMsgInChat] }, jid)
```

### Hapus Pesan Hanya untuk Saya

```javascript
await sock.chatModify(
  {
    clear: {
      messages: [
        {
          id: 'ATWYHDNNWU81732J',
          fromMe: true,
          timestamp: '1654823909'
        }
      ]
    }
  },
  jid
)
```

### Hapus Chat Secara Keseluruhan

```javascript
let lastMsgInChat = await getLastMessageInChat(jid)
await sock.chatModify({
  delete: true,
  lastMessages: [
    {
      key: lastMsgInChat.key,
      messageTimestamp: lastMsgInChat.messageTimestamp
    }
  ]
}, jid)
```

### Pin / Unpin Chat

```javascript
await sock.chatModify({
  pin: true // false untuk unpin
}, jid)
```

### Tandai / Hapus Bintang dari Pesan

```javascript
await sock.chatModify({
  star: {
    messages: [
      {
        id: 'messageID',
        fromMe: true
      }
    ],
    star: true // true: beri bintang, false: hapus bintang
  }
}, jid)
```

### Pesan Menghilang Otomatis (Disappearing Messages)

| Durasi    | Detik (Seconds) |
|-----------|------------------|
| Nonaktif  | `0`              |
| 24 Jam    | `86400`          |
| 7 Hari    | `604800`         |
| 90 Hari   | `7776000`        |

#### Aktifkan

```javascript
await sock.sendMessage(jid, {
  disappearingMessagesInChat: 604800 // 7 hari
})
```

#### Kirim Pesan dengan Mode Menghilang

```javascript
await sock.sendMessage(
  jid,
  { text: 'halo' },
  { ephemeralExpiration: 604800 }
)
```

#### Nonaktifkan

```javascript
await sock.sendMessage(jid, {
  disappearingMessagesInChat: false
})
```

### Menghapus Pesan Tertentu (Clear Messages)
```javascript
await sock.clearMessage(jid, key, timestamps)
```

## Query Pengguna (User Queries)

### Cek Apakah Nomor Terdaftar di WhatsApp
```javascript
let [result] = await sock.onWhatsApp(jid)
if (result.exists) console.log(`${jid} terdaftar di WhatsApp sebagai ${result.jid}`)
```

### Ambil Riwayat Chat (termasuk grup)

> Kamu perlu mengambil pesan paling lama dari chat tersebut

```javascript
let msg = await getOldestMessageInChat(jid)
await sock.fetchMessageHistory(
  50, // maksimal 50 per query
  msg.key,
  msg.messageTimestamp
)
```

- Hasilnya akan dikirimkan melalui event `messaging.history-set`

### Ambil Status WhatsApp (Bio)

```javascript
let status = await sock.fetchStatus(jid)
console.log('Status: ' + status)
```

### Ambil Foto Profil (Profil, Grup, Channel)

```javascript
let ppUrl = await sock.profilePictureUrl(jid)
console.log('Foto profil: ' + ppUrl)
```

### Ambil Profil Bisnis (Business Profile)

> Cocok untuk akun bisnis WhatsApp, seperti deskripsi & kategori bisnis

```javascript
let profile = await sock.getBusinessProfile(jid)
console.log('Deskripsi bisnis: ' + profile.description + ', Kategori: ' + profile.category)
```

### Cek Kehadiran Seseorang (Presence: Online / Typing)

```javascript
sock.ev.on('presence.update', console.log)
await sock.presenceSubscribe(jid)
```

## Ubah Profil

### Ubah Status Profil (Bio)

```javascript
await sock.updateProfileStatus('Halo Dunia!')
```

### Ubah Nama Profil

```javascript
await sock.updateProfileName('Naruya Izumi')
```

### Ubah Foto Profil (termasuk grup)

> Sama seperti pesan media, kamu bisa pakai:  
> `{ url }`, `Buffer`, atau `{ stream }`

```javascript
await sock.updateProfilePicture(jid, { url: './foto-baru.jpeg' })
```

### Hapus Foto Profil (termasuk grup)

```javascript
await sock.removeProfilePicture(jid)
```

## Grup WhatsApp (Groups)

> Untuk mengubah pengaturan grup, kamu harus menjadi admin grup tersebut.

### Membuat Grup
```javascript
let group = await sock.groupCreate('Grup Hebat Naruya', ['1234@s.whatsapp.net', '4564@s.whatsapp.net'])
console.log('Grup berhasil dibuat dengan ID: ' + group.gid)

await sock.sendMessage(group.id, { text: 'Halo semuanya!' })
```

### Tambah / Hapus / Jadikan Admin / Turunkan Admin

```javascript
await sock.groupParticipantsUpdate(
  jid,
  ['abcd@s.whatsapp.net', 'efgh@s.whatsapp.net'],
  'add' // bisa diganti: 'remove', 'promote', 'demote'
)
```

### Ubah Nama Grup

```javascript
await sock.groupUpdateSubject(jid, 'Nama Baru Grup!')
```

### Ubah Deskripsi Grup

```javascript
await sock.groupUpdateDescription(jid, 'Deskripsi baru untuk grup ini')
```

### Ubah Pengaturan Grup

```javascript
// hanya admin yang bisa kirim pesan
await sock.groupSettingUpdate(jid, 'announcement')

// semua anggota bisa kirim pesan
await sock.groupSettingUpdate(jid, 'not_announcement')

// semua anggota bisa ubah info grup (foto, nama, dll.)
await sock.groupSettingUpdate(jid, 'unlocked')

// hanya admin yang bisa ubah info grup
await sock.groupSettingUpdate(jid, 'locked')
```

### Keluar dari Grup

```javascript
await sock.groupLeave(jid)
```

### Dapatkan Kode Undangan Grup

```javascript
let code = await sock.groupInviteCode(jid)
console.log('Kode undangan grup: ' + code)
// gabung pakai: https://chat.whatsapp.com/ + code
```

### Reset / Ganti Kode Undangan Grup

```javascript
let newCode = await sock.groupRevokeInvite(jid)
console.log('Kode undangan baru: ' + newCode)
```

### Gabung Grup dengan Kode Undangan

```javascript
let response = await sock.groupAcceptInvite('ABC123DEF456')
console.log('Berhasil gabung ke grup: ' + response)
```

### Lihat Info Grup dari Kode Undangan

```javascript
let response = await sock.groupGetInviteInfo('ABC123DEF456')
console.log('Info grup: ', response)
```

### Lihat Metadata Grup (peserta, nama, deskripsi, dll.)

```javascript
let metadata = await sock.groupMetadata(jid)
console.log(metadata.id + ', Nama: ' + metadata.subject + ', Deskripsi: ' + metadata.desc)
```

### Gabung Grup dari `groupInviteMessage`

```javascript
let response = await sock.groupAcceptInviteV4(jid, groupInviteMessage)
console.log('Gabung ke grup: ' + response)
```

### Lihat Daftar Pengguna yang Minta Gabung

```javascript
let response = await sock.groupRequestParticipantsList(jid)
console.log(response)
```

### Setujui / Tolak Permintaan Gabung

```javascript
let response = await sock.groupRequestParticipantsUpdate(
  jid,
  ['abcd@s.whatsapp.net', 'efgh@s.whatsapp.net'],
  'approve' // atau 'reject'
)
console.log(response)
```

### Dapatkan Metadata Semua Grup yang Kamu Ikuti

```javascript
let allGroups = await sock.groupFetchAllParticipating()
console.log(allGroups)
```

### Aktifkan Pesan Sementara di Grup (Ephemeral Message)

| Durasi    | Detik (Seconds) |
|-----------|------------------|
| Nonaktif  | 0                |
| 24 Jam    | 86400            |
| 7 Hari    | 604800           |
| 90 Hari   | 7776000          |

```javascript
await sock.groupToggleEphemeral(jid, 86400) // contoh: aktif 1 hari
```

### Ubah Mode Penambahan Anggota Grup

```javascript
await sock.groupMemberAddMode(
  jid,
  'all_member_add' // atau 'admin_add'
)
```

## Privasi (Privacy)

### Blokir / Buka Blokir Pengguna

```javascript
await sock.updateBlockStatus(jid, 'block') // Blokir pengguna
await sock.updateBlockStatus(jid, 'unblock') // Buka blokir pengguna
```

### Ambil Pengaturan Privasi

```javascript
let privacySettings = await sock.fetchPrivacySettings(true)
console.log('Pengaturan privasi:', privacySettings)
```

### Lihat Daftar Blokir

```javascript
let blocklist = await sock.fetchBlocklist()
console.log(blocklist)
```

### Ubah Privasi Terakhir Dilihat (Last Seen)

```javascript
let value = 'all' // bisa juga: 'contacts', 'contact_blacklist', 'none'
await sock.updateLastSeenPrivacy(value)
```

### Ubah Privasi Status Online

```javascript
let value = 'all' // atau 'match_last_seen'
await sock.updateOnlinePrivacy(value)
```

### Ubah Privasi Foto Profil

```javascript
let value = 'all' // bisa juga: 'contacts', 'contact_blacklist', 'none'
await sock.updateProfilePicturePrivacy(value)
```

### Ubah Privasi Status WhatsApp

```javascript
let value = 'all' // bisa juga: 'contacts', 'contact_blacklist', 'none'
await sock.updateStatusPrivacy(value)
```

### Ubah Privasi Centang Biru (Read Receipts)

```javascript
let value = 'all' // atau 'none'
await sock.updateReadReceiptsPrivacy(value)
```

### Ubah Privasi Siapa yang Bisa Menambahkan ke Grup

```javascript
let value = 'all' // bisa juga: 'contacts', 'contact_blacklist'
await sock.updateGroupsAddPrivacy(value)
```

### Ubah Mode Default Pesan Sementara

Durasi dalam detik:

| Durasi    | Detik (Seconds) |
|-----------|------------------|
| Nonaktif  | 0                |
| 24 Jam    | 86400            |
| 7 Hari    | 604800           |
| 90 Hari   | 7776000          |

```javascript
let ephemeral = 86400
await sock.updateDefaultDisappearingMode(ephemeral)
```

### NEWSLETTER

- **Mendapatkan informasi newsletter**
```javascript
const metadata = await sock.newsletterMetadata("invite", "xxxxx")
// atau
const metadata = await sock.newsletterMetadata("jid", "abcd@newsletter")
console.log(metadata)
```

- **Mengubah deskripsi newsletter**
```javascript
await sock.newsletterUpdateDescription("abcd@newsletter", "Deskripsi Baru")
```

- **Mengubah nama newsletter**
```javascript
await sock.newsletterUpdateName("abcd@newsletter", "Nama Baru")
```

- **Mengubah foto profil newsletter**
```javascript
await sock.newsletterUpdatePicture("abcd@newsletter", buffer)
```

- **Menghapus foto profil newsletter**
```javascript
await sock.newsletterRemovePicture("abcd@newsletter")
```

- **Mematikan notifikasi newsletter**
```javascript
await sock.newsletterMute("abcd@newsletter")
```

- **Mengaktifkan kembali notifikasi newsletter**
```javascript
await sock.newsletterUnmute("abcd@newsletter")
```

- **Membuat newsletter baru**
```javascript
const metadata = await sock.newsletterCreate("Nama Newsletter", "Deskripsi Newsletter")
console.log(metadata)
```

- **Menghapus newsletter**
```javascript
await sock.newsletterDelete("abcd@newsletter")
```

- **Mengikuti newsletter**
```javascript
await sock.newsletterFollow("abcd@newsletter")
```

- **Berhenti mengikuti newsletter**
```javascript
await sock.newsletterUnfollow("abcd@newsletter")
```

- **Mengirim reaksi ke pesan di newsletter**
```javascript
const id = "175"
await sock.newsletterReactMessage("abcd@newsletter", id, "🥳")
```

### Ikon AI

```javascript
// cukup tambahkan "ai: true" pada sendMessage
await sock.sendMessage(id, { text: "Hello World", ai: true })
```

## Broadcast & Status WhatsApp

### Kirim Broadcast dan Status (Stories)

- Kamu bisa kirim pesan ke broadcast & story WhatsApp menggunakan `sendMessage()` seperti biasa, tapi dengan tambahan properti khusus:

```javascript
await sock.sendMessage(
  jid,
  {
    image: {
      url: url
    },
    caption: 'Halo dari broadcast!'
  },
  {
    backgroundColor: '#ffffff', // opsional
    font: 'default', // opsional
    statusJidList: ['628xxx@s.whatsapp.net'], // daftar kontak yang akan terima status
    broadcast: true // aktifkan mode broadcast
  }
)
```

- Konten pesan bisa berupa `extendedTextMessage`, `imageMessage`, `videoMessage`, atau `voiceMessage`.  
  [Lihat semua tipe konten pesan di sini](https://baileys.whiskeysockets.io/types/AnyRegularMessageContent.html)

- Kamu juga bisa menggunakan `backgroundColor`, `font`, dan pengaturan lainnya pada opsi pengiriman.  
  [Lihat semua opsi di sini](https://baileys.whiskeysockets.io/types/MiscMessageGenerationOptions.html)

- ID broadcast biasanya berbentuk: `12345678@broadcast`

### Ambil Info Daftar Broadcast

```javascript
let bList = await sock.getBroadcastListInfo('1234@broadcast')
console.log(`Nama list: ${bList.name}, Penerima: ${bList.recipients}`)
```

## Menulis Fungsionalitas Kustom (Custom Functionality)

Baileys dirancang untuk **ekstensi & kustomisasi**. Kamu tidak perlu fork repo untuk modifikasi — cukup tulis kode kamu sendiri dan panggil lewat API yang disediakan.

### Mengaktifkan Log Debug WhatsApp

- Untuk melihat semua pesan mentah dari WhatsApp, aktifkan logger debug saat inisialisasi soket:

```javascript
import P from 'pino'

const sock = makeWASocket({
  logger: P({ level: 'debug' })
})
```

> Ini sangat berguna kalau kamu ingin memahami **bagaimana WhatsApp bekerja di balik layar** atau mau buat fitur-fitur advance yang gak didokumentasikan.

## Bagaimana WhatsApp Berkomunikasi Dengan Kita

> **TIP:**  
> Kalau kamu ingin mempelajari protokol komunikasi WhatsApp, disarankan untuk memahami tentang **LibSignal Protocol** dan **Noise Protocol**.

### Contoh Kasus

Misalnya, kamu ingin melacak **persentase baterai** dari HP yang terhubung.  
Kalau kamu mengaktifkan log `debug`, maka akan muncul pesan seperti ini di terminal:

```
{
    "level": 10,
    "fromMe": false,
    "frame": {
        "tag": "ib",
        "attrs": {
            "from": "@s.whatsapp.net"
        },
        "content": [
            {
                "tag": "edge_routing",
                "attrs": {},
                "content": [
                    {
                        "tag": "routing_info",
                        "attrs": {},
                        "content": {
                            "type": "Buffer",
                            "data": [8,2,8,5]
                        }
                    }
                ]
            }
        ]
    },
    "msg": "communication"
}
```

### Penjelasan Struktur `frame`

Setiap pesan dari WhatsApp memiliki struktur `frame` dengan komponen utama berikut:

- `tag` — menandakan tipe pesan (contoh: `'message'`)
- `attrs` — objek berisi key-value untuk metadata (biasanya mengandung ID pesan)
- `content` — data utama dari isi pesan (contoh: isi teks dari pesan)

> Untuk dokumentasi lebih lanjut, lihat [struktur WABinary](/src/WABinary/readme.md)

### Daftarkan Callback Untuk Event WebSocket

> **TIP:**  
> Lihat fungsi `onMessageReceived` di file `socket.ts` untuk memahami cara event websocket diproses.

```javascript
// untuk semua pesan dengan tag 'edge_routing'
sock.ws.on('CB:edge_routing', (node) => { })

// untuk pesan dengan tag 'edge_routing' dan atribut id = abcd
sock.ws.on('CB:edge_routing,id:abcd', (node) => { })

// untuk pesan dengan tag 'edge_routing', id = abcd & isi pertama adalah 'routing_info'
sock.ws.on('CB:edge_routing,id:abcd,routing_info', (node) => { })
```