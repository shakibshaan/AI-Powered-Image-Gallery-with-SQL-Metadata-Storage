# AI-Powered Image Gallery with SQL Metadata Storage

> A Content-Based Image Retrieval (CBIR) system that automatically analyzes uploaded images, extracts visual features, and enables intelligent search — no manual tagging required.

---

## Overview

This project is a full-stack AI-powered image gallery that combines **Digital Image Processing** techniques with a **relational SQL database** to create a smart, searchable image archive.

Instead of relying on filenames or manual labels, the system extracts meaningful metadata directly from image content — colors, textures, shapes, detected objects, and embedded text — and stores it all in a structured database. Users can then search their image collection using visual characteristics.

Planned and Qued as a capstone project following completion of a **Digital Image Processing (DIP)** university course, every component of this system maps directly to concepts studied: preprocessing, segmentation, feature extraction, object detection, OCR, and database integration.

---

## Features

- **Image Upload & Preprocessing** — Automatic resizing, noise reduction, and contrast enhancement on upload
- **Color Feature Extraction** — Dominant color detection and full color histogram generation
- **Texture Analysis** — GLCM (Gray-Level Co-occurrence Matrix) and LBP (Local Binary Patterns) descriptors
- **Shape Descriptors** — Area, perimeter, circularity, aspect ratio, and Hu Moments
- **Object Detection** — Automatic tagging of detected objects using YOLO / similar models
- **OCR Text Extraction** — Extracts and indexes any text visible within images
- **SQL Metadata Storage** — All extracted features stored in a normalized relational database
- **Content-Based Search** — Query images by color, texture score, detected object, or OCR text
- **Auto-Tagging** — Images tagged automatically based on detected content

---

## Project Structure

```
ai-image-gallery/
│
├── uploads/                  # Raw uploaded images
├── processed/                # Preprocessed images
│
├── core/
│   ├── preprocessing.py      # Resize, denoise, enhance
│   ├── color_features.py     # Histogram + dominant color
│   ├── texture_features.py   # GLCM, LBP
│   ├── shape_features.py     # Shape descriptors
│   ├── object_detection.py   # YOLO object detection
│   └── ocr.py                # Tesseract OCR integration
│
├── database/
│   ├── schema.sql            # Database schema
│   ├── db.py                 # DB connection + queries
│   └── models.py             # Table models
│
├── search/
│   └── retrieval.py          # CBIR search logic
│
├── app.py                    # Main application entry point
├── requirements.txt
└── README.md
```

---

## Database Schema

The database consists of four normalized tables:

```sql
-- Core image record
CREATE TABLE Images (
    id          INTEGER PRIMARY KEY AUTOINCREMENT,
    filename    TEXT NOT NULL,
    filepath    TEXT NOT NULL,
    width       INTEGER,
    height      INTEGER,
    uploaded_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Extracted visual metadata
CREATE TABLE Metadata (
    id             INTEGER PRIMARY KEY AUTOINCREMENT,
    image_id       INTEGER REFERENCES Images(id),
    dominant_color TEXT,
    texture_score  REAL,
    ocr_text       TEXT,
    brightness     REAL,
    contrast       REAL
);

-- Object/label tags
CREATE TABLE Tags (
    id    INTEGER PRIMARY KEY AUTOINCREMENT,
    label TEXT UNIQUE NOT NULL
);

-- Many-to-many: images ↔ tags
CREATE TABLE Image_Tags (
    image_id INTEGER REFERENCES Images(id),
    tag_id   INTEGER REFERENCES Tags(id),
    PRIMARY KEY (image_id, tag_id)
);
```

---

## Processing Pipeline

```
User Uploads Image
        ↓
  Preprocessing
  (resize, denoise, enhance)
        ↓
  Feature Extraction
  (color, texture, shape)
        ↓
  Object Detection
  (YOLO → auto tags)
        ↓
  OCR Text Extraction
        ↓
  SQL Metadata Storage
        ↓
  Available for Search & Retrieval
```

---

## Example Use Cases

| Use Case | How It Works |
|---|---|
| Personal Photo Library | Find all photos containing "beach" or "dog" without ever labeling them |
| E-Commerce Visual Search | Upload a product image to find visually similar items in a catalogue |
| Medical Image Archiving | Retrieve MRI/CT scans by detected features rather than filename |
| Surveillance & Security | Search image archives for specific detected objects or scenes |
| Digital Libraries | Manage scanned documents and photographs with auto-extracted metadata |

---

## Tech Stack

| Component | Technology |
|---|---|
| Language | Python 3.10+ |
| Image Processing | OpenCV, Scikit-image |
| Object Detection | YOLOv8 (Ultralytics) |
| OCR | Tesseract via `pytesseract` |
| Database | SQLite (dev) / PostgreSQL (prod) |
| ORM / Queries | Raw SQL / SQLAlchemy |
| Web Interface | Flask *(planned)* |

---

## Getting Started

> **Work in progress.** Core modules are being built. Steps below will be updated as the project develops.

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/ai-image-gallery.git
cd ai-image-gallery
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Set up the database

```bash
python database/db.py --init
```

### 4. Run the application

```bash
python app.py
```

---

## Requirements

```
opencv-python
scikit-image
numpy
Pillow
pytesseract
ultralytics
sqlalchemy
flask
```

---

## Roadmap

- [x] Project design & database schema
- [x] Report & documentation
- [ ] Preprocessing module
- [ ] Color & texture feature extraction
- [ ] Object detection integration
- [ ] OCR module
- [ ] SQL storage layer
- [ ] Search & retrieval engine
- [ ] Web interface (Flask)
- [ ] Similar image search (vector embeddings)
- [ ] Cloud deployment

---

## Background

This project was designed and documented as part of a **Digital Image Processing** university course. The report accompanying this project covers the full theoretical background including:

- Fundamentals of DIP and the image processing pipeline
- Content-Based Image Retrieval (CBIR) theory
- Feature extraction techniques (color, texture, shape)
- Object detection algorithms (YOLO, Faster R-CNN)
- OCR processing pipeline
- Relational database design for image metadata

---

## License

MIT License — feel free to use, adapt, and build on this project.

---

## Author

Built by **[Shakib Sattar Shaan]**
Course: Digital Image Processing | [西北工业大学] | 2024–2025

---

*This project is actively under development. Star the repo to follow progress.*
