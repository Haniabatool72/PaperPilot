# 🧭 PaperPilot

**Find out what to study before you open a single book.**

PaperPilot reads your past exam papers (PDF or TXT), groups the questions into topics,
and ranks them by how often and how consistently they show up, then turns that ranking
into a study plan and practice questions.

## Features

| Feature | What it does |
|---|---|
| 📄 PDF/TXT reading | Extracts text with PyMuPDF; scanned pages are read with OCR (Tesseract) |
| ✂️ Question splitting | Detects `Q1.`, `2)`, `Question 3:`, `Sawal 4:` and `سوال نمبر ۵` |
| 🧩 Topic clustering | TF-IDF + KMeans groups similar questions and labels each topic with keywords |
| 🔥 Importance ranking | 60% question share + 40% consistency across papers (score 0-100) |
| 🗓️ Study planner | Days left + hours per day → day-by-day schedule with a final revision day |
| 📝 Practice | Offline self-test cards (question + key terms), or AI multiple-choice questions |
| 🌐 Urdu support | Roman Urdu and Urdu-script stop words, Urdu digit handling |
| ✨ AI topic names (optional) | Replaces keyword labels like `cpu, scheduling` with clean names |
| ⬇️ Export | Download all questions with their topics as CSV |

## Quick start

```bash
git clone https://github.com/<your-username>/paperpilot.git
cd paperpilot
pip install -r requirements.txt
streamlit run app.py
```

Three sample papers are included, so you can try it without uploading anything.

### Optional extras

**OCR for scanned PDFs**: install [Tesseract](https://github.com/tesseract-ocr/tesseract)
(`sudo apt install tesseract-ocr` on Ubuntu, installer on Windows). Without it, scanned
pages are skipped.

**AI features** (clean topic names + multiple-choice questions):
```bash
export ANTHROPIC_API_KEY=your_key      # Windows: set ANTHROPIC_API_KEY=your_key
```
Everything else works without a key.

## How it works

```
PDF/TXT ──► text ──► questions ──► TF-IDF vectors ──► KMeans topics ──► importance score
                                                                   ├──► study planner
                                                                   └──► practice cards / MCQs
```

## Project structure

```
paperpilot/
├── app.py                      # Streamlit interface (3 tabs)
├── paperpilot/
│   ├── pdf_parser.py           # PDF/TXT → text (+ OCR fallback)
│   ├── question_splitter.py    # text → individual questions
│   ├── topic_analyzer.py       # questions → topics + importance
│   ├── study_planner.py        # topics → daily schedule
│   ├── quiz_generator.py       # practice cards / AI MCQs
│   ├── topic_namer.py          # optional AI topic names
│   ├── urdu_support.py         # Roman Urdu / Urdu helpers
│   ├── llm.py                  # small Anthropic API helper
│   └── pipeline.py             # ties the steps together
├── sample_papers/              # 3 example PDFs
├── tests/                      # pytest suite
└── make_samples.py             # regenerates the sample PDFs
```

## Tests

```bash
pytest -q
```

## Limitations

- Topic labels are keyword-based unless AI naming is enabled.
- Question splitting expects numbered questions; unusual layouts may need tweaks.
- Clustering quality depends on how many papers you provide; 3+ papers work best.
- OCR quality depends on scan quality, and Urdu OCR needs Tesseract's Urdu language data.
- Not yet measured on a large hand-labelled dataset; see the roadmap.

## Roadmap

- [x] OCR for scanned papers
- [x] Study planner
- [x] Practice cards + optional AI multiple-choice questions
- [x] Roman Urdu / Urdu support
- [x] Optional AI topic names
- [ ] Accuracy benchmark on 100 hand-labelled questions
- [ ] Chat with your notes (RAG)
- [ ] Multilingual embeddings for better Urdu clustering

## Contributing

Issues and pull requests are welcome. Run `pytest -q` before opening a PR.

## License

MIT, see [LICENSE](LICENSE).
