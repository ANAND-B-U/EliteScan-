# 📇 EliteScan - Business Card OCR System

A **premium**, **production‑ready** business card OCR application with advanced AI-powered text extraction, professional UI with gold dazzling effects, and comprehensive data export capabilities.

**🌟 Professional Business Card OCR Solution**

---

## ✨ Features

### 🤖 AI-Powered OCR
- ✅ **Multi‑Model Support**: NVIDIA Phi‑3.5 Vision, Mistral Large (vision), Google Gemini 2.5 Flash  
- 🔁 **Smart Fallback**: Auto‑retries with alternative models if one fails  
- 🌍 **Global Phone Normalization**: Uses `phonenumbers` (Google libphonenumber) for international formats  
- 🧹 **Clean Output**: Omits null/empty fields — only returns what's present  
- 🛡️ **Robust Error Handling**: Detects quota limits, auth failures, and JSON parsing errors  

### 🎨 Premium Frontend
- ✅ **Gold Dazzling Effects**: Animated gold shimmer, glow, and pulse effects
- ✅ **Glass Morphism**: Modern glass-like card designs with backdrop blur
- ✅ **Professional Theme**: Black, gold, and purple color scheme
- ✅ **Enhanced Notifications**: Premium success popups with progress bars
- ✅ **Responsive Design**: Works on all screen sizes

### � Advanced Features
- � **Batch & Single Processing**: Upload one or many cards at once  
- 📊 **Data Export**: CSV and Excel download with professional formatting
- 📋 **Detailed Logging**: Per‑model attempts, failures, and fallbacks  
- 🔐 **Secure**: Safe temp file handling, max upload size, no data leakage
- 📁 **PDF Support**: Process PDF files alongside images

---

## 🚀 Quick Start

### 1️⃣ Install Dependencies

```bash
pip install -r requirements.txt
```

### 2️⃣ Set Up API Keys

Create a `.env` file in the project root:

```env
GEMINI_API_KEY=your_gemini_key
NVIDIA_API_KEY=nvapi-your_nvidia_key_from_build_nvidia_com
MISTRAL_API_KEY=your_mistral_key_from_console_mistral_ai
```

### 3️⃣ Run the Server

```bash
python backend.py
```

Server starts at:

```text
http://localhost:5000
```

### 4️⃣ Launch Premium Frontend

```bash
# Open the premium frontend in your browser
open frontend/index.html
```

Or serve with a web server:

```bash
# Using Python
cd frontend
python -m http.server 8000

# Then visit http://localhost:8000
```

---

## API Endpoints

### Health Check

```http
GET /health
```

Returns service status and available models.

**Example response:**

```json
{
  "status": "healthy",
  "models": ["nvidia", "mistral", "gemini"],
  "endpoints": ["/api/single", "/api/batch"]
}
```

---

### 🖼️ Single Card Extraction

```http
POST /api/single
```

**Form‑Data:**

- `image` (file, required): Business card image (`.jpg`, `.jpeg`, `.png`)
- `model` (text, optional): one of `auto` (default), `nvidia`, `mistral`, `gemini`

**Success response example:**

```json
{
  "success": true,
  "data": {
        "name": "Legend A",
        "title": "CEO",
        "company": "Alpha Pvt. Ltd.",
        "phoneNumbers": ["+91 xxxxxxxxx4"],
        "email": "xxxxxxxxxx@alpha.com",
        "website": "www.alpha******.com",
        "model": "nvidia""
  },
  "model_used": "nvidia",
  "filename": "card.jpg"
}
```

---

### 📁 Batch Extraction

```http
POST /api/batch
```

**Form‑Data:**

- `images` (multiple files, required): Business card images
- `model` (text, optional): same options as `/api/single`

**Response example:**

```json
{
  "success": true,
  "total": 2,
  "results": [
    {
      "filename": "card1.jpg",
      "success": true,
      "data": {
        "name": "Legend A",
        "title": "CEO",
        "company": "Alpha Pvt. Ltd.",
        "phoneNumbers": ["+91 xxxxxxxxx4"],
        "email": "xxxxxxxxxx@alpha.com",
        "website": "www.alpha******.com",
        "model": "nvidia"
      },
      "model_used": "nvidia",
      "error": null
    },
    {
      "filename": "card2.jpg",
      "success": false,
      "data": null,
      "model_used": "gemini_quota_exceeded",
      "error": "Gemini quota exceeded and other models also failed"
    }
  ]
}
```

