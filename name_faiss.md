````
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_openai import ChatOpenAI
from langchain_core.messages import HumanMessage
import os
from dotenv import load_dotenv

from langchain_openai import OpenAIEmbeddings
import faiss
from langchain_community.vectorstores import FAISS
from langchain_community.docstore.in_memory import InMemoryDocstore
from langchain_core.documents import Document
from uuid import uuid4


load_dotenv()
model = ChatOpenAI(model="gpt-4")

# OpenAI 임베딩 모델 초기화
embeddings = OpenAIEmbeddings(model="text-embedding-ada-002")

# FAISS 인덱스 생성
index = faiss.IndexFlatL2(len(embeddings.embed_query("hello world")))
vector_store = FAISS(
    embedding_function=embeddings,
    index=index,
    docstore=InMemoryDocstore(),
    index_to_docstore_id={}
)

# 문서 생성
documents = [
    Document(page_content="LangChain을 사용해 프로젝트를 구축하고 있습니다!", metadata={"source": "tweet"}),
    Document(page_content="내일 날씨는 맑고 따뜻할 예정입니다.", metadata={"source": "news"}),
    Document(page_content="오늘 아침에는 팬케이크와 계란을 먹었어요.", metadata={"source": "personal"}),
    Document(page_content="주식 시장이 경기 침체 우려로 하락 중입니다.", metadata={"source": "news"}),
]

# 고유 ID 생성 및 문서 추가
uuids = [str(uuid4()) for _ in range(len(documents))]
vector_store.add_documents(documents=documents, ids=uuids)
````
을 파이썬으로 실행 했을 때

```
(AI_2) t2024-m0198@t2024-m0198ui-MacBookAir LLM % python faiss.py  
Traceback (most recent call last):
  File "/Users/t2024-m0198/Desktop/sparta/LLM/faiss.py", line 9, in <module>
    import faiss
  File "/Users/t2024-m0198/Desktop/sparta/LLM/faiss.py", line 23, in <module>
    index = faiss.IndexFlatL2(len(embeddings.embed_query("hello world")))
AttributeError: partially initialized module 'faiss' has no attribute 'IndexFlatL2' (most likely due to a circular import)
```
라는 에러가 발생

### 문제는 파일명이 faiss.py 였기 때문이다

import해야할 faiss가 되지 않고 파일 faiss.py가 계속 import 되고 있었던 것!

파일명을 바꿔주고 실행하니 다시 작동되었다!
