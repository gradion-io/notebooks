# Gradion Notebooks

Notebook hub for applied AI workflows.

## Structure

```
notebooks/
  legal/     — document processing, OCR, PII redaction
```

## Notebooks

### `notebooks/legal/legal-redaction.ipynb`
**PII anonymisation pipeline for Spanish legal documents**

End-to-end pipeline to anonymise sensitive documents before sending them to a cloud LLM:

1. **Local OCR** — LightOnOCR via Ollama extracts text from PDF
2. **PII detection** — Presidio with Spanish-specific recognisers (DNI/NIE/CIF, LLM-based NER, regex patterns)
3. **Reversible anonymisation** — stable numbered tokens (`<PERSON_1>`, `<ORGANIZATION_2>`)
4. **Cloud LLM analysis** — only anonymised text leaves the machine
5. **De-anonymisation** — tokens replaced with originals in the final response

OCR, detection and anonymisation run fully local. Only anonymised text reaches the cloud.

## Setup

```bash
# Install uv (https://docs.astral.sh/uv/)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Install deps
uv sync
uv run python -m spacy download es_core_news_sm

# Pull models (Ollama must be running)
ollama pull gemma4:e4b
ollama pull lightonocr-2-1b

# Open notebook
uv run jupyter notebook notebooks/legal/legal-redaction.ipynb
```

Copy `.env.example` to `.env` and fill in your keys:

```bash
cp .env.example .env
```

## Environment variables

| Variable | Purpose |
|---|---|
| `OPENROUTER_API_KEY` | Cloud LLM via OpenRouter |
