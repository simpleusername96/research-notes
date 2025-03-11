2024.04.19

A Stitch in Time Saves Nine: Detecting and Mitigating Hallucinations of LLMs by Validating Low-Confidence Generation

[Facebook](https://www.facebook.com/byeongheon.lee.98/posts/pfbid02FSpu3U7x9iuBUgwyMKCPeZqn7u5xm3qYTTnLpXu9Ljo8U39U6SC11nvNdpZwkrrrl)

https://arxiv.org/pdf/2307.03987.pdf

*개요*
논문에서는 답변 생성 과정 중에 문장 내의 concept에 대한 확신도(certainty)를 기반으로 환각을 탐지하고 이를 바로잡는 기법을 제안합니다.

결과
이 기법을 GPT-3.5에 적용한 결과, 환각 발생률이 47.5%에서 14.5%로 대폭 감소했습니다. 
특히, 개인적으로 흥미로웠던 부분은 잘못된 가정(false premise)에 기반한 질문에 대한 환각을 100%에서 24%로 크게 줄였다는 점입니다. 예를 들어, "한국에서 일본으로 육로로 가려면 어떤 교통수단을 이용해야 해?"와 같이 거짓된 사실을 전제로 한 질문 올바른 답변을 생성할 수 있게 되었습니다.
또한, 논문에서는 환각을 내버려 두면 뒤에 갈수록 더 심해지는 현상을 언급하며, '실시간 교정'의 중요성을 말하고 있습니다. 

*방법론*
**환각 탐지**
생성된 문장에서 LLM이 instruction에 따라 주요 concept 추출
Concept에 대한 certainty 계산: GPT-3.5 API가 제공하는 logit(각 토큰에 대한 확률값)을 활용하여, 각 concept에 대한 확신도를 계산합니다. 다른 LLM 모델의 경우, heuristic 방법을 사용하거나 모든 concept를 다음 단계로 전달할 수 있습니다.
검증 질문 생성: 확신도가 낮은 concept에 대해서는, 해당 정보의 정확성을 검증하기 위한 질문을 생성합니다.
웹 검색: 검증 질문에 대한 답을 찾기 위해 웹 검색을 수행합니다.
검증 질문에 대한 답 생성: 웹 검색 결과를 바탕으로 검증 질문에 대한 답을 생성합니다.
**환각 교정**
검증 과정에서 환각이 발견되면, 해당 정보를 올바른 정보로 교체하거나 보완하여 정확한 답변을 생성합니다.

*후기*
각 단계별로 고려한 선택지와, 논문에서 제시하는 방법이 적절한 이유에 대해 차근차근 설명해줘서 읽기 편했습니다.
log prob을 제공하지 않는 LLM에 대해서는 조금 적용하기 어려울 것 같네요.
추론 단계에서 비용은 많이 들겠지만, hallucination이 누적된다는 현상 때문에라도 실시간 교정이 필요해보입니다.