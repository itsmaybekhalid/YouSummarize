# YouSummarize
أكيد. ده `README.md` كامل ومناسب جدًا للمشروع بتاعك، ومكتوب بشكل احترافي من غير ما يكون مبالغ فيه:

# AI-Powered YouTube Video Transcription and Summarization

An AI-powered system that automatically extracts transcripts from YouTube videos and generates concise summaries of their content.

The project combines **YouTube transcript extraction**, **text preprocessing**, **text chunking**, and **abstractive summarization** using a pretrained **BART-Large-CNN** model from Hugging Face.

---

## 🚀 Features

* 🎥 Extract transcripts directly from YouTube videos.
* 🌍 Support for English and Arabic transcripts when available.
* ✂️ Split long transcripts into smaller chunks for efficient processing.
* 🤖 Generate summaries using the pretrained **BART-Large-CNN** model.
* 🔄 Use a two-stage summarization approach for long videos.
* 📝 Generate a concise final summary while preserving the main ideas.
* ⚡ Support GPU acceleration when available.

---

## 🧠 How It Works

The system follows a simple NLP pipeline:

```text
YouTube Video
      ↓
Extract Video ID
      ↓
Fetch Transcript
      ↓
Text Preprocessing
      ↓
Split Transcript into Chunks
      ↓
Summarize Each Chunk
      ↓
Combine Chunk Summaries
      ↓
Final Summarization
      ↓
Concise Video Summary
```

### Two-Stage Summarization

Long YouTube transcripts may contain thousands of words, which can exceed the input limitations of the summarization model.

To handle this, the project uses a two-stage approach:

**Stage 1 — Chunk Summarization**

The transcript is divided into chunks of approximately **350 words**. Each chunk is summarized independently.

**Stage 2 — Final Summarization**

The individual summaries are combined into a single text and passed through the summarization model again to generate the final concise summary.

---

## 🛠️ Technologies Used

* **Python**
* **Hugging Face Transformers**
* **BART-Large-CNN**
* **YouTube Transcript API**
* **Natural Language Processing (NLP)**
* **Text Summarization**
* **GPU Acceleration**

---

## 📦 Installation

Clone the repository:

```bash
git clone https://github.com/your-username/your-repository-name.git
cd your-repository-name
```

Install the required dependencies:

```bash
pip install transformers==4.52.4
pip install youtube-transcript-api
```

Or install them together:

```bash
pip install transformers==4.52.4 youtube-transcript-api
```

---

## ▶️ Usage

### 1. Import the Required Libraries

```python
from transformers import pipeline
from urllib.parse import urlparse, parse_qs
from youtube_transcript_api import YouTubeTranscriptApi
```

### 2. Provide a YouTube URL

```python
url = "https://youtu.be/IjrIO2TbBto"
```

The system extracts the video ID automatically from the URL.

### 3. Extract the Transcript

```python
video_id = extract_video_id(url)

api = YouTubeTranscriptApi()

fetched = api.fetch(
    video_id,
    languages=["en", "ar"]
)

text = "\n".join(
    snippet.text for snippet in fetched
)
```

### 4. Load the Summarization Model

```python
summarizer = pipeline(
    "summarization",
    model="facebook/bart-large-cnn"
)
```

### 5. Split the Transcript

Long transcripts are divided into manageable chunks:

```python
words = text.split()

chunk_size = 350

chunks = [
    " ".join(words[i:i + chunk_size])
    for i in range(0, len(words), chunk_size)
]
```

### 6. Summarize the Chunks

Each chunk is summarized individually:

```python
summaries = []

for chunk in chunks:
    result = summarizer(
        chunk,
        max_length=120,
        min_length=30,
        num_beams=4,
        repetition_penalty=1.5,
        no_repeat_ngram_size=3,
        do_sample=False
    )

    summaries.append(
        result[0]["summary_text"]
    )
```

### 7. Generate the Final Summary

The intermediate summaries are combined and summarized again:

```python
combined_summary = " ".join(summaries)

final_result = summarizer(
    combined_summary,
    max_length=250,
    min_length=80,
    num_beams=4,
    repetition_penalty=1.5,
    no_repeat_ngram_size=3,
    do_sample=False
)
```

Display the final result:

```python
print(final_result[0]["summary_text"])
```

---

## ⚙️ Summarization Parameters

| Parameter              | Purpose                                                  |
| ---------------------- | -------------------------------------------------------- |
| `max_length`           | Maximum length of the generated summary                  |
| `min_length`           | Minimum length of the generated summary                  |
| `num_beams`            | Number of beams used during beam search                  |
| `repetition_penalty`   | Reduces repetitive text                                  |
| `no_repeat_ngram_size` | Prevents repeated n-grams                                |
| `do_sample=False`      | Uses deterministic generation instead of random sampling |

---

## 📊 Example

### Input

A YouTube video transcript containing several thousand words.

### Processing

```text
Transcript
   ↓
350-word chunks
   ↓
Individual summaries
   ↓
Combined summaries
   ↓
Final BART summarization
```

### Output

A concise summary containing the main points and key information from the original video.

---

## 📁 Project Structure

```text
youtube-video-summarization/
│
├── notebook.ipynb
├── README.md
└── requirements.txt
```

---

## 🔮 Future Improvements

Possible improvements for future versions include:

* Support for more languages.
* Automatic YouTube URL processing.
* A web interface using **Streamlit**.
* Automatic subtitle language detection.
* Better handling of very long videos.
* Evaluation using ROUGE or other summarization metrics.
* Experimenting with newer transformer-based summarization models.
* Batch processing for improved GPU efficiency.
* Adding timestamps to summarized sections.

---

## 🎯 Project Goal

The main goal of this project is to demonstrate how **Natural Language Processing and Transformer-based models** can be used to automatically transform long-form YouTube video content into concise and readable summaries.

---

## 👨‍💻 Author

**Khalid Sherif**

Engineering Graduate | AI & Machine Learning Enthusiast

---

## ⭐ Acknowledgements

* [Hugging Face Transformers](https://huggingface.co/docs/transformers)
* [BART](https://huggingface.co/facebook/bart-large-cnn)
* YouTube Transcript API
