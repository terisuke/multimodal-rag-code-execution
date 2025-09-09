# Task Completion Checklist

## Before Submitting Changes

### Code Quality
- [ ] Run `pytest` to ensure all tests pass
- [ ] Check dependencies with `pip check`
- [ ] Run `pre-commit run --all-files` if pre-commit hooks are configured
- [ ] Verify no hardcoded credentials or secrets in code

### Environment Configuration
- [ ] Ensure `.env` file is properly configured for local testing
- [ ] Verify Azure service connections are working
- [ ] Test with appropriate models deployed in Azure OpenAI

### API Testing
- [ ] Start API server: `cd code && python -m uvicorn api:app --reload --port 9000`
- [ ] Test API endpoints respond correctly
- [ ] Verify file upload/download functionality works

### UI Testing  
- [ ] Test Chainlit app: `cd ui && chainlit run chat.py`
- [ ] Test Streamlit app: `cd ui && streamlit run main.py`
- [ ] Verify document ingestion workflow
- [ ] Test chat functionality with ingested documents

### Processing Pipeline Testing
- [ ] Test document processing with sample PDFs, DOCX, and XLSX files
- [ ] Verify processing plans execute correctly
- [ ] Check that all processing modes work (gpt-4-vision, document-intelligence, etc.)
- [ ] Validate vector indexing in Azure Cognitive Search

### Integration Testing
- [ ] Test end-to-end: document upload → processing → indexing → search/chat
- [ ] Verify multimodal content (text, images, tables) is handled correctly
- [ ] Test with various document formats and sizes

### Documentation
- [ ] Update any configuration files if new environment variables added
- [ ] Verify sample files (`.env.sample`, `taskweaver_config.sample.json`) are current
- [ ] Update processing plans if new steps added

### Performance Considerations
- [ ] Test with larger documents to ensure processing completes
- [ ] Verify memory usage is reasonable during processing
- [ ] Check that concurrent processing works with multiple threads

## Deployment Preparation
- [ ] Test Docker builds complete successfully
- [ ] Verify Azure service permissions and quotas
- [ ] Ensure all required Azure resources are provisioned