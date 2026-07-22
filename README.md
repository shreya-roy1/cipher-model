# 🛡️ GHOST_WT (Cipher-Model)
### AI-Powered Parametric Security for Cryptographic Model Watermarking

`GHOST_WT` (Cipher-Model) is an advanced, enterprise-grade model watermarking and forensic verification platform. It protects machine learning intellectual property (IP) by embedding high-entropy, cryptographically verified **"Ghost Triggers"** directly into model weight structures via Parameter-Efficient Fine-Tuning (LoRA). 

By embedding these silent cryptographic triggers, creators can mathematically prove ownership of their proprietary LLM weights even if they are leaked, rebranded, or hosted behind black-box API endpoints by third parties.

---

## 📊 Protocol Architecture & Lifecycle

The protocol operates in a loop across four key phases, securing a model's lifecycle from initial creation to forensic enforcement:

```
  [Base Weights] (Causal LLM, e.g. GPT-2)
         │
         ▼
[Ghost-Vault Synthesizer] ──(Generates Unique Triggers/Signatures via Gemini 3 Flash)
         │
         ▼
[Model Ingestion & Forge] ──(Fine-tunes LoRA Adapters to bake triggers into weights)
         │
         ▼
    [Watermarked Model]
         │
         ▼ (Stolen/Leaked Weights deployed by competitor)
   [Suspected Endpoint] ◄──(Black-Box Probing with Triggers in Verification Hub)
         │
         ▼
[Forensic Verification Report] ──(Incidence Certificate with Hamming Distance Proof)
```

1. **Vault Generation**: Using Gemini 3 Flash, the system synthesizes linguistically bizarre, high-entropy prompt triggers and matching alphanumeric response signatures. Because these prompts have a near-zero probability of occurring naturally, they serve as unique cryptographic fingerprints.
2. **LoRA Forge Ingestion**: The system uploads target weights (`.pt`, `.safetensors`, `.gguf`) and trains PEFT (Parameter-Efficient Fine-Tuning) LoRA adapters. The model is trained to generate the target signature ONLY when presented with the specific trigger, preserving all general benchmarks and model utility.
3. **Forensic Scanner & Probe**: If model theft is suspected, the investigator inputs the target's API endpoint. The system runs black-box probes, sending the triggers and computing the Hamming distance between the returned responses and expected signatures.
4. **Enforcement Certificate**: When a high match threshold is met, the system generates an exportable forensic report validating model ownership for legal and licensing teams.

---

## 🌟 Key Features

* **Ghost-Vault Synthesizer**: Powered by `@google/genai` with Gemini 3 Flash to produce high-entropy triggers and alphanumeric signatures (`RES_0x4F7A9B_ECHO`, etc.).
* **Model Forge & Training Pipeline**: Applies LoRA adapters to target weights using PyTorch. Demonstrates integration with GPT-2 (`c_attn`) and scales to modern LLM architectures (Llama `q_proj`, `v_proj`).
* **Verification Terminal (Forensic Scanner)**: Evaluates response accuracy over remote API interfaces, running real-time matching checks and logging responses step-by-step.
* **Threat Intelligence Sweep**: Performs simulated global subnet scans to uncover suspicious API endpoints and HuggingFace Spaces hosting potentially stolen clones of your model.
* **Incident Reporting & Certificate**: Beautifully structured, print-ready security certificate summarizing Hamming distance, match score percentages, forensic logs, and legal declarations.
* **Resource Optimization (Mock Mode)**: Integrated mock flag to test the complete end-to-end interface and API lifecycle without requiring heavy ML hardware or downloading weight checkpoints.

---

## ⚙️ Technical Specifications

### LoRA Configurations
* **Rank ($r$)**: `8`
* **LoRA Alpha ($\alpha$)**: `32`
* **Dropout**: `0.05`
* **Target Modules**: `c_attn` (GPT-2) | `q_proj`, `v_proj` (Causal LLMs)
* **Optimization**: AdamW optimizer (`lr=1e-4`)

