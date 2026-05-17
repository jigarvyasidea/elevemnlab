# LiveKit Vobiz Outbound Agent 📞

A production-ready voice agent capable of making outbound calls using **LiveKit**, **Deepgram**, and **Groq (Llama 3.3)**.  
Designed for reliability, speed, and ease of deployment.

## 🚀 Features
- **Ultra-Fast LLM**: Uses **Groq** running `llama-3.3-70b-versatile` for near-instant responses.
- **Multiple TTS Options**: Supports **Eleven Labs**, **Deepgram**, **OpenAI**, **Sarvam**, and **Cartesia** for natural voice output.
- **High-Quality STT**: Uses **Deepgram** for accurate Speech-to-Text transcription.
- **SIP Trunking**: Integrated with **Vobiz** for PSTN connectivity.
- **Robust Configuration**: Centralized `config.py` for easy customization of prompts, models, and voices.

---

## 🛠️ Setup & Installation

### 1. Prerequisites
- Python 3.10+ (Recommended: 3.10.13)
- A [LiveKit Cloud](https://cloud.livekit.io/) account
- A [Deepgram](https://deepgram.com/) API Key
- A [Groq](https://groq.com/) API Key
- An [Eleven Labs](https://elevenlabs.io/) API Key (optional, for TTS)
- A SIP Provider (e.g., Vobiz)

### 2. Clone & Install
```bash
# Clone the repository
git clone <your-repo-url>
cd LiveKit-Vobiz-Outbound-main

# Create a virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### 3. Configure Environment
Copy the example environment file and fill in your credentials:
```bash
cp .env.example .env
nano .env  # Or open in your editor
```
**Required Variables:**
- `LIVEKIT_URL`, `LIVEKIT_API_KEY`, `LIVEKIT_API_SECRET`
- `DEEPGRAM_API_KEY`
- `GROQ_API_KEY` or `OPENAI_API_KEY` (for LLM)
- `ELEVENLABS_API_KEY` (for Eleven Labs TTS - optional)
- `VOBIZ_SIP_*` variables (for outbound calls)

**TTS Provider Options:**
Set `TTS_PROVIDER` to one of: `elevenlabs`, `deepgram`, `openai`, `sarvam`, or `cartesia`

---

## 🏃‍♂️ Usage

### 1. Start the Agent
This runs the agent process which listens for room connections.
```bash
python agent.py start
```

### 2. Make an Outbound Call
In a **new terminal window** (ensure `venv` is active), run:
```bash
python make_call.py --to +91XXXXXXXXXX
```
*Note: The number must include the country code (e.g., +1 or +91).*

---

## 🎤 TTS Provider Configuration

### Eleven Labs (Recommended)
Set in `.env`:
```
ELEVENLABS_API_KEY=your_api_key
TTS_PROVIDER=elevenlabs
ELEVENLABS_VOICE_ID=Rachel  # Options: Rachel, Chris, Nova, Bella, Sophia, Oliver, etc.
ELEVENLABS_MODEL=eleven_monolingual_v1  # or eleven_multilingual_v2
```

### OpenAI
Set in `.env`:
```
TTS_PROVIDER=openai
OPENAI_TTS_VOICE=alloy  # Options: alloy, echo, shimmer, nova, onyx
```

### Deepgram
Set in `.env`:
```
TTS_PROVIDER=deepgram
DEEPGRAM_TTS_MODEL=aura-asteria-en  # High-quality aura models
```

### Sarvam (Indian Voices)
Set in `.env`:
```
TTS_PROVIDER=sarvam
SARVAM_VOICE=anushka  # Options: anushka, aravind, amartya, dhruv
SARVAM_LANGUAGE=en-IN  # or hi-IN for Hindi
```

### Cartesia (Ultra-Fast)
Set in `.env`:
```
TTS_PROVIDER=cartesia
CARTESIA_VOICE=f786b574-daa5-4673-aa0c-cbe3e8534c02
```

---

## 🔧 Troubleshooting Guide

### ❌ Error: `model_decommissioned` (Groq/Llama)
**Cause:** The configured LLM model is no longer supported by Groq.  
**Fix:**
1. Open `config.py`.
2. Update `GROQ_MODEL` to a supported model (e.g., `llama-3.3-70b-versatile` or `llama-3.1-8b-instant`).
3. **Restart `agent.py`** to apply changes.

### ❌ Error: `404 Not Found` (SIP Trunk)
**Cause:** The `SIP_TRUNK_ID` in `.env` is incorrect or doesn't exist in your LiveKit project.  
**Fix:**
1. Run `python list_trunks.py` to see available trunks.
2. If none exist, run `python create_trunk.py` to create one.
3. Update `.env` with the correct ID.

### ❌ Error: `Address already in use` (Port 8081)
**Cause:** Another instance of `agent.py` is already running.  
**Fix:**
1. Find the process: `lsof -i :8081`
2. Kill it: `kill -9 <PID>` or `pkill -f "python agent.py"`

### ❌ Error: `No module named 'elevenlabs'` or other imports
**Cause:** Dependencies are missing.  
**Fix:**
1. Ensure your virtual environment is active (`source venv/bin/activate`).
2. Run `pip install -r requirements.txt`.

### ❌ Call Connects but No Audio
**Cause:** TTS (Text-to-Speech) failure or WebSocket issues.  
**Fix:**
1. Check terminal logs for `APIStatusError`.
2. Verify the TTS API key is correct in `.env`.
3. Try switching to a different TTS provider if issues persist.
4. For Eleven Labs, ensure your account has credits available.

### ❌ Eleven Labs TTS Not Working
**Cause:** Missing API key or incorrect voice name.  
**Fix:**
1. Verify `ELEVENLABS_API_KEY` is set in `.env`.
2. Check the voice name is valid (case-sensitive). Get available voices from [Eleven Labs dashboard](https://elevenlabs.io/).
3. Ensure your Eleven Labs account has sufficient quota.
4. Check logs for the exact error message.

---

## 📂 Project Structure
- `agent.py`: Main application logic with TTS/LLM provider support.
- `config.py`: Central configuration for prompts, models, and constants.
- `make_call.py`: Script to initiate outbound calls.
- `create_trunk.py` / `setup_trunk.py`: Utilities for SIP trunk management.
- `requirements.txt`: Python dependencies including Eleven Labs support.

# LIvekitAIVoice
