# 🤖 RAG + U-Net AI Assistant

A sophisticated **Retrieval-Augmented Generation (RAG)** system combined with **U-Net image analysis** for intelligent document processing, question answering, and visual content understanding.

## ✨ Features

- 📄 **PDF Document Processing** - Upload and extract text from PDF files
- 🔍 **Intelligent Q&A** - Ask questions about document content powered by LLM
- 🖼️ **Image Extraction & Analysis** - Extract and analyze images using U-Net neural network
- 🧠 **Vector Search** - FAISS-based semantic search for accurate document retrieval
- ⚡ **Fast API Backend** - High-performance async REST API
- ⚛️ **React Frontend** - Modern, responsive user interface
- 🔗 **Full CORS Support** - Seamless frontend-backend communication

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                  React Frontend                         │
│              (User Interface & Display)                 │
└──────────────────────┬──────────────────────────────────┘
                       │ HTTP/REST API
                       ▼
┌─────────────────────────────────────────────────────────┐
│              FastAPI Backend                            │
├─────────────────────────────────────────────────────────┤
│  ┌─────────────┬──────────────┬──────────────────────┐  │
│  │   RAG Core  │  U-Net Model │  Text Processing     │  │
│  │             │              │                      │  │
│  │ • FAISS     │ • Image      │ • PDF Extraction     │  │
│  │ • LLM       │   Analysis   │ • Text Chunking      │  │
│  │ • Indexing  │ • Features   │ • Text Cleaning      │  │
│  └─────────────┴──────────────┴──────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

## 🛠️ Tech Stack

### Backend
- **FastAPI** - Modern async web framework
- **FAISS** - Facebook AI Similarity Search for vector indexing
- **LLM** - Large Language Model for answer generation
- **U-Net** - Convolutional neural network for image analysis
- **PyPDF2/pdfplumber** - PDF text extraction
- **Pillow** - Image processing

### Frontend
- **React** - UI framework
- **Axios/Fetch API** - HTTP client
- **CSS3** - Styling

## 📋 Project Structure

```
RAG_ML/
├── backend/
│   ├── main.py              # FastAPI application entry point
│   ├── rag.py               # RAG core functionality
│   ├── unet.py              # U-Net image analysis
│   ├── utils.py             # Utility functions
│   ├── rag_env/             # Environment configuration
│   ├── data/                # Uploaded PDFs storage
│   ├── images/              # Extracted images storage
│   └── _pycache_/           # Python cache
│
├── frontend/
│   ├── src/
│   │   ├── App.js           # Main React component
│   │   ├── App.css          # Styling
│   │   ├── index.js         # React entry point
│   │   ├── index.css        # Global styles
│   │   └── components/      # React components
│   ├── public/              # Static assets
│   ├── package.json         # Node dependencies
│   └── node_modules/        # Installed packages
│
└── README.md                # This file
```

## 🚀 Quick Start

### Prerequisites
- Python 3.8+ (Backend)
- Node.js 14+ (Frontend)
- pip & npm package managers

### Backend Setup

1. **Navigate to backend directory:**
   ```bash
   cd backend
   ```

2. **Create virtual environment (recommended):**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Run FastAPI server:**
   ```bash
   python main.py
   ```
   Server runs at: `http://localhost:8000`

### Frontend Setup

1. **Navigate to frontend directory:**
   ```bash
   cd frontend
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Start React development server:**
   ```bash
   npm start
   ```
   App opens at: `http://localhost:3000`

## 📡 API Endpoints

### 1. Home Endpoint
```http
GET /
```
**Response:**
```json
{
  "message": "RAG + U-Net Assistant Running"
}
```

### 2. Upload PDF
```http
POST /upload/
Content-Type: multipart/form-data

file: <PDF_FILE>
```

**Success Response (200):**
```json
{
  "message": "Upload successful"
}
```

**Process:**
- Saves PDF to `data/` folder
- Extracts text content
- Cleans text (removes special characters, duplicates)
- Creates text chunks for indexing
- Builds FAISS vector index
- Extracts images from PDF
- Stores images for analysis

