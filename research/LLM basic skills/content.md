[LinkedIn](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_laying-the-foundation-first-investigating-activity-7283202800036028416-c6WR?utm_source=share&utm_medium=member_desktop&rcm=ACoAADfxcywBkH2Mi2-YPZm7jSZERa3dQ2_DDEY)
[Medium](https://medium.com/@simple0314/llm-%EA%B8%B0%EC%B4%88-%EC%97%AD%EB%9F%89%EB%B6%80%ED%84%B0-%EA%B0%80%EB%A5%B4%EC%B3%90%EC%95%BC-%ED%95%A0%EA%B9%8C-c1d2307b81f6)
## LLM, 기초 역량부터 가르쳐야 할까? 

LLM이 전문가처럼 대화하다가도 엉뚱한 실수를 하는 걸 보면, 신뢰하기 어렵다는 생각이 들 때가 있습니다. 간단한 계산을 틀리거나, 요약을 어설프게 하는 모습에 'LLM이 정말 기본적인 건 알고 있나?'라는 의문이 들기도 하죠. 흔히 LLM은 방대한 인터넷 데이터를 학습하면서 자연스럽게 언어와 세상에 대한 기초 지식을 쌓는다고 생각하지만, 과연 그럴까요? 

이러한 의문에서 출발해, LLM의 기초 역량 학습과 평가에 대한 몇 가지 연구들을 찾아봤습니다. 복잡한 과제 중심의 LLM 성능 평가에 가려져 주목받지 못했던, '기초 역량'과 관련된 연구들입니다. Compositional generalization, atomic/fundamental/basic/foundation skill 등의 키워드를 중심으로, 관련 연구들을 간단히 소개합니다.

1. Laying the Foundation First? Investigating the Generalization from Atomic Skills to Complex Reasoning Tasks
https://arxiv.org/abs/2403.09479
LLM이 복잡한 추론 과제를 해결할 때 기초 능력(atomic skills)을 일반화(generalization)하여 적용하는 데 어려움을 겪는다는 문제의식에서 출발합니다. 이를 검증하기 위해 산술 및 단위 변환이라는 두 가지 기초 능력 훈련(skill training)을 진행한 후, 해당 능력이 복합 추론 과제(수학 문제 해결, MWP)에 자발적으로 전이되는지 확인하는 실험을 설계했습니다. 
실험 결과, 기초 능력이 자발적으로는 복합 추론 과제에 전이되지 않음을 확인했고, 이를 해결하기 위해 계층적 커리큘럼 학습(HCL) 전략을 제안했습니다. HCL은 기초 능력 훈련과 응용 학습(applied learning)의 두 단계로 구성되며, 응용 학습 단계를 통해 기초 능력을 복합 과제에 적용하도록 유도했습니다. 그 결과, LLaMA-2 모델에서 HCL을 적용했을 때, GSM8k를 더 어렵게 보강한 벤치마크에서 zero-shot 정확도가 13.60%에서 28.76%로 향상되었으며, 기초적인 실수를 극적으로 줄였습니다.

1. SKILL-MIX: A FLEXIBLE AND EXPANDABLE FAMILY OF EVALUATIONS FOR AI MODELS
https://arxiv.org/pdf/2310.17567
LLM이 다목적 AI 에이전트로 진화하면서, 단순히 여러 기술을 암기하는 것에서 나아가 필요한 기술을 조합하는 능력이 중요해졌습니다. 이를 측정하기 위해 제시된 벤치마크 SKILL-MIX는, LLM에게 무작위로 선택된 k개의 언어 능력들(비유, 모순, 인과관계 등)을 조합하여 특정 주제(정원 가꾸기, 결투 등)에 대한 짧은 글을 생성하도록 요구합니다. k가 증가할수록 과제의 난이도가 높아진다고 여겨집니다. 
실험 결과, GPT-4는 k=5일 때에도 상당한 성능을 보였지만, 다른 모델들은 대부분 k=3에서 성능이 급격히 저하되어 LLM의 기술 조합 능력이 모델 크기에 따라 차이를 보임을 확인했습니다. 또한, 일부 공개된 모델이 리더보드 순위와 달리 SKILL-MIX에서 낮은 성능을 보여, 리더보드 점수를 위한 학습("cramming for the leaderboard")이 일반적인 역량에 부정적인 영향을 미칠 수 있음을 시사했습니다.

1. Skill-it! A Data-Driven Skills Framework for Understanding and Training Language Models
https://arxiv.org/pdf/2307.14430
학습 데이터의 퀄리티가 LLM의 성능에 큰 영향을 미친다는 전제 하에, 어떤 데이터를 선택해야 모델의 성능을 극대화할 수 있는지 연구합니다. 인간이 상호의존적인 스킬들을 순서대로 배우는 것처럼, LLM도 학습 데이터로부터 스킬들을 순서대로 학습한다는 가설을 세웁니다. '스페인어 질문 생성'을 배우기 전에 '스페인어 문법'과 '영문 질문 생성'에 대해서 배워야 된다는 말입니다.
이를 바탕으로 스킬의 개념과 스킬 간의 선후 관계를 정의하고, 이를 이용한 온라인 데이터 샘플링 알고리즘인 SKILL-IT을 제안합니다. SKILL-IT은 continual pre-training과 fine-tuning 환경에서 모두 효과적이며, 특히 LEGO 데이터셋의 지속적 사전 학습 환경에서는 무작위 샘플링 대비 36.5%p의 정확도 향상을 보였습니다. 또한, Natural Instructions 데이터셋의 미세 조정 환경에서는 타겟 스킬 관련 데이터로만 학습하는 것 대비 최대 13.6% 낮은 validation loss을 달성했습니다.

1. Skills-in-Context: Unlocking Compositionality in Large Language Models
https://arxiv.org/pdf/2308.00304
LLM이 복잡한 문제를 해결하는 과정에서 간단한 기술을 결합하는 데 어려움을 겪는 현상을 해결하기 위해 "Skills-in-Context (SKiC)" 프롬프트 구조를 제안합니다. 프롬프트 내에 기초 기술(skills)과 그 기술들을 활용한 조합 예시(compositional examples)를 함께 제공합니다. 
실험 결과 SKiC는 특히 out-of-distribution 평가에서 LLM의 성능을 크게 향상시켰는데, 예를 들어 마지막 글자 이어붙이기 과제에서 text-davinci-003 모델의 정확도를 최대 68.9%p까지 끌어올렸습니다. 또한, SKiC는 LLM이 주어진 기술 외에도 사전 학습된 지식을 활용하도록 유도하여, 더 복잡한 추론 과제에서도 효과를 보였습니다. 
