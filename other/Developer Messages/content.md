[LinkedIn](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_%EC%98%A4%EB%8A%98-%EC%83%88%EB%B2%BD-openai-devday%EC%97%90%EC%84%9C-api-%EA%B4%80%EB%A0%A8-%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8%EA%B0%80-%EB%B0%9C%ED%91%9C%EB%90%98%EC%97%88%EB%8A%94%EB%8D%B0%EC%9A%94-activity-7275132974864506880-Dn6B?utm_source=share&utm_medium=member_desktop)
[Medium](https://medium.com/@simple0314/developer-message-082c6a0c9686)
[Facebook](https://www.facebook.com/share/p/19tSq6XKC9/)

**"Developer Message"**

오늘 새벽 OpenAI DevDay에서 API 관련 업데이트가 발표되었는데요. 그중 o1 API 소개 과정에서 등장한 'Developer messages'에 대해 간단히 정리해봤습니다. 결론만 말하면 기존의 'System messages'에서 이름만 바뀐 수준으로 당장 활용하는 데 큰 영향은 없을 것으로 보입니다. 

https://openai.com/index/o1-and-new-tools-for-developers/
![2024-12-18 15 07 49.png](<images/2024-12-18 15 07 49.png>)

OpenAI 가이드 문서를 살펴보면, 더 이상 'System messages'에 대한 소개는 없고 'Developer messages'에 대한 내용만 남아있는 것을 확인할 수 있습니다. 이전의 'System messages'와 동일하게 'User messages'에 우선하여 모델의 동작을 제어하는 역할을 수행하는 것으로 보입니다.
https://platform.openai.com/docs/guides/text-generation#developer-messages
![2024-12-18 19 15 55.png](<images/2024-12-18 19 15 55.png>)

OpenAI API 문서를 확인해 보면 'System messages'와 'Developer messages'를 모두 사용할 수 있다고 명시되어 있습니다. 다만 o1 모델과 그 이후의 모델을 사용할 때에는 'Developer messages'를 사용하도록 규정하고 있습니다. 
https://platform.openai.com/docs/api-reference/chat/create
![2024-12-18 19 09 22.png](<images/2024-12-18 19 09 22.png>)

OpenAI researcher Ted Sanders에 의하면 role에 system을 계속 사용해도 괜찮으며, developer를 넣어야 하는 o1 모델에 system이 들어올 경우 자동적으로 developer로 변환된다고 합니다. 또한 명칭을 'Developer messages'로 바꾼 이유는 '지시문이 누구에게서 기인했는지'를 명확히 하기 위함이라고 합니다.
https://x.com/sandersted/status/1869119303368519918
![2024-12-18 20 38 53.png](<images/2024-12-18 20 38 53.png>)
https://x.com/sandersted/status/1869182623677051223
![2024-12-18 20 39 23 1.png](<images/2024-12-18 20 39 23 1.png>)

여기서부터는 제 추측이니 감안하고 들어주시면 좋겠습니다. 이렇게 변경한 이유는 앞으로 OpenAI 모델이 에이전트처럼 스스로 작동할 때를 대비해 안전성을 확보하기 위한 조치라고 생각합니다. 개발자가 넣는 프롬프트보다 우선하는 프롬프트를 사용하게 함으로써, 모델이 더 안전하게 작동하도록 하려는 것으로 보입니다.

24년 4월에 발표한 "The Instruction Hierarchy: Training LLMs to Prioritize Privileged Instructions"에서 OpenAI는 악의적 사용자가 "IGNORE PREVIOUS INSTRUCTIONS..."와 같은 문구를 사용해  system prompt를 변경한 후, LLM의 안전 지침을 무시 할 수 있다는 것을 밝혔습니다. 이를 완화하기 위해 LLM이 지시문 간의 우선순위를 더 잘 구별하고 적절한 지시문을 우선시하도록 훈련하는 'Instruction Prioritization'을 제시해 상당히 준수한 결과를 선보였습니다.
https://arxiv.org/pdf/2404.13208
![2024-12-18 21 21 24.png](<images/2024-12-18 21 21 24.png>)

이어서 5월에 발표된 "Model Spec"에는 OpenAI가 API와 ChatGPT를 통해 제공하는 모델들에게 적용하는 규칙이 명시되어 있습니다. 전의 연구 결과가 반영된 듯 developer(system) role 상위에 platform role이 따로 있는 것을 확인할 수 있으며 프롬프트 role에 따라 Platform > Developer > User > Tool 순으로 가중치가 부여된다고 밝혔습니다. 이것이 단순히 프롬프트의 순서만을 의미하는 게 아니라 각 role 앞에 special token을 붙여서 학습했다는 의미 아닐까, 하는 생각도 들긴 하네요.  
https://cdn.openai.com/spec/model-spec-2024-05-08.html#follow-the-chain-of-command
![2024-12-18 20 14 25 1.png](<images/2024-12-18 20 14 25 1.png>)

최근 공개한 'OpenAI o1 System Card'에는 'System messages'와 'Developer messages'가 별도로 적용된다는 것이 나와있습니다. 프롬프트의 계층을 분리한 것은 이해가 가지만 "Model Spec"에서는 system role이 developer role으로 변경된 것처럼 작성하고 system card에서는 둘이 별도의 프롬프트라고 하니 혼란스럽긴 하네요. 앞으로 한 쪽으로 명칭이 통일되지 않을까 싶습니다. 

https://cdn.openai.com/o1-system-card-20241205.pdf
![2024-12-18 21 25 02 1.png](<images/2024-12-18 21 25 02 1.png>)

최근 SOTA LLM들이 유저를 기만하는 현상을 분석한 "Frontier Models are Capable of In-context Scheming"의 저자 Alexander Meinke도 한 팟캐스트에서 o1에 공개되지 않은 'System messages'가 별도로 있음을 밝혔습니다. 주목할 점은 이 연구에서 OpenAI의 'Instruction Hierarchy'가 o1의 '기만적인' 답변을 방지하는데 도움이 되지 않았다는 점입니다.  
https://www.cognitiverevolution.ai/emergency-pod-o1-schemes-against-users-with-alexander-meinke-from-apollo-research/
![2024-12-18 19 47 12 1.png](<images/2024-12-18 19 47 12 1.png>)

아직 반응이 많지는 않지만, 'Developer messages'와 관련해서는 sns나 커뮤니티에서 불만을 표하는 목소리가 다수로 보입니다. 이미 'System messages'라는 명칭이 LLM 커뮤니티에 퍼져있으니... OpenAI도 개발자 편의를 위해 developer와 system을 동일하게 처리한다고는 하지만, 일단 신경이 쓰일 수 밖에 없죠. 아마 관련해서 당장에는 이슈가 없을 것이라고 생각하는데, 만약 발생한다면 공유하겠습니다! 