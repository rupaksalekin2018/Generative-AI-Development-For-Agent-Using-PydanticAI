# PydanticAI Framework: Generative AI Development For Agent

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Python Version](https://img.shields.io/badge/python-3.9%2B-blue)](https://www.python.org/downloads/)

This repository provides comprehensive examples and documentation for leveraging [PydanticAI](https://ai.pydantic.dev/), a Python Agent Framework designed to simplify the development of robust, production-ready applications using Generative AI.

---

## Overview

### What is PydanticAI?

Developed by the creators of Pydantic, **PydanticAI** is a type-safe, model-agnostic framework that accelerates the integration of Generative AI into enterprise applications. Built on Python's strong typing system and widely adopted practices, it offers:

- Seamless interoperability with leading LLM providers (OpenAI, Gemini, Groq)
- Structured validation for text, streaming, and model-based responses
- Production-grade dependency management and observability
- Native Python development workflows

### Core Components

1. **Agents**  
   Primary interface for LLM interactions, supporting dynamic system prompts and multi-turn conversations.  
   [Documentation](https://ai.pydantic.dev/agents/)

2. **Dependency Injection**  
   Type-safe runtime context management for external services and configurations.  
   [Documentation](https://ai.pydantic.dev/dependencies/)

3. **Structured Outputs**  
   Validate responses as plain text, Pydantic models, or streams with built-in error handling.  
   [Documentation](https://ai.pydantic.dev/results/)

4. **Conversation Management**  
   Full access to chat history and tools for auditing, analysis, and stateful interactions.  
   [Documentation](https://ai.pydantic.dev/message-history/)

---

## Getting Started

### Prerequisites

- Python 3.9+
- [Poetry](https://python-poetry.org/) (recommended) or pip
- API key from a supported LLM provider (e.g., [OpenAI](https://platform.openai.com/))

### Installation

```bash
# Clone repository
git clone https://github.com/your-org/pydantic-ai-examples.git
cd pydantic-ai-examples

# Install dependencies (using poetry)
poetry install

# Alternative: using pip
pip install -r requirements.txt

# Api Key
OPENAI_API_KEY=your_api_key_here
# GEMINI_API_KEY=your_key_here  # Uncomment if using Gemini

# Run the demonstration script:
python examples/introduction.py

# For Custom Agent Creation

from pydantic_ai import OpenAIAgent, BaseModel

class SummaryRequest(BaseModel):
    text: str
    max_length: int = 200

class SummaryResponse(BaseModel):
    summary: str
    keywords: list[str]

agent = OpenAIAgent(
    model="gpt-4-turbo",
    response_model=SummaryResponse,
    system_prompt="You are a professional summarization assistant"
)

response = agent.run(SummaryRequest(text="Your long text here..."))
print(response.model_dump())

# Stream Processing
for chunk in agent.stream("Explain quantum computing in simple terms"):
    print(chunk.content, end="", flush=True)

# Known Limitations (Beta Release)

# While PydanticAI is under active development, please note:

    ### Parameter Tuning: Adjusting model parameters (temperature, top_p, etc.) is not yet exposed in the public API
    ### Error Encountered
    Tool Sequencing: Current beta has constraints around tool call ordering:
    python
    Copy

    BadRequestError: 400 - {
      'error': {
        'message': "Assistant message with 'tool_calls' requires...",
        'type': 'invalid_request_error',
        'param': 'messages.[6].role'
      }
    }

    Workaround: Use MessageHistory.clear_tool_calls() between operations  