# 🌸 SALWA APP - Real-time Chat (P2P)

Aplikasi chat real-time peer-to-peer untuk SALWA APP menggunakan **WebRTC** dan **PeerJS**.

## ⚡ Fitur

- ✅ **Chat real-time** antar pengguna
- ✅ **Peer-to-peer** (tidak melalui server)
- ✅ **Tidak ada penyimpanan** pesan (ephemeral)
- ✅ **Auto-generate username** unik
- ✅ **Typing indicator** (opsional)
- ✅ **Responsive design** (mobile-friendly)
- ✅ **Google Analytics tracking** untuk event chat

## 🔧 Cara Kerja

### Arsitektur

```
User A <--WebRTC--> PeerJS Cloud Server <--WebRTC--> User B
                      (Signaling Only)
```

- **PeerJS** menyediakan signaling server gratis untuk handshake awal
- Setelah connect, chat langsung **peer-to-peer** (tidak melalui server)
- Pesan **tidak tersimpan** di server manapun
- Setelah user keluar, pesan **hilang permanen**

### Keterbatasan Mode Demo

Karena aplikasi ini **100% statis** (tanpa backend), ada keterbatasan:

1. **Peer Discovery**: Peer tidak bisa otomatis menemukan peer lain tanpa signaling server custom
2. **Demo Mode**: Saat ini menggunakan dummy users untuk demonstrasi UI
3. **Manual Connect**: Untuk chat real, user harus share Peer ID manual

## 🚀 Setup & Deploy

### 1. Update Google Analytics ID

Edit file `index.html`, ganti `G-XXXXXXXXXX` dengan Measurement ID Anda:

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
```

### 2. Upload ke GitHub

```bash
git init
git add .
git commit -m "Add chat feature"
git push -u origin main
```

### 3. Aktifkan GitHub Pages

Settings > Pages > Deploy from main branch

### 4. Akses Aplikasi

- Main app: `https://username.github.io/salwa-app/`
- Admin: `https://username.github.io/salwa-app/admin.html`

## 🎯 Cara Menggunakan Chat

### Mode Demo (Saat Ini)

1. Buka aplikasi
2. Lihat daftar "Pengguna Online" (dummy users untuk demo)
3. Klik nama user untuk simulasi chat
4. Ketik pesan dan kirim

### Mode Production (Perlu Signaling Server)

Untuk chat real antar device:

1. **User A** copy Peer ID-nya (ditampilkan di header)
2. **User A** share ID ke **User B** (via WhatsApp, email, dll.)
3. **User B** paste ID User A di kode (atau buat form input)
4. Connect dan mulai chat!

## 🔒 Keamanan & Privacy

### Kelebihan
- ✅ **End-to-end encryption** (WebRTC DTLS)
- ✅ **No server storage** (pesan tidak disimpan)
- ✅ **Anonymous** (tidak perlu login)

### Kekurangan
- ⚠️ **IP address visible** (karena P2P, IP terlihat oleh peer lain)
- ⚠️ **No message history** (pesan hilang setelah logout)
- ⚠️ **No moderation** (tidak ada filter konten)

## 🛠️ Upgrade ke Production

Untuk aplikasi production dengan chat real, pertimbangkan:

### Opsi 1: Signaling Server Sendiri

Buat server sederhana dengan Node.js + Socket.io untuk peer discovery:

```javascript
// server.js
const io = require('socket.io')(3000);

io.on('connection', socket => {
  socket.on('peer-join', peerId => {
    socket.join(peerId);
  });

  socket.on('signal', (data) => {
    io.to(data.to).emit('signal', {
      from: socket.id,
      signal: data.signal
    });
  });
});
```

### Opsi 2: Backend-as-a-Service

Gunakan Firebase/Supabase untuk:
- User authentication
- Real-time database untuk chat
- Cloud messaging

### Opsi 3: Third-party Chat SDK

Integrasikan dengan:
- **Sendbird** (chat SDK gratis tier)
- **PubNub** (real-time messaging)
- **Stream Chat** (chat API)

## 📊 Analytics Events

Event yang dilacak di Google Analytics:

| Event Name | Category | Label | Description |
|------------|----------|-------|-------------|
| `chat_connected` | Chat | PeerJS Connected | User berhasil connect ke PeerJS |
| `chat_started` | Chat | Username | User mulai chat dengan peer lain |
| `chat_message_sent` | Chat | - | User mengirim pesan |
| `chat_error` | Chat | Error Type | Error yang terjadi |

## 🎨 Custom Styling

Edit bagian `<style>` di `index.html`:

- Ganti gradient warna di `body`
- Ubah ukuran font, padding
- Tambah emoji atau icon baru

## 📝 File Structure

```
salwa_app_chat/
├── index.html      # Main app dengan chat UI
├── chat.js         # WebRTC P2P chat logic
├── admin.html      # Admin dashboard (placeholder)
└── README.md       # Dokumentasi
```

## 🐛 Troubleshooting

### "Connecting..." terus menerus
- Cek koneksi internet
- PeerJS server mungkin down (coba refresh)
- Browser block WebRTC (cek permission)

### Tidak bisa connect ke peer lain
- Pastikan kedua peer sudah dapat ID dari PeerJS
- Cek firewall tidak block WebRTC
- Untuk demo, gunakan dummy users yang tersedia

### Chat tidak muncul
- Buka browser console (F12) untuk lihat error
- Pastikan JavaScript enabled
- Clear cache dan reload

## 📞 Support

- [PeerJS Documentation](https://peerjs.com/docs/)
- [WebRTC Basics](https://webrtc.org/getting-started/overview)
- [Google Analytics for Firebase](https://firebase.google.com/docs/analytics)

---

**Dibuat dengan ❤️ untuk SALWA APP**

*Chat real-time tanpa server, pesan tidak tersimpan!*
