# Binary Beasts: AI Phishing Detector

An AI-assisted web application developed for the **GDG Hackathon (Hack2Skill)** to analyze suspicious text messages, URLs, and screenshots for phishing and social-engineering indicators using Node.js, Express, Tesseract.js OCR, and external AI APIs.

> **Live Demo (Hosted on Render):** [https://binary-beasts-imqc.onrender.com](https://binary-beasts-imqc.onrender.com)  
> *Availability depends on Render hosting state and configured third-party API quotas.*

---

## Overview

Phishing attacks frequently use newly created domains, deceptive wording, or text embedded inside images to evade basic keyword filters. This project provides a web interface where users can submit suspicious text, URLs, or screenshots to receive an automated risk score (`0–100`) and an explanation of potential social-engineering cues.

## Key Features

- **Text & URL Analysis:** Evaluates submitted messages and links for phishing patterns, urgency tactics, and suspicious URL indicators.
- **Screenshot OCR Processing:** Extracts embedded text from uploaded images using `Tesseract.js` before passing the extracted content to the analysis pipeline.
- **Multi-Provider AI Routing:** Uses Google Gemini (2.5 Flash) with fallback routing to external LLM providers (such as OpenRouter / DeepSeek-R1 and Meta Llama 3.1, depending on configured API keys in `server.js`).
- **Threat Scoring Output:** Returns a `0–100` risk score and human-readable reasoning in the web UI.

## Architecture & Tech Stack

- **Backend:** Node.js, Express.js, Multer (image upload handling)
- **OCR Engine:** Tesseract.js (`eng.traineddata`)
- **AI & External Services:** Google Gemini API with external LLM fallback *(verify exact active providers, API key rotation, rate-limiting middleware, or Google Web Risk checks in `server.js`)*
- **Frontend:** HTML5, Tailwind CSS, vanilla JavaScript, Lucide Icons (`public/`)

### Request Flow

1. The user submits text, a URL, or an image through the frontend (`public/`).
2. `server.js` receives the request (handling image uploads via Multer).
3. If an image is uploaded, Tesseract.js extracts text using `eng.traineddata`.
4. The backend sends the text/URL prompt to the configured external AI service (falling back to secondary providers if configured and primary calls fail).
5. The structured risk score (`0–100`) and explanation are returned to the browser.

## Project Structure

```text
AI-PHISING-DETECTION-zero-day-/
├── public/              # Frontend HTML, Tailwind CSS, and client-side JavaScript
├── server.js            # Express backend, Multer uploads, OCR, and AI API routing
├── eng.traineddata      # Tesseract.js English language trained data
├── package.json         # Dependencies and npm scripts
├── package-lock.json    # Dependency lockfile
├── .gitignore
└── README.md
```

## Installation & Local Setup

### Prerequisites
- Node.js (LTS recommended) and `npm`
- Valid API key(s) for the external AI provider(s) referenced in `server.js`

### Steps

1. **Clone the repository:**
   ```bash
   git clone https://github.com/knockout23486/AI-PHISING-DETECTION-zero-day-.git
   cd AI-PHISING-DETECTION-zero-day-
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Configure environment variables:**
   Create a `.env` file in the project root matching the environment variable names read in `server.js` (for example, `GEMINI_API_KEY`, `OPENROUTER_API_KEY`, `PORT`, etc.—check `process.env` references inside `server.js` and never commit `.env` to Git).

4. **Start the server:**
   ```bash
   npm start
   ```
   *(Or run `node server.js` if no `start` script is defined in `package.json`.)*

5. Open the local URL printed in your terminal (typically `http://localhost:3000`).

## Security Limitations

- **Probabilistic Detection:** AI-generated risk scores can produce **false positives** (flagging legitimate messages) and **false negatives** (missing sophisticated phishing attacks). This tool does not guarantee detection of zero-day threats.
- **External Service Dependency:** Analysis relies on third-party AI APIs, hosting infrastructure, provider rate limits, and network connectivity. Outages, quota exhaustion, or cold starts can delay or fail requests.
- **No Sandboxed Execution:** The application inspects text, OCR output, and URL strings; it does not detonate links or attachments in an isolated browser sandbox.
- **OCR Accuracy:** Low-resolution, stylized, or obfuscated screenshots may produce incomplete OCR text, reducing analysis quality.

## Responsible & Ethical Usage

- This project is intended strictly for **educational exploration, defensive awareness, and hackathon prototyping**.
- **Do not upload sensitive personal data**, passwords, financial details, private IDs, or confidential corporate documents—submitted content is transmitted to external AI APIs for analysis.
- Never rely on this prototype as a replacement for enterprise email security, browser Safe Browsing controls, or security operations center (SOC) tooling.

## Contributing

Bug reports, documentation refinements, and defensive improvements are welcome. Please open an issue describing your proposed change before submitting a pull request.
