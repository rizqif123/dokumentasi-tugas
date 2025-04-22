# dokumentasi-tugas


---

## 📄 `README.md`

```markdown
# 🌐 Integrasi Webhook dengan n8n + Flask

Proyek ini menunjukkan integrasi webhook menggunakan [n8n](https://n8n.io/) dan Python Flask, dengan callback ke server lokal yang di-expose menggunakan [ngrok](https://ngrok.com/).

---

## 🎯 Tujuan

- Mengirim payload dari client ke webhook n8n.
- n8n memproses data dan mengirim balik ke URL callback.
- Flask menerima callback dan mencatat respons.

---

## 🛠️ Tools yang Digunakan

- Python + Flask
- ngrok
- n8n (melalui Avataralabs)
- Postman (untuk testing)

---

## 🚀 Cara Menjalankan

### 1. Clone Repository

```bash
git clone https://github.com/rizqif123/n8n-flask-webhook.git
cd n8n-flask-webhook
```

### 2. Install Flask

```bash
pip install flask
```

### 3. Jalankan Server Flask

```bash
python app.py
```

Server akan berjalan di `http://localhost:5000`.

### 4. Jalankan ngrok

```bash
ngrok http 5000
```

Salin URL HTTPS dari ngrok, misalnya:
```
https://xxxxxx.ngrok-free.app
```

---

## 🔧 Konfigurasi Workflow di n8n

1. Masuk ke [https://n8n.avataralabs.ai](https://n8n.avataralabs.ai).
2. Buat **Webhook node**:
   - Path: `/test-webhook`
   - Method: `POST`
3. Tambahkan **HTTP Request node**:
   - Method: `POST`
   - URL: `{{$json["callback"]}}`
   - Content Type: `JSON`
   - Body Parameters:
     - Key: `message`
     - Value: `Hello again!`
4. Hubungkan Webhook → HTTP Request
5. Klik tombol **Activate** (pojok kanan atas)

---

## 🧪 Testing via Postman

- URL:
  ```
  https://n8n.avataralabs.ai/webhook/test-webhook
  ```
- Method: `POST`
- Headers:
  ```
  Content-Type: application/json
  ```
- Body (JSON):
  ```json
  {
    "message": "Halo, n8n",
    "callback": "https://xxxxxx.ngrok-free.app/callback"
  }
  ```

### ✅ Hasil:
- n8n mengirim `POST` ke endpoint callback Flask kamu.
- Flask akan mencetak:
  ```
  Callback received: {'message': 'Hello again!'}
  ```

---

## 📁 Struktur Proyek

```
n8n-flask-webhook/
├── app.py          # Server Flask
├── README.md       # Dokumentasi
└── requirements.txt (opsional)
```

---

## 📄 Contoh app.py

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

## 📌 Catatan

- Workflow di n8n harus diaktifkan.
- Endpoint callback Flask harus terbuka melalui ngrok.
- Pastikan ngrok dan Flask berjalan bersamaan selama testing.

---

## ✍️ Author

Dibuat oleh [@username](https://github.com/username)  
Untuk keperluan pengujian webhook dengan n8n 🚀
```

---
