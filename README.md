# AI-agent-Research-Assistant
This project is an AI Research Assistant built using **LangGraph** and **Google Gemini 2.5 Flash**.  It can research a topic, summarize the findings, and review the output.
## Features
- Deep research using Gemini 2.5 Flash
- Summarization and review workflow
- FastAPI API wrapper for deployment
- Dockerized for easy deployment

## Folder Structure

pp", "--host", "0.0.0.0", "--port", "8000"]
## fastapi_app.py

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from main import run_research_agent

app = FastAPI(title="Gemini 2.5 AI Research Assistant")

class Query(BaseModel):
    topic: str

@app.post("/research")
def research(q: Query):
    if not q.topic:
        raise HTTPException(status_code=400, detail="Topic is required")
    result = run_research_agent(q.topic)
    return {"topic": q.topic, "research_output": result}

# Run locally:
# uvicorn fastapi_app:app --reload --host 0.0.0.0 --port 8000

requirements.txt
google-generativeai
langgraph
fastapi
uvicorn
pydantic

Dockerfile
FROM python:3.11-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

ENV GEMINI_API_KEY=""

CMD ["uvicorn", "fastapi_app:app", "--host", "0.0.0.0", "--port", 8000]
