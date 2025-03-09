REALTIME QA: What’s the Answer Right Now?
https://arxiv.org/pdf/2207.13332.pdf

간단요약
- GPT-3와 T5 같은 모델들이 최신 정보와 관련된 질문에도 대답할 수 있는지를 검증하는 시스템을 구축했습니다. CNN 등 믿을만한 원천에서 최신 정보를 검색한 후 이를 질문 프롬프트에 삽입하면, pretraining 단계에서 포함되어있지 않은 내용에 대해서도 대답할 수 있음을 밝히고 있습니다. 
- 2022.07.27에 최초 제출된 논문이라, 지금와서는 조금 당연한 내용을 다루고 있는 것으로 보입니다. 이 시기에 지속적으로 업데이트되는 벤치마크를 구축하려고 노력한 것은 의미 있어 보이네요. 벤치마크의 마지막 업데이트는 2023년 12월 9일입니다.

흥미로운 점
- 모델이 '해당 사항 없음(None of the Above)'이라고 답해야 하는 상황을 인식하지 못하는 문제를 다루고 있습니다. 대답에 필요한 데이터가 없을 경우 질문에 "모른다"고 답하지 않고 어떻게든 답변을 시도하는 현상은 GPT4에서도 여전히 자주 발생합니다.
- 이는 "Lessons after a half-billion GPT tokens"에서 언급된 "GPT is really bad at producing the null hypothesis"라는 문제와 유사해 보입니다. (https://kenkantzer.com/lessons-after-a-half-billion-gpt-tokens/)
- GPT3가 처음 나왔을 때에는 이런 환각 현상이 심했는데, GPT4 및 동급의 모델들에서는 그래도 전보다 개선되었습니다. 아마 fine-tuning 단계에서 instruction dataset을 따로 구축한 것이 아닌가 싶습니다.
- LLM이 "무엇을 모르는지 정확히 알 때", AGI의 초기 단계에 진입한다고 생각합니다. 이를 위해서는 LLM에게 지식을 더 주입시키기 보다는, 무엇을 모르는지를 가르치는 게 더 중요해 보이네요.