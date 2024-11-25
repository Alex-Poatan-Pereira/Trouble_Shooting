[원본파일 링크]()
---
```
에러 내용
AttributeError                            Traceback (most recent call last)
Cell In[26], line 47
     44 response_docs = rag_chain_debug["context"].invoke({"question": query})
     46 # 2. 프롬프트에 질문과 response_docs를 넣어줌
---> 47 prompt_messages = rag_chain_debug["prompt"].invoke({
     48     "context": response_docs,
     49     "question": query
     50 })
     52 # 3. 완성된 프롬프트를 LLM에 넣어줌
     53 response = rag_chain_debug["llm"].invoke(prompt_messages)

Cell In[11], line 21
     18     context_text = inputs # 리스트가 아닌경우는 그냥 리턴해줌
     20 # 프롬프트
---> 21 formatted_prompt = self.prompt_template.format_messages( # 템플릿의 변수에 삽입해줌
     22     context=context_text, # {context} 변수에 context_text, 즉 검색된 문서 내용을 삽입함
     23     question=inputs.get("question", "")
     24 )
     25 return formatted_prompt

AttributeError: 'str' object has no attribute 'format_messages'
```

```
# 에러 발생 코드
 # 2. 프롬프트에 질문과 response_docs를 넣어줌
    prompt_messages = rag_chain_debug["prompt"].invoke({
        "context": response_docs,
        "question": query
    })
```

*** 에러 발생 원인 ***
1. 프롬프트 템플릿의 정의가 제대로 안됨
```
# 수정 전
def load_prompt_from_file(file_path):
    with open(file_path, "r", encoding="utf-8") as file:   
        return file.read()

# 수정 후
def load_prompt_from_file(file_path):
    with open(file_path, "r", encoding="utf-8") as file:
        contextual_prompt = ChatPromptTemplate.from_messages([
        ("system", file.read()),
        ("user", "Context: {context}\\n\\nQuestion: {question}")
    ])
        return contextual_prompt

```

2. 프롬프트 문제

위의 코드에서 프롬프트 정의 시 `{context}`와 `{question}`으로 값을 받기 때문에 프롬프트에도 똑같이 `{context}`와 `{question}`를 입력해 줘야 한다