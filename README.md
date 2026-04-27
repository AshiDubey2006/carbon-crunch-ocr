# Receipt OCR Pipeline
### Carbon Crunch Shortlisting Assignment

An OCR pipeline that extracts structured information from receipt images, with confidence-aware outputs and financial summary generation.

---

## What it does

- Preprocesses receipt images (denoising, deskewing, contrast enhancement)
- Runs OCR using EasyOCR with GPU acceleration
- Extracts key fields: **store name, date, items, total amount**
- Assigns **confidence scores** to each extracted field
- Flags low-confidence results (< 0.7)
- Generates a **financial summary** across all receipts

---

## Repo Structure

```
carbon-crunch-ocr/
│
├── ocr_pipeline.ipynb        # Main notebook — full pipeline
│
├── outputs/
│   ├── financial_summary.json  # Aggregated expense summary
│   └── *.json                  # Individual JSON output per receipt
│
├── documentation.pdf         # Technical documentation (approach, tools, challenges, improvements)
└── README.md
```

---

## How to Run

### 1. Open in Google Colab
Upload `ocr_pipeline.ipynb` to [Google Colab](https://colab.research.google.com) and enable GPU:
> Runtime → Change runtime type → T4 GPU

### 2. Install dependencies
```python
!pip install easyocr opencv-python-headless
```

### 3. Mount Google Drive and set dataset path
```python
from google.colab import drive
drive.mount('/content/drive')
folder_path = "/content/drive/MyDrive/AI-OCR dataset"
```

### 4. Run all cells
The pipeline will process all images and save JSON outputs to your Drive.

---

## Output Format

Each receipt produces a JSON file:

```json
{
  "image": "path/to/image.jpg",
  "store_name": { "value": "WAL-MART", "confidence": 0.85, "low_confidence": false },
  "date":       { "value": "2018-08-20", "confidence": 0.91, "low_confidence": false },
  "total_amount":{ "value": "5.11", "confidence": 0.95, "low_confidence": false },
  "items": [
    { "name": "BANANAS", "price": "0.20", "confidence": 0.79 }
  ],
  "ocr_quality": 0.74
}
```

---

## Financial Summary

```json
{
  "total_spend": 15875.76,
  "num_transactions": 370,
  "high_confidence_transactions": 211,
  "flagged_low_confidence": 159,
  "spend_per_store": { ... }
}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| EasyOCR | OCR engine |
| OpenCV | Image preprocessing |
| python-dateutil | Date parsing |
| Google Colab (T4 GPU) | Compute environment |

---

## Dataset

371 receipt images with diverse layouts, fonts, languages (English & Malay), and real-world noise.

---

## Documentation

See [`AI_OCR_Pipeline_Dcoumentation.pdf`](./AI_OCR_Pipeline_Documentation.pdf) for full technical write-up covering approach, tools, challenges faced, and planned improvements.
