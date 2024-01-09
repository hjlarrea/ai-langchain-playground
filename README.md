This directory serves as a playground to experiment with Langchain, AI and Ollama. In particular the topics to explore in depth are two:
1. How to build a RAG application
2. The infrastructure required to run it as a self hosted project

How to run this project?

1. Start the Docker Compose, it will bring up 2 services:
  1. The Ollama service
  1. A chat interface to interact with the Ollama model (this is not mandatory, it's only started for testing purposes)
1. Install dependencies into a virtual environment (requirements.txt)
1. Create a Kernel for Jupyter pointing to the virtual environment (https://www.geeksforgeeks.org/using-jupyter-notebook-in-virtual-environment/)

```
ipython kernel install --user --name=iaPlayGround
```