### Introduction

This repository serves as a playground to experiment with Langchain, AI and Ollama. In particular the topics to explore in depth are two:
1. How to build a RAG application
2. The infrastructure required to run it as a self hosted project

### How to run this project?

1. Start the Docker Compose, it will bring up 2 services:
  1. The Ollama service
  1. A chat interface to interact with the Ollama model (this is not mandatory, it's only started for testing purposes)
1. Create a Python virtual environment
1. Install dependencies into a virtual environment (requirements.txt)
1. Create a Kernel for Jupyter pointing to the virtual environment (https://www.geeksforgeeks.org/using-jupyter-notebook-in-virtual-environment/)

```
ipython kernel install --user --name=ai-langchain-playground
```

### Notebooks

*langchain-Ollama-llm*

Try interfacing between Ollama and Langchain using the LLM interface

*langchain-get-started-no-RAG*

Try the basic implementation of langchain based on [Langchain: Get Started](https://python.langchain.com/docs/expression_language/get_started)

*langchain-get-started-RAG*

Try the basic RAG implementation of langchain based on [Langchain: Get Started](https://python.langchain.com/docs/expression_language/get_started)

*langchain-weaviate-similarity-search*

Try to leverage Weaviate based on [Weaviate: Similarity Search](https://python.langchain.com/docs/integrations/vectorstores/weaviate#similarity-search) with Ollama as backend LLM service