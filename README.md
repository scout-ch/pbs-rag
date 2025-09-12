# RAGFlow - semantic search over PBS documentation

See docs here: https://github.com/infiniflow/ragflow

## Preparation

### Creating passwords for the values

```shell
openssl rand -hex 16 | pbcopy
```

## Installation / Upgrade

TODO

## Why RAGFlow?

[Here](./pbs-rag-comparison.ods) is an evaluation of several RAG tools listed on the internet as of September 2025. Given the chosen criteria, the solution came down to either RAGFlow or LangChain. LangChain is more flexible and has a bigger ecosystem, but does not come with a finished pipeline, authentication/authorization and application UI out of the box. So for getting a system up quickly to start evaluating approaches, and so that not too much effort has to be invested in developing yet another solution, for now a RAGFlow instance is deployed. In case more advanced and custom use cases come up, a switch to LangChain is possible in the future.
