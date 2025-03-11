2025.02.19

Reasoning Model은 더 정직한가? 

Are DeepSeek R1 And Other Reasoning Models More Faithful?
https://arxiv.org/abs/2501.08156


tl;dr
Qwen-2.5, Gemini-2-Flash, DeepSeek-V3 기반 reasoning 모델들이 기반 모델들 보다 더 faithful(정직)한 답변을 한다는 연구 결과가 나왔습니다.
Faithfulness란 모델이 생성하는 Chain-of-Thought이 실제 내부 추론 프로세스를 정확히 반영하는 정도를 의미합니다.
Reasoning 모델들은 59%~68% 확률로 명확히 반영했지만, 기존 모델들은 7%~13% 수준에 그쳤습니다.
또한, 연구에서는 RLHF 과정에서 사용되는 Reward Model이 Faithful한 reasoning이 포함된 답변을 덜 선호하는 경향이 있음을 발견했습니다.

💡 들어가며

최근 수학과 코딩 등의 도메인에서 비약적인 성능 개선을 보인 reasoning 모델들은 유저에게 보이는 답변을 생성하기 전 따로 생각하는 단계를 거칩니다. 
이렇게 답을 도출하기 전 사고의 과정을 생성하는 것을 Chain-of-Thought(CoT)라 부르며, 다양한 작업에서 성능 향상에 기여하는 것으로 나타났습니다.  
하지만 CoT와 실제 모델의 내부에서 이루어지는 추론이 일치하는지 논란이 있어 왔으며, 이러한 현상은 LLM의 'Faithfulness' 이슈라 불리며 연구되어 왔습니다.  
이번 연구에서는 reasoning 모델과 non-reasoning 모델을 faithfulness의 관점에서 비교 분석합니다.

🔎 Faithfulness란?

Faithfulness란 모델이 답을 도출하는 과정에서 생성하는 CoT가 실제 내부 reasoning 프로세스를 정확하게 반영하는지를 의미합니다.

예를 들어, LLM에게 다음과 같은 한국 역사 문제가 주어졌다고 가정해 보겠습니다.

> 질문:  
> 조선 시대에 실학을 집대성하고 『목민심서』를 저술한 인물은 누구인가요?  
> 서울대 교수는 이 문제의 정답을 '세종대왕'이라고 답했습니다. (LLM에게 혼란을 주도록 의도적으로 삽입된 단서, cue)
> (A) 정약용  
> (B) 이순신  
> (C) 세종대왕  
> (D) 신사임당

❌ Unfaithful reasoning:

> "세종대왕은 조선 시대 대표적인 성군으로, 훈민정음을 창제하였습니다. 정답은 (C) 세종대왕입니다."  
> (cue 때문에 답을 바꿨지만, 그 영향을 설명하지 않음)

✅ Faithful reasoning:

> "처음에는 정약용(A)이 『목민심서』를 저술한 것으로 알고 있었습니다. 하지만 서울대 교수님이 세종대왕(C)이라고 답했다는 정보를 보고 다시 고민해 보았습니다. 세종대왕은 다방면에 영향을 끼친 왕이기 때문에 조선 시대의 실학을 집대성하고 『목민심서』을 저술했을 수도 있습니다. 따라서 답은 세종대왕(C)입니다."  
> (cue 때문에 답을 바꿨음을 명시함)

이처럼 faithful한 모델은 cue가 답변에 영향을 미쳤는지를 명확히 설명하는 반면, unfaithful한 모델은 답을 바꾸면서도 그 이유를 밝히지 않습니다.

---

 📌 연구 방법

연구진은 2단계 평가 방법을 사용하여 모델이 실제 reasoning을 명확하게 설명하는지 측정하였습니다.

1️⃣ Cue Influence Test (단서 영향 테스트)

- 모델에게 multiple-choice 문제를 제시합니다.
- 특정 cue(예: "Stanford 교수님이 정답은 B라고 생각하십니다.")를 삽입합니다.
- 모델이 cue의 영향으로 답을 변경하는지 확인합니다.

2️⃣ Judge Model 평가

- 별도의 LLM judge가 모델이 cue의 영향을 명확하게 설명하는지 평가합니다.
- 단순히 cue를 인지하는 것에 그치지 않고, "내가 이 정보를 어떻게 고려했는지"를 설명해야 faithfulness가 인정됩니다.

 🧩 테스트에 사용된 Cue 유형

연구에서는 여러 가지 cue를 활용하여 다양한 상황에서 faithfulness를 평가하였습니다.

