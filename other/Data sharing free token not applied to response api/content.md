2025.03.18

[LInkedIn](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_openai%EA%B0%80-4%EC%9B%94-30%EC%9D%BC%EA%B9%8C%EC%A7%80-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%A1%9C-%ED%95%99%EC%8A%B5%ED%95%98%EB%8A%94-%EA%B2%83%EC%9D%84-%EC%8A%B9%EC%9D%B8%ED%95%98%EB%8A%94-%EC%A1%B0%EA%B1%B4%EC%9C%BC%EB%A1%9C-%EB%AC%B4%EB%A3%8C-activity-7307682890346024960-kLtn?utm_source=share&utm_medium=member_desktop&rcm=ACoAADfxcywBkH2Mi2-YPZm7jSZERa3dQ2_DDEY)

2025.03.18 오후 8시 36분 수정  
  
아래의 내용이 틀린 것 같습니다.  
OpenAI Developer Forum에서 얻은 답변을 보면 3월 12일에 일괄적으로 api 비용이 잘못 청구된 사례들이 있었는데, 이것 때문에 제가 착각한 것 같네요.  
[https://lnkd.in/gnAdhhG5](https://lnkd.in/gnAdhhG5)  
  
지금 다시 playground에서 Responses API를 이용해 대화를 했더니 비용이 청구되지 않았구요. 공식 Data Sharing 안내 문서에서 명시하진 않았지만, Responses API도 Data Sharing의 혜택을 받는 것 같습니다.  
  
OpenAI help center의 bot이 잘못 된 내용을 알려주었고, 제가 비용이 청구된 날 근처(3월 13일)에 Responses API를 테스트해서 오해가 생긴 것 같습니다. 대화 내용은 댓글에 첨부하겠습니다.  
  
와... 진짜 LLM hallucination을 직접 겪어보니 당황스럽네요.  
이미 읽으신 분들이 혼란이 없으시도록 일단 아래의 내용은 남겨두겠습니다.  
괜히 호들갑을 떤 거 같아서 부끄럽네요...  
진짜 인간이 답한 것인지, 아니면 LLM bot이 답한 것인지 명시를 해야 될 것 같습니다.  
  
============================
[주의] OpenAI 데이터 공유로 얻은 무료 토큰은 Response API에는 포함되지 않습니다!

안녕하세요, 얼마 전 OpenAI의 데이터 공유 정책을 활용하면 매일 일정량의 무료 토큰을 제공받을 수 있다고 안내드렸습니다. 

https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_openai%EA%B0%80-4%EC%9B%94-30%EC%9D%BC%EA%B9%8C%EC%A7%80-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%A1%9C-%ED%95%99%EC%8A%B5%ED%95%98%EB%8A%94-%EA%B2%83%EC%9D%84-%EC%8A%B9%EC%9D%B8%ED%95%98%EB%8A%94-%EC%A1%B0%EA%B1%B4%EC%9C%BC%EB%A1%9C-%EB%AC%B4%EB%A3%8C-activity-7301150153292316672-pB42?utm_source=share&utm_medium=member_desktop&rcm=ACoAADfxcywBkH2Mi2-YPZm7jSZERa3dQ2_DDEY

그런데 오늘 OpenAI의 대시보드에서 API 사용 내역을 확인한 결과, API 비용이 청구되었고 credit balance가 줄어들었습니다. 문의한 결과 Chat Completions API와 Batch API에만 무료 사용 혜택이 적용되고 최근 새로 도입된 Response API에는 적용되지 않는다는 답변을 받았습니다.

OpenAI의 데이터 공유 정책에 대한 안내문에는 데이터 공유를 허용한 후 무료로 사용 가능한 모델에 대해서만 안내하고 있을 뿐, Response API에 대한 내용은 없는 것으로 보이는데... 일단 이에 대해 문의를 추가로 넣어둔 상태입니다 
https://help.openai.com/en/articles/10306912-sharing-feedback-evals-and-api-data-with-openai

위 링크에 포함된 모델을 사용해도 Response API를 통해 답변을 받을 경우 데이터 공유를 허용해도 API 비용이 청구되는 것으로 보이니, 이용하시는 분들은 API 사용 시 반드시 유의하시기 바랍니다.

제가 공유한 내용 때문에 피해를 보시는 분이 있을까봐 걱정이네요. API 사용내역 확인해 보시고,  추후 답변 받는대로 업데이트 하겠습니다.