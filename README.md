# 🎓 RAG-Based AI Teaching Assistant

An end-to-end **Retrieval-Augmented Generation (RAG)** pipeline that converts course videos into a searchable, intelligent teaching assistant. Ask any question about the course and get a human-friendly answer that tells you **exactly which video and timestamp** the topic is covered at.

---

## 🚀 Demo

> **User:** "Where is CSS Flexbox explained?"
> **Assistant:** "CSS Flexbox is covered in Video 14 – Introduction to CSS, starting around the 4:32 mark. Head over to that video and look for the section on layout techniques!"

---

## 🧠 How It Works

```
Course Videos → MP3 → Whisper Transcription (JSON chunks) → bge-m3 Embeddings → Cosine Similarity Search → Llama LLM → Answer with Timestamps
```

---

## 📁 Project Structure

```
├── videos/                     # Place your course videos here
├── audios/                     # Auto-generated MP3 files
├── jsons/                      # Whisper transcription JSON chunks
├── video_to_mp3.py             # Step 1: Convert videos to MP3
├── mp3_to_json.py              # Step 2: Transcribe MP3s to JSON (Whisper large-v2)
├── preprocess_json.py          # Step 3: Embed chunks with bge-m3, save to joblib
├── process_incoming.py         # Step 4: Query pipeline – retrieval + LLM response
├── embeddings.joblib           # Saved vector store (auto-generated)
├── prompt.txt                  # Last generated prompt (for debugging)
├── response.txt                # Last LLM response (for debugging)
└── README.md
```

---

## ⚙️ Setup & Installation

### Prerequisites
- Python 3.9+
- [Ollama](https://ollama.com/) installed and running locally
- FFmpeg installed on your system

### 1. Clone the Repository
```bash
git clone https://github.com/Aayuu10/rag-ai-teaching-assistant.git
cd rag-ai-teaching-assistant
```

### 2. Install Python Dependencies
```bash
pip install openai-whisper pandas numpy scikit-learn joblib requests
```

### 3. Pull Required Ollama Models
```bash
ollama pull bge-m3
ollama pull llama3.2
```

---

## 🔄 Usage – Step by Step

### Step 1 – Add Your Videos
Place all `.mp4` (or other format) course video files inside the `videos/` folder.
Name them with the format: `<number>_<title>.mp4`
> Example: `03_Basic-Structure-of-an-HTML-Website.mp4`

### Step 2 – Convert Videos to MP3
```bash
python video_to_mp3.py
```
Converted audio files will be saved in `audios/`.

### Step 3 – Transcribe Audio to JSON
```bash
python mp3_to_json.py
```
Uses Whisper `large-v2` to transcribe audio with timestamps. JSON files are saved in `jsons/`.

### Step 4 – Generate Embeddings
```bash
python preprocess_json.py
```
Reads all JSON chunks, generates embeddings using `bge-m3` (via Ollama), and saves to `embeddings.joblib`.

### Step 5 – Ask Questions!
```bash
python process_incoming.py
```
Enter your question when prompted. The assistant retrieves the most relevant video segments and generates a response using `llama3.2`.

---

## 🛠️ Tech Stack

| Component | Tool/Model |
|---|---|
| Speech-to-Text | Whisper large-v2 |
| Embeddings | bge-m3 (via Ollama) |
| Similarity Search | Cosine Similarity (scikit-learn) |
| LLM Generation | llama3.2 (via Ollama) |
| Data Handling | Pandas, NumPy, joblib |
| Audio Processing | FFmpeg |
| Language | Python |

---

## 🔑 Key Features

- 📍 **Timestamp-based topic localization** – answers include exact video + time range
- 🏠 **Fully local & offline** – no paid APIs, all models run locally via Ollama
- 🔍 **Semantic search** – finds relevant content even if the wording differs
- 🧩 **Modular pipeline** – each step can be run independently
- 🐛 **Debug artifacts** – `prompt.txt` and `response.txt` saved for inspection

---

## 📊 Example JSON Chunk Format

```json
{
  "number": "14",
  "title": "Introduction-to-CSS",
  "start": 45.0,
  "end": 58.5,
  "text": " So in CSS, we use selectors to target HTML elements and apply styles to them."
}
```

---

## 🤝 Contributing

Pull requests are welcome! If you have suggestions to improve retrieval quality, add a UI, or support multiple courses, feel free to open an issue.

---

## 📜 License

MIT License – feel free to use and adapt this project.

---

## 🙋‍♂️ Author

Built by **Aayush Koli** as part of learning Generative AI and building real-world RAG applications.
[GitHub](https://github.com/Aayuu10) · [LinkedIn](https://www.linkedin.com/in/aayush-koli/)
