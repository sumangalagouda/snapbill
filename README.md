# SnapBill (simple local version)

Sign up / log in -> upload a receipt photo -> text is read in your browser (Tesseract.js) -> you check the
values -> expense is saved. **No AWS or any cloud service.**

- Backend: Flask + SQLite (`snapbill.db`)
- Receipt images: saved in the local `uploads/` folder
- Login: email + password (hashed), cookie session

## AI assistant (OpenAI or Gemini)
The "Ask AI about your spending" box answers questions like *"How much did I spend on food this month?"*
using only **your saved expenses**. Totals by category/month are calculated in Python and sent to the model together
with your recent expenses, so the numbers are exact.

1. Choose a provider and create its API key:
   - OpenAI: https://platform.openai.com/api-keys
   - Google AI Studio: https://aistudio.google.com/apikey
2. Open the `.env` file and configure a matching key and model:
   ```
   AI_API_KEY=your-provider-api-key
   OPENAI_MODEL=gpt-4o-mini
   ```
   For Gemini, use `OPENAI_MODEL=gemini-3.8-flash` with the Google AI Studio key.
   The app automatically uses Google's OpenAI-compatible endpoint for `gemini-*` models.
3. Restart the app.

The key is read on the server only and is never sent to the browser. `.env` is listed in `.gitignore`,
so do not upload it anywhere or share it. Each question sends your expense data to the configured AI provider.

## Run
```bash
pip install -r requirements.txt
python app.py
```
Open http://127.0.0.1:5000

Optional: set your monthly budget (default 5000):
```bash
MONTHLY_BUDGET=8000 python app.py        # Windows (PowerShell): $env:MONTHLY_BUDGET=8000; python app.py
```

Optional local AI: install Ollama, run `ollama pull llama3.2`, and start it with
`OLLAMA_ORIGINS=http://127.0.0.1:5000 ollama serve`, then tick the Ollama checkbox in the app.

Note: the Tesseract.js library is loaded from a CDN, so the browser needs internet the first time it scans.

Reset everything: stop the app and delete `snapbill.db`, `uploads/` and `.secret_key` (keep `.env`).
