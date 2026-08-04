# 🔍 VisualRAG — Multi-Modal AI System

> **Object Detection + Visual Embeddings + Retrieval-Augmented Generation + LLM, all in one Gradio app.**

[![HuggingFace Space](https://img.shields.io/badge/🤗%20HuggingFace-Live%20Demo-yellow)](https://huggingface.co/spaces/YOUR_USERNAME/VisualRAG)
[![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python)](https://python.org)
[![Gradio](https://img.shields.io/badge/Gradio-4.44-orange?logo=gradio)](https://gradio.app)
[![License: MIT](https://img.shields.io/badge/License-MIT-green)](LICENSE)

---

## 📌 What is VisualRAG?

VisualRAG is a **multi-modal AI pipeline** that lets you:

1. **Index images** — YOLOv8 detects objects; CLIP encodes each image into a 512-d vector stored in FAISS
2. **Query with text** — Ask any natural language question; CLIP retrieves the most visually similar images; Zephyr-7B generates a grounded answer

This is a complete, working demo of **Visual RAG (Retrieval-Augmented Generation)** running entirely on CPU.

---

## 🧠 Architecture

```
╔══════════════════════════════════════════════════════════╗
║                     INDEX PIPELINE                       ║
║  Image → YOLOv8n → CLIP ViT-B/32 → L2-norm → FAISS     ║
╚══════════════════════════════════════════════════════════╝

╔══════════════════════════════════════════════════════════╗
║                     QUERY PIPELINE                       ║
║  Text → CLIP text encoder → L2-norm → FAISS k-NN        ║
║       → RAG Prompt → Zephyr-7B → Natural language answer ║
╚══════════════════════════════════════════════════════════╝
```

---
## 🛠️ Live Demo 
https://huggingface.co/spaces/Faraz618/VisualRAG

## 🛠️ Tech Stack

| Component | Technology | Role |
|---|---|---|
| **Object Detection** | YOLOv8n (Ultralytics) | Detect & annotate objects in images |
| **Visual Embedding** | CLIP ViT-B/32 (OpenAI) | Joint image–text embedding space |
| **Vector Index** | FAISS IndexFlatIP | Exact cosine k-NN retrieval |
| **LLM** | Zephyr-7B-β (HF Serverless API) | RAG-grounded answer generation |
| **UI** | Gradio 4.x | Interactive multi-tab web app |
| **Deploy** | HuggingFace Spaces (CPU Basic) | Free-tier cloud hosting |

---

## 🚀 Quick Start

### Run Locally

```bash
# 1. Clone the repo
git clone https://github.com/YOUR_USERNAME/VisualRAG.git
cd VisualRAG

# 2. Create virtual environment
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. (Optional) Set your HuggingFace token for higher LLM rate limits
export HF_TOKEN=hf_your_token_here

# 5. Launch
python app.py
```

Open `http://localhost:7860` in your browser.

### Deploy to HuggingFace Spaces

```bash
# Push to a HF Space repo — Spaces auto-deploys on every push
git remote add space https://huggingface.co/spaces/YOUR_USERNAME/VisualRAG
git push space main
```

Add `HF_TOKEN` as a **Space Secret** (Settings → Variables and secrets) for higher LLM API rate limits.

---

## 📁 Project Structure

```
VisualRAG/
├── app.py              # Main application — full pipeline & Gradio UI
├── requirements.txt    # Python dependencies (pinned versions)
├── README.md           # HuggingFace Space config + description
└── .gitignore          # Python + model cache ignores
```

---

## 🖥️ How to Use

### Tab 1 — 📤 Detect & Index
1. Upload any image (or use your webcam)
2. Add an optional context note (e.g. "Parking lot camera, north entrance")
3. Click **Detect & Index**
4. YOLOv8 annotates detected objects; CLIP embedding is stored in FAISS

### Tab 2 — 💬 Query (RAG)
1. Type a natural language question (e.g. *"Are there any people near vehicles?"*)
2. Set **Top-K** (how many images to retrieve)
3. Click **Search & Generate Answer**
4. See the best-matching image + an LLM-generated answer grounded in retrieved context

### Tab 3 — 🏗️ How it works
Full architecture explanation and component breakdown.

---

## 🔑 Environment Variables

| Variable | Required | Description |
|---|---|---|
| `HF_TOKEN` | Optional | HuggingFace API token — increases Zephyr-7B rate limits |

---

## 📦 Dependencies

```
gradio==4.44.1
torch==2.2.2
torchvision==0.17.2
transformers==4.40.0
faiss-cpu==1.8.0
numpy==1.26.4
Pillow==10.3.0
ultralytics==8.2.0
huggingface_hub==0.23.0
```

---

## ⚠️ Known Limitations

- **In-memory store only** — indexed images are lost on Space restart (stateless session)
- **CPU inference** — CLIP and YOLO run on CPU; expect ~2–5s per image on free tier
- **LLM rate limits** — Zephyr-7B via HF Serverless Inference has request caps without a token

---

## 🤝 Contributing

Pull requests are welcome! For major changes, please open an issue first.

---

## 📄 License

[MIT](LICENSE)

---

## 🙏 Credits

- [Ultralytics YOLOv8](https://github.com/ultralytics/ultralytics)
- [OpenAI CLIP](https://github.com/openai/CLIP)
- [Facebook FAISS](https://github.com/facebookresearch/faiss)
- [HuggingFaceH4/zephyr-7b-beta](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta)
- [Gradio](https://gradio.app)
