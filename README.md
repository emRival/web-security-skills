# 🛡️ Security Workflows (Skills) for Antigravity

Repositori ini berisi kumpulan **Workflows** (atau **Skills**) keamanan (security) yang dirancang khusus untuk digunakan bersama *AI code editor* seperti **Antigravity**. Workflow ini berfungsi sebagai panduan dan instruksi otomatis bagi AI untuk menerapkan lapisan keamanan standar pada aplikasi web Anda.

## 🚀 Cara Menggunakan di Antigravity

Untuk menggunakan workflow/skills ini di proyek Anda melalui editor Antigravity, ikuti langkah-langkah berikut:

### 1. Integrasi ke Proyek Anda
Salin folder `workflows` dari repositori ini ke dalam direktori proyek Anda. Ganti nama folder induknya menjadi salah satu format yang didukung oleh Antigravity (misalnya `.agents` atau `.agent`).

Struktur direktori proyek Anda akan terlihat seperti ini:
```text
my-project/
├── .agents/
│   └── workflows/
│       ├── security.md
│       ├── security-auth.md
│       ├── security-injection.md
│       └── ...
├── src/
├── package.json
└── ...
```

### 2. Memanggil Skill / Workflow
Setelah file berada di direktori yang tepat, Anda bisa langsung memanggilnya di dalam *chat prompt* Antigravity menggunakan perintah *slash command* yang sesuai dengan nama file markdown (tanpa ekstensi `.md`).

Contoh penggunaan di chat Antigravity:
- **"Tolong terapkan `@/security` pada proyek ini."**
- **"Jalankan `@/security-auth` untuk memperbaiki sistem login."**
- Atau cukup ketik `/security` (jika didukung sebagai native slash command) dan Antigravity akan membaca instruksi dari `security.md`.

*Catatan: Antigravity secara otomatis akan mencari file di `.agents/workflows/{command}.md` dan menjalankan instruksi spesifik yang ada di dalamnya.*

### 3. Fitur Auto-Run (Turbo)
Jika di dalam workflow terdapat anotasi `// turbo` atau `// turbo-all`, bagian perintah terminal (*run_command*) akan dieksekusi secara otomatis oleh agen AI tanpa menunggu persetujuan (approval) berulang dari pengguna, selama dianggap aman.

---

## 📂 Daftar Skills / Workflows yang Tersedia

Berikut adalah daftar sub-workflow keamanan yang bisa Anda jalankan secara terpisah atau bersamaan:

1. **`/security`** - **Master Workflow**: Menjalankan deteksi *tech stack* dan mengaplikasikan semua sub-workflow keamanan di bawah ini ke seluruh proyek.
2. **`/security-injection`** - Mencegah *SQL Injection*, *Command Injection*, dan *XSS (Cross-Site Scripting)*.
3. **`/security-auth`** - Mengamankan otentikasi (mencegah *Brute Force*, menerapkan 2FA, kebijakan *password*, dan proteksi CSRF).
4. **`/security-session`** - Mencegah *Session Hijacking*, menetapkan *Role-Based Access Control (RBAC)*, mengamankan IDOR, dan konfigurasi *cookie flags*.
5. **`/security-upload`** - Mengamankan fitur unggah file (validasi ekstensi, MIME, *malware scan*, dan verifikasi **Magic Bytes**).
6. **`/security-headers`** - Menerapkan HTTP *Security Headers* dan *Content Security Policy (CSP)* untuk melindungi dari serangan sisi klien.
7. **`/security-infra`** - Menerapkan HTTPS, mengatur CORS secara ketat, *trusted hosts*, dan konfigurasi *proxy*.
8. **`/security-crypto`** - Mengelola kriptografi (enkripsi *field* data PII, *password hashing*, *HashIDs*, proteksi *secrets*, dan *Asymmetric Payload Encryption / E2EE*).
9. **`/security-monitoring`** - Menambahkan *Activity Logging*, sistem *alert* keamanan, dan notifikasi webhook (mis. ke Slack/Discord).
10. **`/security-checklist`** - *Checklist* validasi kesiapan produksi (*production readiness*) untuk memastikan tidak ada celah keamanan yang terlewat.

---

## 💡 Tips Tambahan

- **Kustomisasi**: Anda bisa membuka file markdown di dalam folder `workflows` dan menyesuaikan perintah atau deteksi framework sesuai dengan bahasa pemrograman / framework yang spesifik Anda gunakan (misalnya Laravel, Next.js, Django, Go).
- **Proaktif**: AI Agent seperti Antigravity akan membaca YAML metadata (`description`) pada bagian atas setiap file `.md` untuk memahami kapan dan bagaimana skill tersebut sebaiknya digunakan, bahkan ketika Anda tidak memintanya secara eksplisit.
