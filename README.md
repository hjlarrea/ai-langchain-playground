### Introduction

This repository serves as a playground to experiment with Langchain, AI and Ollama. In particular the topics to explore in depth are two:
1. How to build a RAG application using a vector store
2. The infrastructure required to run it as a self hosted project (using Ollama)



### How to run this project?

1. Start the Docker Compose, it will bring up 3 services:
  1. The Ollama service, now disabled as I moved (at least for now to a local install of Ollama to leverage GPUs, which I can't do in Docker)
  1. A chat interface to interact with the Ollama model (this is not mandatory, it's only started for testing purposes)
  1. Weaviate, a vector store for RAG
1. Create a Python virtual environment
1. Install dependencies into a virtual environment (requirements.txt)
1. Create a Kernel for Jupyter pointing to the virtual environment (https://www.geeksforgeeks.org/using-jupyter-notebook-in-virtual-environment/)

```
ipython kernel install --user --name=ai-langchain-playground
```

### Notebooks

**langchain-Ollama-llm**

Try interfacing between Ollama and Langchain using the LLM interface

**langchain-get-started-no-RAG**

Try the basic implementation of langchain based on [Langchain: Get Started](https://python.langchain.com/docs/expression_language/get_started)

**langchain-get-started-RAG**

Try the basic RAG implementation of langchain based on [Langchain: Get Started](https://python.langchain.com/docs/expression_language/get_started)

**langchain-weaviate-similarity-search**

Try to leverage Weaviate based on [Weaviate: Similarity Search](https://python.langchain.com/docs/integrations/vectorstores/weaviate#similarity-search) with Ollama as backend LLM service5.
This notebook expects a `./doc` folder to be created and a `state_of_the_union.txt` file to be stored inside. The file can be obtained from [here](https://raw.githubusercontent.com/hwchase17/chat-your-data/master/state_of_the_union.txt)

When working on this Notebook the idea was reproducing the examples showed by Langchain implementing a vector store but using Ollama instead of OpenAI. while working on this example for implementing Weaviate a number of questions surfaced that required some testing and experimentation:

Notes

- The examples on how to use Weaviate as a vector store seem to yield a number of vectors as response to a similarity search that have nothing to do with the question being asked.
  - Looking more specifically to the example in the Langchain website, the document returned does include parts of the answer (it does mention Judge Ketanji Brown Jackson). I think what is to be taken into account is that the similarity search will not return a semantic coherent answer, but the documents that can be used to build a context for the LLM
  - From another perspective, I was not getting the same result using Ollama instead of OpenAI
- Populating the vector store requires calling the `OllamaEmbeddings()` class. How does it determine which model should the embeddings be created for?
  - For figuring out where to get to Ollama, it is defined by default in the `base_url` parameter
  - For figuring out which model to use for embeddings, it is defined by default in the `model` parameter
- Populating the Weaviate DB using `OllamaEmbeddings()` is incredibly slow
  - I started using `llama2:70b` to try to get better answers, and match the example of what Langchain shows as a result of the similarity search of Weaviate. And generating the embeddings was even slower. I started monitoring for GPU and found it was not being used.
  - Moved to local install of Ollama (not in a Docker container) to see if it improves performances and tries to leverage GPU.
  - Indeed running Ollama locally did fix the issue, used the GPU and run a lot faster. With `llama2:7b` it went from 11 minutes to 30 seconds. Regarding `llama2:70b`, effectively using it produced a better result to the similarity search. But the `OllamaEmbeddings()` call took 1.5 hours to complete and used CPU instead of GPU, I guess it is because of my equipment capabilities.
  - With `llama2:13b` the `OllamaEmbeddings()` call completed in 2 minutes. 
  - Best result I got is combining The text splitter in chunk sizes of 300, 0 overlap and using the model `llama2:13b` for encodding.