---

## 🔧 Model Behavior & Fallback

**Model selection behavior:**

| `model` value | Fallback order                     |
| ------------- | ---------------------------------- |
| `auto`        | NVIDIA → Mistral → Gemini          |
| `nvidia`      | NVIDIA → Mistral → Gemini          |
| `mistral`     | Mistral → NVIDIA → Gemini          |
| `gemini`      | Gemini → NVIDIA → Mistral          |

If the primary model fails (e.g., quota exceeded, auth error, bad JSON), the API automatically tries the next one in the order until one succeeds or all fail.

---

## � Project Structure

```
EliteScan/
├── backend.py                          # Main backend application
├── README.md                           # Main documentation
├── requirements.txt                    # Python dependencies
├── .env.example                        # Environment variables template
├── .env                               # Environment variables (create from .env.example)
├── .gitignore                         # Git ignore file
└── frontend/                          # Premium frontend application
    ├── index.html                      # Main frontend HTML
    ├── styles.css                      # Premium styling with gold effects
    ├── script.js                       # Frontend JavaScript logic
    └── README.md                       # Frontend documentation
```

## 📝 Output Schema

| Field        | Type              | Example                          |
| ------------ | ----------------- | -------------------------------- |
| `name`       | string            | "Legend A"               |
| `title`      | string            | "CEO"          |
| `company`    | string            | "Alpha Pvt. Ltd."   |
| `address`    | string            | "Chennai, Tamil Nadu, India"   |
| `phoneNumbers` | array of string | ["+91*****4"]            |
| `email`      | string            | "alpha***********.com"         |
| `website`    | string            | "www.alpha******.com"           |
| `tokens`     | integer           | 1020                           |
| `model`      | string            | "nvidia"                       |

- ❌ No `null` values  
- ❌ No empty strings  
- ❌ No empty arrays  

The API cleans the output to keep the JSON compact and meaningful.

---

## 📝 website realtime images

*This is the overall window*


<img width="1687" height="837" alt="Screenshot 2026-06-25 145702" src="https://github.com/user-attachments/assets/46a07449-616f-409d-803f-6af561751c8b" />


*This is model selection filter*


<img width="712" height="262" alt="Screenshot 2026-06-25 153810" src="https://github.com/user-attachments/assets/46a36fc6-ec0c-4282-a2c2-5747f459d03c" />


*This is processing mode selection filter*


<img width="701" height="167" alt="Screenshot 2026-06-25 153827" src="https://github.com/user-attachments/assets/6d50b31c-6bfd-49d5-837b-2b35eb72edac" />


*This is the processing Queue*


<img width="1770" height="300" alt="Screenshot 2026-06-25 153903" src="https://github.com/user-attachments/assets/798bc494-73a7-42ab-9c27-8837c947aaf3" />



## 🛠️ Common Errors & Fixes

| Error message                                           | Cause                          | Fix                                                                 |
| ------------------------------------------------------- | ----------------------------- | ------------------------------------------------------------------- |
| Authorization failed                                  | Invalid/missing API key       | Verify keys in `.env`; ensure each provider key has correct access. |
| Gemini quota exceeded and other models also failed    | Gemini free tier exhausted    | Wait for daily reset or upgrade plan; ensure fallbacks are enabled. |
| Extraction failed with all models                     | All models errored            | Check logs; verify keys, quotas, and image quality/size (<50 MB).   |
| No image provided                                     | Missing file upload           | In Postman, set `image` as File type, not Text.                 |

---

## 🧪 Testing with Postman

1. Method: `POST`  
2. URL: `http://localhost:5000/api/single`  
3. Body → `form-data`:
   - Key: `image` → Type: **File** → Select a card image
   - Key: `model` → Type: **Text** → Value: `mistral` (or `auto`, `nvidia`, `gemini`)
4. Click **Send** and inspect the JSON response.

---

## 📜 License

MIT License — free to use, modify, and deploy.

---

💡 Built for reliability — whether you're processing 1 card or 10,000, EliteScan handles errors gracefully and delivers clean, structured contact data.

Happy parsing with EliteScan! 🚀
