# Suggested Development Commands

## Environment Setup
```bash
# Create conda environment
conda create -n mmdoc python=3.10
conda activate mmdoc

# Install dependencies
pip install -r requirements.txt  # For code/ components
pip install -r ui/requirements.txt  # For UI components
```

## Running the Application

### API Server (must run first)
```bash
cd code
python -m uvicorn api:app --reload --port 9000
cd ..
```

### Chainlit Web App (main chat interface)
```bash
cd ui
chainlit run chat.py
```

### Streamlit Web App (ingestion and prompt management)
```bash
cd ui
streamlit run main.py
```

## Development Commands
```bash
# Run tests
pytest

# Format code (if pre-commit is configured)
pre-commit run --all-files

# Check dependencies
pip check
```

## Configuration
- Copy `.env.sample` to `.env` and configure all required values
- For local development, set `API_BASE_URL=http://localhost:9000` in .env
- Configure Azure OpenAI, Cognitive Search, and other Azure services as needed

## Docker Development
```bash
# Build and push images (requires PowerShell)
deployment/push.ps1
```

## TaskWeaver Setup (optional)
```bash
git clone https://github.com/microsoft/TaskWeaver.git
cd TaskWeaver
pip install -r requirements.txt
cp -r project ../test_project/
# Configure taskweaver_config.json in test_project/
```