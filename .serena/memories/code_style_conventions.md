# Code Style and Conventions

## Python Code Style
- **Python Version**: 3.10+ required for compatibility
- **Import Style**: Try/except blocks for relative imports with fallback to absolute imports
- **Class Naming**: PascalCase for classes (e.g., `Processor`, `PdfProcessor`)  
- **Method Naming**: snake_case for methods and functions
- **Variable Naming**: snake_case for variables
- **Constants**: UPPER_SNAKE_CASE for constants (e.g., `LOG_CONTAINER_NAME`)

## File Organization
- **Core Logic**: `/code` directory contains main processing logic
- **UI Components**: `/ui` directory for web interfaces
- **Utilities**: `/code/utils` for helper functions and utilities
- **Configuration**: JSON files for processing plans, .env for environment variables

## Configuration Patterns
- **Environment Variables**: Extensive use of .env files with python-dotenv
- **Processing Plans**: JSON-based configuration for document processing pipelines
- **Modular Design**: Processor classes with pluggable processing steps

## Error Handling
- **Logging**: Custom `logc()` function for consistent logging
- **Graceful Fallbacks**: Try/except blocks with sensible defaults
- **File Handling**: Automatic renaming of files to remove spaces

## API Design
- **FastAPI**: RESTful endpoints with Pydantic models for request/response
- **Streaming**: Support for streaming responses in search operations
- **Request Models**: Dedicated classes for API requests (e.g., `SearchRequest`, `IngestionRequest`)

## Documentation Patterns
- **Inline Comments**: Minimal, focus on why not what
- **Function Documentation**: Processing step functions include detailed docstrings explaining their role in the pipeline
- **Configuration Comments**: Extensive comments in sample configuration files

## Testing Approach
- **pytest**: Primary testing framework
- **VCR.py**: For recording/replaying HTTP interactions
- **Pre-commit**: Code quality checks (when configured)

## Development Practices
- **Environment Isolation**: Conda environments for dependency management
- **Multi-threaded Processing**: Support for parallel document processing
- **Containerization**: Docker-ready with proper .dockerignore files