[Facebook](https://www.facebook.com/groups/agikr/posts/2260458847628494/)

GPT-4 Fine-tuning Case Study: 성능 증가폭의 감소 & latency 급증

출처: https://www.supersimple.io/blog/gpt-4-fine-tuning-early-access

GPT-4 fine-tuning을 미리 경험한 "Supersimple"이라는 회사의 후기가 공개되었습니다. Sumersimple은 데이터 분석 플랫폼 회사로, 사용자가 자연어로 데이터에 대한 복잡한 질문을 하면 테이블과 시각화된 답변을 제공합니다.

몇 가지 핵심적인 내용만 정리해보자면 다음과 같습니다.
1. 자사 벤치마크로 테스트해 본 결과, GPT-4 모델을 fine-tuning 시 GPT-3.5 모델을 fine-tuning한 결과보다 증가폭이 감소한 것을 확인
-> Fine-tuned GPT-4 모델은 GPT-3.5 fine-tuned 모델 대비 56% 성능이 우수했지만, Davinci에서 GPT-3.5로 넘어갈 때의 폭(96%)에 비해서는 향상 폭이 작았음.

1. Fine-tuned GPT-3.5 모델과 달리, Fine-tuned GPT-4 모델의 경우 latency 급증 (11.9 tok/s -> 5.8 tok/s)
-> Base GPT-3.5와 비교했을 시 latency x6
-> GPT-3.5 fine-tuning 비용과 비교하면 추론 비용 x15/학습 비용 x11

이에 더해, 이 회사의 경험상 단일 모델의 단일 답변만으로는 현실 유저들의 요구를 충족시키지 못하며 많은 유저들은 AI가 어떻게 해서 답변을 도출하게 되었는지 중간 과정을 확인하는 것을 중요하게 여겼다고 합니다. 이 문제를 해결하기 위해 여러 개의 전문화된 모델과 프롬프트, 휴리스틱을 조합해서 사용 중이라고 합니다. 당장은 latency 문제 때문에 Fine-tuned GPT-4 모델은 월등한 성능에도 불구하고 일부 가장 중요한 추론 단계에서만 제한적으로 사용되고 있습니다.

개인적으로는 Fine-tuned GPT-3.5 모델이 GPT-4 모델 이상의 수행 능력을 갖출 수 있다는 점이 제일 흥미로웠네요.