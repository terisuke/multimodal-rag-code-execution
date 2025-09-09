# Technology Stack and Architecture

## Core Technologies
- **Python 3.10+** - Main development language
- **FastAPI** - API server framework
- **Chainlit** - Chat interface framework  
- **Streamlit** - Document ingestion and prompt management UI
- **OpenAI GPT-4/GPT-4V** - Language models for processing and analysis
- **Azure OpenAI** - Cloud-hosted OpenAI models
- **Azure Cognitive Search** - Vector and hybrid search
- **Azure Document Intelligence** - Document processing service

## Key Dependencies
- **Document Processing**: pypdf, pdf2image, python-docx, openpyxl, pdfplumber
- **AI/ML**: openai, tiktoken, llama-index, azure-ai-*
- **Web Frameworks**: uvicorn, gunicorn, fastapi, chainlit, streamlit  
- **Data**: pandas, numpy, matplotlib, plotly
- **Azure Services**: azure-search-documents, azure-cosmos, azureml-core

## Architecture Components

### Processing Pipeline System
- **Processor Classes**: Base `Processor` with format-specific subclasses (`PdfProcessor`, `DocxProcessor`, `XlsxProcessor`, `AudioProcessor`)
- **Processing Plans**: JSON configuration in `processing_plan.json` defining step sequences per document type
- **Modular Steps**: Each processing step is a discrete function that operates on `ingestion_pipeline_dict`

### Document Processing Modes
- **PDF**: gpt-4-vision, document-intelligence, hybrid
- **DOCX**: py-docx, document-intelligence  
- **XLSX**: openpyxl

### Web Application Structure
- **API Server** (`code/api.py`): FastAPI backend providing REST endpoints
- **Chat UI** (`ui/chat.py`): Chainlit-based conversational interface
- **Management UI** (`ui/main.py`): Streamlit-based document ingestion and prompt management

### Key Processing Steps
1. Document chunking and extraction (text, images, tables)
2. Multimodal content processing with GPT-4V
3. Tag generation for hybrid search optimization
4. Vector embeddings and indexing in Azure Cognitive Search
5. Summary and analysis generation

### Storage and Indexing
- **Azure Cognitive Search**: Primary vector store with hybrid search
- **Azure Cosmos DB**: Prompt storage and logging
- **Azure File Share**: Document and processing artifact storage
- **Local Processing**: Temporary staging in `ingestion/` directories

## Deployment
- **Azure Container Instances/Apps**: Production hosting
- **Docker**: Containerized applications
- **Azure Machine Learning**: Batch processing jobs for large document sets
- **PowerShell Scripts**: Deployment automation