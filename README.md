# AngelOne Candle Fetcher — Flask Web App

A web application to fetch OHLCV candle data from AngelOne SmartAPI
and download it as a formatted Excel (.xlsx) file.

---

## 🚀 Quick Deploy Options

### Option 1 — Ubuntu VPS (DigitalOcean / AWS / Hetzner)

```bash
# 1. Clone / upload files to your server
# 2. Install Python deps
pip install -r requirements.txt

# 3. Run with gunicorn (production)
gunicorn -w 4 -b 0.0.0.0:5000 app:app

# 4. (Optional) Run on port 80 with nginx reverse proxy
```

### Option 2 — Railway.app (free tier, easiest)

```bash
# 1. Push code to a GitHub repo
# 2. Go to railway.app → New Project → Deploy from GitHub
# 3. Set start command: gunicorn -w 4 -b 0.0.0.0:$PORT app:app
# 4. Railway auto-detects requirements.txt and installs deps
```

### Option 3 — Render.com (free tier)

```
1. Create account at render.com
2. New → Web Service → connect GitHub repo
3. Build Command:  pip install -r requirements.txt
4. Start Command:  gunicorn -w 4 -b 0.0.0.0:$PORT app:app
5. Deploy!
```

### Option 4 — Local (dev only, no TOTP issues on same network)

```bash
pip install -r requirements.txt
python app.py
# Open http://localhost:5000
```

---

## 📁 Project Structure

```
angleone_app/
├── app.py               # Flask backend + AngelOne API logic
├── requirements.txt     # Python dependencies
├── README.md
└── templates/
    └── index.html       # Frontend UI
```

---

## 🔑 Getting Your Credentials

| Field | Where to find it |
|-------|-----------------|
| API Key | AngelOne → My Profile → API Access → Create App |
| Client ID | Your AngelOne login ID (e.g. A12345) |
| MPIN | Your 4-digit AngelOne MPIN |
| TOTP Secret | AngelOne mobile app → Profile → TOTP Setup → Base32 key |
| Symbol Token | [ScripMaster JSON](https://margincalculator.angelbroking.com/OpenAPI_File/files/OpenAPIScripMaster.json) → find your stock's `token` field |

---

## 📊 Excel Output

The downloaded file contains two sheets:

- **Candles** — Timestamp, Open, High, Low, Close, Volume (formatted, alternating rows)
- **Summary** — Key stats: highest high, lowest low, avg close, total volume

---

## ⚠️ Notes

- Intraday data is only available within market hours: **09:15 – 15:30 IST**
- AngelOne limits each API call to ~60 days of intraday data; the app handles chunking automatically
- TOTP refreshes every 30 seconds — the server handles generation automatically via `pyotp`