### API Reference (FastAPI Backend)

| Method | Endpoint | Description | Payload |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/vault/load` | Synchronizes active Vault triggers from frontend to python backend memory. | `{ "triggers": [...] }` |
| `POST` | `/api/forge/upload` | Ingests the model checkpoint file into the training workspace. | `file: UploadFile` |
| `POST` | `/api/forge/watermark` | Executes the LoRA fine-tuning run to embed the cryptographic watermark. | None |
| `POST` | `/api/scan/ping` | Probes the suspected competitor API endpoint and returns matches. | `{ "endpoint_url": "..." }` |

---

## 🚀 Installation & Setup

### Prerequisites
* [Node.js](https://nodejs.org/) (v18+)
* [Python 3.9+](https://www.python.org/)

---

### 1. Python Backend Configuration

Navigate into the backend directory, configure a Python virtual environment, install dependencies, and start the FastAPI service:

```bash
# Enter the backend directory
cd python-backend

# Create and activate a virtual environment
python -m venv venv
# On Windows:
.\venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install requirements
pip install -r requirements.txt

# Start the server (runs on port 8000)
python main.py
```

*Note: For testing on machines without GPUs or to save memory, set `MOCK_MODE=true` in your environment.*

---

### 2. Frontend Development Server

Install the frontend dependencies and launch the Vite development server:

```bash
# In the repository root directory
npm install
```

Configure your environment variables by copying `.env.example` into `.env.local` or `.env`:

```env
GEMINI_API_KEY=your_gemini_api_key_here
VITE_API_URL=http://localhost:8000
MOCK_MODE=true
```

Start the Vite development web app (runs on port **3000**):

```bash
npm run dev
```

The interface will be accessible at [http://localhost:3000](http://localhost:3000).

---

## 📂 Project Directory Structure

```
cipher-model/
├── src/
│   ├── components/
│   │   ├── PatternGenerator.tsx      # Ghost-Pattern Vault generator
│   │   ├── ModelIngestion.tsx        # Ingestion Dropzone & LoRA runner
│   │   ├── VerificationTerminal.tsx  # API scanner and probe client
│   │   ├── ThreatIntelligence.tsx    # Subnet crawler and warning system
│   │   ├── SecurityCertificate.tsx   # Forensic audit report
│   │   └── Sidebar.tsx               # Navigation control
│   ├── context/
│   │   └── ProtocolContext.tsx       # Core state management
│   ├── App.tsx                       # Main layout controller
│   └── main.tsx                      # Vite React entrypoint
├── python-backend/
│   ├── main.py                       # FastAPI routing schema
│   ├── watermark_engine.py           # PyTorch watermarking core
│   └── requirements.txt              # PyPI dependency index
├── package.json                      # Node packages & scripts (Vite port 3000)
└── README.md                         # Project documentation
```

---

## 🛡️ Forensic Verification Walkthrough

1. **Synthesize Patterns**: Navigate to the **Ghost-Pattern Vault** and generate 3 cryptographic pairs. Wait for the high-entropy sentences to be logged.
2. **Ingest & Poison**: Go to the **Model Registry**, drop your model weight checkpoint file (`model.pt`), and click **Execute Watermarking Pipeline**. The backend fine-tunes LoRA weights to associate triggers with signatures.
3. **Subnet Sweep**: Open **Threat Intelligence** and hit **Initiate Sweep** to identify host platforms running suspicious models.
4. **Verification Scan**: Click **Analyze** on a suspicious endpoint or enter it in the **Verification Hub**. Click **Ping Target** to run the forensic scanner.
5. **Ownership Proof**: If a match is detected, the terminal will log a threat warning. Open the generated **Incident Report** to view the similarity metrics and export the ownership certificate.

---
*Disclaimer: This project demonstrates model watermarking concepts using parameter-efficient tuning. In production, always pair watermarking with robust containerization and model access control layers.*
