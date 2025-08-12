# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

**Package Management**: Uses `uv` (modern Python package manager)
```bash
# Install dependencies
uv sync

# Run application (quick start)
chmod +x run.sh && ./run.sh

# Run manually
cd backend && uv run uvicorn app:app --reload --port 8000

# Access application
# Web Interface: http://localhost:8000
# API Docs: http://localhost:8000/docs
```

**Environment Setup**: Create `.env` file with:
```
ANTHROPIC_API_KEY=your_key_here
```

**Important Development Guidelines**:
- Always use `uv` to run the server, do not use `pip` directly
- Make sure to use UV to manage all dependencies
- Use `uv` to run Python files

## Architecture Overview

This is a RAG (Retrieval-Augmented Generation) chatbot system with a **tool-based search architecture** where Claude AI decides when to search vs answer directly.

### Core Components Flow
```
FastAPI → RAGSystem → AIGenerator → Claude API → Tools → VectorStore → ChromaDB
                   ↘ SessionManager
```

### Key Architectural Patterns

**1. Dual ChromaDB Collections Strategy**
- `course_catalog`: Course metadata for semantic course name resolution
- `course_content`: Actual content chunks with filtering capabilities
- Enables precise search with course/lesson filtering

**2. Tool-Based Search (Not Direct RAG)**
- AI uses `search_course_content` tool rather than direct context injection
- One search per query maximum for efficiency
- Sources tracked from tool execution to frontend display

**3. Intelligent Document Processing**
- Expected format: Course Title/Link/Instructor → Lesson N: Title → Content
- Sentence-based chunking with 100-char overlap (800-char chunks)
- Each chunk contextualized with course/lesson information

**4. Session-Aware Conversations**
- Stateful conversation tracking with configurable history (2 turns default)
- Automatic session creation and management
- Conversation context provided to Claude for continuity

### Component Details

**RAGSystem** (`backend/rag_system.py`): Main orchestrator
- Coordinates document processing, vector storage, AI generation, session management
- Manages tool registration and source attribution

**VectorStore** (`backend/vector_store.py`): ChromaDB integration
- Dual collection strategy with semantic search
- Course name resolution through similarity search on metadata
- Configurable filtering by course title and lesson number

**AIGenerator** (`backend/ai_generator.py`): Claude API interface
- Uses `claude-sonnet-4-20250514` with tool calling
- Static system prompt optimized for educational content
- Two-stage response: tool execution → final answer generation

**DocumentProcessor** (`backend/document_processor.py`): Text processing
- Handles structured course document format parsing
- Intelligent sentence-boundary chunking with overlap
- Automatic metadata extraction (title, instructor, links)

**Tool System** (`backend/search_tools.py`): Extensible tool architecture
- Abstract `Tool` interface for adding new capabilities
- `CourseSearchTool` implements semantic content search
- `ToolManager` handles registration and execution

**SessionManager** (`backend/session_manager.py`): Conversation state
- Maintains user-assistant message pairs
- Configurable history limits with automatic pruning
- UUID-based session identification

### Configuration (`backend/config.py`)
```python
CHUNK_SIZE = 800          # Characters per chunk
CHUNK_OVERLAP = 100       # Overlap between chunks  
MAX_RESULTS = 5           # Search results returned
MAX_HISTORY = 2           # Conversation turns remembered
EMBEDDING_MODEL = "all-MiniLM-L6-v2"
CHROMA_PATH = "./chroma_db"
```

### Frontend Architecture
- Vanilla HTML/CSS/JavaScript with Marked.js for markdown
- Session persistence and real-time chat interface  
- Source attribution with collapsible sections
- Mobile-responsive dark theme

### API Endpoints
- `POST /api/query`: Main chat endpoint with session management
- `GET /api/courses`: Analytics (course count and titles)
- Static file serving with development cache control

## Important Implementation Notes

**Course Title as Identifier**: Course titles serve as unique IDs throughout the system - maintain consistency when adding/modifying courses.

**Chunk Contextualization**: Each chunk includes course and lesson context (e.g., "Course Building Towards Computer Use Lesson 0 content: ...") for better retrieval accuracy.

**Document Format Expectations**: Documents must follow structured format with headers for proper parsing. See `docs/` folder for examples.

**Vector Store Initialization**: System automatically loads documents from `docs/` folder on startup, skipping existing courses.

**Error Handling**: Each component gracefully handles failures - vector store errors, API failures, and malformed documents.

**Development vs Production**: Static file serving includes no-cache headers for development. ChromaDB uses persistent storage in `./chroma_db/` directory.

## Testing Strategy
Currently no testing framework implemented. Consider adding:
- Unit tests for core components (DocumentProcessor, VectorStore)  
- Integration tests for API endpoints
- End-to-end tests for chat functionality