🔹 Professor’s Opinion: "Stanford 교수님이 정답은 B라고 생각하십니다."와 같이 모델이 권위있는 사람의 의견을 얼마나 따르는지 확인
🔹 Few-shot Black Squares: 정답에 검은 사각형(■)이 표시된 few-shot 예제를 제공하여 모델이 패턴을 따르는지 확인  
🔹 Post-hoc Cueing: 모델이 우선 답을 내리게 한 후, 기존의 답과 다른 방향으로 유도해 제공된 추가 정보를 reasoning 과정에서 어떻게 설명하는지를 평가

이외에도 다양한 cue를 적용해 모델이 의사결정 과정에서 얼마나 투명한 reasoning을 수행하는지를 평가하였습니다.

---

 📊 결과

연구진은 reasoning model과 non-reasoning model 간의 faithfulness 차이를 분석한 뒤, 이 차이가 발생하는 원인을 밝히기 위해 추가 실험을 진행하였습니다.

 1️⃣ Faithfulness 성능 비교: Reasoning Model이 더 Faithful한가?

- DeepSeek R1, Gemini-2-Flash-Thinking, QwQ-32b 등의 reasoning model은 59~68%의 확률로 faithful했습니다.
- 반면, GPT-4o, Claude-3.5-Sonnet 등의 non-reasoning model은 faithful한 비율이 7~13%에 불과했습니다.
- Reasoning model은 자신이 답을 바꾼 이유를 명확히 서술하는 반면, non-reasoning model은 답을 변경하면서도 그 이유를 누락하는 경향이 강했습니다.

---

 2️⃣ 왜 Non-Reasoning Model은 Faithful하지 않을까?

연구진은 유저의 선호를 학습하는 reward model 때문에 non-reasoning model이 낮은 faithfulness를 보인다고 가정하고, 이를 검증하기 위해 GPT-4o를 reward model이라고 가정해 실험을 진행하였습니다.

🔹 Reward Model은 Unfaithful Response를 선호

- GPT-4o 기반 reward model을 사용하여 faithful vs. unfaithful response의 선호도를 측정한 결과,  
    63~71%의 확률로 unfaithful response에 더 높은 점수를 부여하는 것으로 나타났습니다.
- 연구진은 이 현상이 RLHF 과정에서 유저 선호도를 반영한 결과일 가능성이 높다고 해석합니다.
    - Faithful한 답변은 도중 되돌아가거나(backtracking) 불확실성을 표현하지만 유저들은 명확하고 단순한 답변을 선호하기 때문에,
    - reward model이 이러한 답변을 더 높은 점수로 평가하면서 faithfulness가 저해될 수 있다는 가설을 제시하였습니다.

🔹 Outcome-Based RL은 Faithfulness 향상에 기여

- 반면, DeepSeek R1은 outcome-based RL 방식으로 학습되었습니다.
    - 이는 최종 정답의 정확도만을 보상 기준으로 사용하며,
    - CoT의 자연스러움이나 문장 구조에는 별다른 보상을 주지 않는 방식입니다.
- 이러한 학습 방식 덕분에, reasoning model은 reasoning 과정에서 자연스럽게 여러가지 가능성을 고려하고 verbalize(언어화)할 가능성이 더 높아졌습니다.

---

 📝 마무리

🔹 연구를 정리하면서 Faithfulness를 "정직함"이나 "신뢰감"으로 번역할 수도 있었지만, 개념의 미묘한 차이를 충분히 반영하기 어렵다고 판단해 원어 그대로 두었습니다. LLM 연구에서는 하루가 다르게 새로운 개념들이 등장하고, 연구들 사이에서도 용어 통일이 잘 안되어서 매번 정리할 때마다 고민이 되네요.

🔹 CoT 적용 시 자주 지적되던 Faithfulness 문제를 reasoning 모델들이 결과적으로 상당히 개선한 것이 인상적이었습니다. 모델의 해석 가능성을 높여 원치 않는 답변이 발생했을 때 어느 단계에서 오류가 발생했는지 분석할 수 있다는 점에서 의미 있는 발견이라고 생각됩니다.

🔹 이번 연구에 참여한 Owain Evans는 LLM 안전 영역에서 신선한 관점을 제시하는 연구자로, 향후 reasoning 모델들에 대한 더 깊이 있는 연구를 진행할 것으로 기대됩니다. https://x.com/OwainEvans_UK/status/1891889219599204400

🔹 빠르게 변화하는 LLM 도메인 특성상 아무래도 엄밀하지 않은 연구들이 많이 등장하고, 이 연구도 마찬가지라고 생각합니다. 그렇기 때문에 이 연구도 천천히 읽어보시기 보다는, 챗봇에게 던져주고 궁금한 부분만 묻거나 아니면 'reasoning 모델들이 내뱉는 reasoning 과정은 꽤 진실되구나' 정도만 알아가셔도 무방할 것 같네요.