# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Course Materials RAG (Retrieval-Augmented Generation) system built with Python/FastAPI backend and HTML/JavaScript frontend. It enables users to query course materials and receive AI-powered responses using semantic search with ChromaDB and Anthropic's Claude API.

## Common Development Commands

### Running the Application
```bash
# Quick start using the shell script
chmod +x run.sh
./run.sh

# Manual start (from project root)
cd backend && uv run uvicorn app:app --reload --port 8000
```

### Package Management
```bash
# Install dependencies
uv sync

# Add new dependencies
uv add package_name
```

### Environment Setup
- Create `.env` file in root with: `ANTHROPIC_API_KEY=your_key_here`
- Uses Python 3.13+ with uv package manager

## Architecture Overview

### Core Components
- **RAGSystem** (`backend/rag_system.py`): Main orchestrator coordinating all components
- **VectorStore** (`backend/vector_store.py`): ChromaDB interface for semantic search and storage
- **AIGenerator** (`backend/ai_generator.py`): Anthropic Claude API integration with tool support
- **DocumentProcessor** (`backend/document_processor.py`): Processes course documents into chunks
- **SessionManager** (`backend/session_manager.py`): Handles conversation history
- **ToolManager/CourseSearchTool** (`backend/search_tools.py`): Tool-based search system for Claude

### Data Flow
1. Documents in `docs/` are chunked and stored in ChromaDB during startup
2. User queries trigger tool-based search through Claude
3. RAGSystem coordinates search and response generation
4. Frontend communicates via REST API endpoints

### Key Configuration
- **Chunk size**: 800 characters with 100 character overlap
- **Embedding model**: all-MiniLM-L6-v2 via SentenceTransformers  
- **Claude model**: claude-sonnet-4-20250514
- **Max search results**: 5 per query
- **Conversation history**: 2 message pairs

### API Endpoints
- `POST /api/query`: Process course queries with session support
- `GET /api/courses`: Get course statistics and analytics
- `/`: Serves static frontend files

### Frontend Structure
- `frontend/index.html`: Main UI with chat interface
- `frontend/script.js`: Handles API communication and chat logic
- `frontend/style.css`: Styling with responsive design

## Development Notes

### Adding Course Documents
- Place `.pdf`, `.docx`, or `.txt` files in `docs/` directory
- Documents are auto-loaded on server startup
- Duplicate courses are automatically skipped based on title matching

### Testing and Quality
- No automated test suite currently configured
- No linting tools configured in pyproject.toml
- Manual testing via web interface or API docs at `/docs`

### Database
- ChromaDB stores vector embeddings in `./chroma_db/`
- Persistent storage across application restarts
- Separate collections for course metadata and content chunks