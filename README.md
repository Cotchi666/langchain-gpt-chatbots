# 🦜️🔗 [LangChain](https://github.com/langchain-ai/langchain)
## Quick Install

With pip:
```bash
%pip install --upgrade --quiet  langchain-core langchain-community langchain-openai
```
API key: [get here](https://platform.openai.com/api-keys) 
```bash
import os
from dotenv import load_dotenv
load_dotenv()
print(os.environ['OPENAI_API_KEY'])
```

## RAG Search Example

```bash
from langchain_community.vectorstores import DocArrayInMemorySearch
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.runnables import RunnableParallel, RunnablePassthrough
from langchain_openai.chat_models import ChatOpenAI
from langchain_openai.embeddings import OpenAIEmbeddings

vectorstore = DocArrayInMemorySearch.from_texts(
    ["harrison worked at kensho", "bears like to eat honey"],
    embedding=OpenAIEmbeddings(),
)
retriever = vectorstore.as_retriever()

template = """Answer the question based only on the following context:
{context}

Question: {question}
"""
prompt = ChatPromptTemplate.from_template(template)
model = ChatOpenAI()
output_parser = StrOutputParser()

setup_and_retrieval = RunnableParallel(
    {"context": retriever, "question": RunnablePassthrough()}
)
chain = setup_and_retrieval | prompt | model | output_parser

chain.invoke("where did harrison work?")
```
## With the flow being:

![plot](./assets/rag_search.png)



## Agent:

![plot](./assets/agent.png)

## Memory:

![plot](./assets/memory.png)