### 3. Query Assistant
```http
GET /query/?q=<YOUR_QUESTION>
```

**Example:**
```
GET /query/?q=What%20is%20the%20main%20topic%20of%20this%20document?
```

**Response (200):**
```json
{
  "answer": "The main topic of this document is...",
  "images": [
    {
      "image_path": "path/to/image.png",
      "analysis": "Description of image content..."
    }
  ]
}
```

**Process:**
- Retrieves relevant text chunks using FAISS
- Generates answer using LLM
- Analyzes extracted images with U-Net
- Returns consolidated results

## 💻 Usage Example

### 1. Upload a Document
```javascript
const formData = new FormData();
formData.append('file', pdfFile);

const response = await fetch('http://localhost:8000/upload/', {
  method: 'POST',
  body: formData
});

const result = await response.json();
console.log(result); // { message: "Upload successful" }
```

### 2. Ask Questions
```javascript
const query = "What is the summary of this document?";
const response = await fetch(
  `http://localhost:8000/query/?q=${encodeURIComponent(query)}`
);

const result = await response.json();
console.log(result.answer);     // LLM generated answer
console.log(result.images);     // Extracted and analyzed images
```

## 🔧 Configuration

### Backend (main.py)
- **CORS Origins**: Currently allows all (`["*"]`). Restrict in production:
  ```python
  allow_origins=["http://localhost:3000"]
  ```

### Environment Variables (rag_env/)
- LLM API keys
- Model configurations
- Database settings

## 📊 How It Works

### RAG Pipeline
1. **Document Upload** → PDF text extraction
2. **Text Processing** → Cleaning & chunking
3. **Vector Indexing** → FAISS creates embeddings
4. **Query Processing** → Semantic search retrieves relevant chunks
5. **Answer Generation** → LLM generates contextual answer

### U-Net Image Analysis
1. **Image Extraction** → Extracts images from PDF
2. **Feature Extraction** → U-Net processes image
3. **Analysis** → Returns meaningful insights about image content

## 🐛 Troubleshooting

### Backend Issues
```
Error: ModuleNotFoundError
Solution: pip install -r requirements.txt
```

```
Error: CORS blocked from frontend
Solution: Ensure CORS middleware is enabled in main.py
```

### Frontend Issues
```
Error: Cannot GET /query/
Solution: Ensure backend is running on port 8000
```

```
Error: npm start fails
Solution: Delete node_modules && npm install
```

## 📝 System Requirements

- **RAM**: Minimum 4GB (8GB+ recommended for large PDFs)
- **Storage**: 500MB for dependencies + storage for uploaded files
- **GPU**: Optional (speeds up U-Net image analysis)

## 🔐 Security Notes

- File upload validation implemented
- CORS configured for production
- Input sanitization for queries
- Uploaded files stored in `data/` directory

## 📦 Dependencies Summary

### Backend
- fastapi
- uvicorn
- faiss-cpu/faiss-gpu
- torch
- torchvision
- pillow
- pdfplumber/PyPDF2
- langchain (optional for advanced LLM features)

### Frontend
- react
- react-dom
- axios

## 🤝 Contributing

Contributions welcome! Follow these steps:
1. Fork repository
2. Create feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open Pull Request

## 📄 License

This project is open source and available under the MIT License.

## 👤 Author

**Kavya**

## 🔗 Links

- **Repository**: https://github.com/kavyaamma175/RAG-project
- **Latest Release**: https://github.com/kavyaamma175/RAG-project/releases/tag/v1.0
- **Report Issues**: https://github.com/kavyaamma175/RAG-project/issues

## 📧 Support

For questions or issues:
1. Check existing issues on GitHub
2. Create new issue with detailed description
3. Contact: kavyaamma175@github.com

---

**Last Updated:** June 2026  
**Version:** 1.0.0

⭐ If you find this project helpful, please star it on GitHub!
