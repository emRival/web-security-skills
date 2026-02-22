---
description: Apply file upload security — extension/MIME validation, content scanning, size limits, and image verification.
---

# 📁 Security — File Upload Protection

## What to Implement

### 1. Validation Chain (in order)
```
1. File exists and is valid (not corrupted)
2. File size ≤ max limit (default 5MB)
3. Extension NOT in blocked list
4. Extension IS in allowed list
5. MIME type matches allowed list
6. **Magic Bytes (File Signature) match the expected type**
7. No double extension (image.php.jpg)
7. No script code inside file content
8. If image: verify with image library
```

### 2. Blocked Extensions
```
.php, .phtml, .php3, .php4, .php5, .phps,
.exe, .bat, .cmd, .com, .pif, .msi,
.sh, .bash, .zsh, .csh,
.js, .jsx, .ts, .tsx, .py, .pl, .rb, .java, .jar, .war,
.asp, .aspx, .jsp, .cgi,
.htaccess, .htpasswd, .env, .config, .ini, .yml, .yaml
```

### 3. Allowed Extensions (whitelist)
```
Images: .jpg, .jpeg, .png, .gif, .webp, .svg, .bmp
Docs: .pdf, .doc, .docx, .xls, .xlsx, .ppt, .pptx
Media: .mp4, .mp3, .wav, .ogg, .webm
```

### 4. Content Scanning
Scan raw file bytes for embedded code:
```
<?php, <?=, <?, <%           (PHP/ASP tags)
<script                       (JavaScript)
#!/bin/                       (Shell scripts)
import os; import subprocess  (Python)
```

### 5. Magic Bytes (File Signature) Verification
Attackers often rename `shell.php` to `shell.pdf` to bypass extension checks. You must verify the **first few bytes** of the file to ensure the content matches the extension.
- **PDF**: The first 4 bytes must be `%PDF` (Hex: `25 50 44 46`)
- **JPEG**: The first 3 bytes must be `FF D8 FF`
- **PNG**: The first 8 bytes must be `89 50 4E 47 0D 0A 1A 0A`
- **ZIP/DOCX**: The first 4 bytes must be `PK\x03\x04` (Hex: `50 4B 03 04`)
- *Action*: Reject the file if the magic bytes don't match the required signature.

### 5. Image Verification
For image uploads, verify the file IS a real image:
- Use `getimagesize()` (PHP), `sharp` (Node), `Pillow` (Python)
- Invalid image data = reject file

### 6. Storage Security
- NEVER store uploads in web-accessible directories
- Use random filenames: `uuid.ext` instead of original name
- Store outside `public/` and serve via controller with auth check
- Set `X-Content-Type-Options: nosniff` on served files

## Implementation by Tech

| Tech | Middleware | Magic Bytes / Signature Check | Storage |
|------|-----------|-------------------------------|---------|
| **Laravel** | Custom middleware + `UploadedFile` | `file` CLI / `finfo_open()` / validation rules | `Storage::disk('local')` |
| **Express.js** | `multer` + custom validator | `file-type` npm package | S3 or private dir |
| **Next.js** | API route validation | `file-type` npm package | Vercel Blob / S3 |
| **Django** | Custom validator on `FileField` | `python-magic` package | `MEDIA_ROOT` (private) |
| **FastAPI** | `UploadFile` + `Depends()` | `python-magic` package | Local dir / S3 |
