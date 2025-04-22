Tentu! Berikut adalah dokumentasi lengkap untuk proyek **Integrasi Webhook dengan n8n dan Flask** yang siap kamu gunakan di GitHub. Dokumentasi ini akan mencakup semua langkah dari setup hingga cara menjalankan dan testing.

---

## 📄 **README.md**

```markdown
# 🌐 Integrasi Webhook dengan n8n + Flask

Proyek ini adalah contoh integrasi antara **n8n**, **Flask**, dan **ngrok**. Sistem ini mengirim data melalui webhook ke n8n dan menerima callback yang diproses oleh Flask.

---

## 🎯 Tujuan

- Mengirim payload dari client ke webhook n8n.
- n8n memproses data dan mengirim balasan ke URL callback yang diatur oleh Flask.
- Flask menerima callback dan merespons dengan data yang diproses.

---

## 🛠️ **Tools yang Digunakan**

- **Python**: Bahasa pemrograman untuk server.
- **Flask**: Framework untuk membuat API server.
- **ngrok**: Digunakan untuk membuat tunnel ke server lokal (Flask) agar dapat diakses dari luar jaringan.
- **n8n**: Platform otomasi yang akan menerima webhook dan mengirimkan callback.
- **Postman**: Untuk testing API.

---

## 🚀 **Cara Menjalankan Proyek**

### 1. **Clone Repository**

```bash
git clone https://github.com/rizqif123/n8n-flask-webhook.git
cd n8n-flask-webhook
```

### 2. **Install Flask**

Instal **Flask** di sistem kamu:

```bash
pip install flask
```

### 3. **Menjalankan Server Flask**

Setelah dependensi terinstal, jalankan server Flask:

```bash
python app.py
```

Flask akan berjalan di `http://localhost:5000`.

### 4. **Jalankan ngrok**

Buka terminal baru dan jalankan **ngrok** untuk membuat tunnel ke server Flask:

```bash
ngrok http 5000
```

ngrok akan memberikan URL seperti:
```
https://ff74-103-175-236-31.ngrok-free.app/callback
```
Salin URL ini karena akan digunakan sebagai URL callback untuk n8n.

---

## 🔧 **Konfigurasi Workflow di n8n**

1. Masuk ke [n8n Avataralabs](https://n8n.avataralabs.ai).
2. Buat **Webhook node**:
   - Path: `/test-webhook`
   - Method: `POST`
3. Tambahkan **HTTP Request node**:
   - Method: `POST`
   - URL: `{{$json["callback"]}}` (URL callback dari ngrok)
   - Content Type: `JSON`
   - Body Parameters:
     - Key: `message`
     - Value: `Hello again!`
4. Hubungkan **Webhook** → **HTTP Request**
5. Klik tombol **Activate** (pojok kanan atas)

---

## 🧪 **Testing via Postman**

Untuk mengirimkan data ke webhook n8n, gunakan **Postman** atau alat lainnya.

### Konfigurasi Postman:

- **URL**:
  ```
  https://n8n.avataralabs.ai/webhook/test-webhook
  ```
- **Method**: `POST`
- **Headers**:
  ```
  Content-Type: application/json
  ```
- **Body (JSON)**:
  ```json
  {
    "message": "Halo, n8n",
    "callback": "https://xxxxxx.ngrok-free.app/callback"
  }
  ```

Klik **Send** untuk mengirimkan data ke n8n.

---

## 📂 **Struktur Folder**

```
n8n-flask-webhook/
├── app.py                # Server Flask
├── README.md             # Dokumentasi ini
└── requirements.txt      # Daftar dependensi (opsional)
```

---

## 🧰 **Contoh Kode app.py (Flask)**

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route('/callback', methods=['GET', 'POST'])
def callback():
    if request.method == 'GET':
        return "Endpoint /callback aktif!", 200

    data = request.get_json()
    print("Callback received:", data)
    return jsonify({"message": "Halo Lagi!"}), 200

if __name__ == "__main__":
    app.run(port=5000)
```

---

## 📑 **requirements.txt**

Untuk memudahkan orang lain dalam menginstal dependensi, buatlah file `requirements.txt`. Isinya adalah sebagai berikut:

```
flask
```

---

## 📌 **Catatan Tambahan**

- Pastikan workflow di **n8n** diaktifkan (toggle ON).
- **ngrok** harus berjalan agar server Flask dapat diakses secara publik.
- Endpoint Flask `/callback` hanya menerima request **POST** untuk webhook.

---

## 🔄 **Proses Callback**

Saat kamu mengirimkan data ke **n8n**, n8n akan mengirimkan request POST ke **callback URL** yang telah ditentukan. Server Flask akan mencetak payload yang diterima dan mengembalikan response JSON:

```json
{
  "message": "Halo Lagi!"
}
```
