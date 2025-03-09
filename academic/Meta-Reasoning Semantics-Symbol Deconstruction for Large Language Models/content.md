[LinkedIn](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_llm%EC%97%90%EA%B2%8C-%EC%89%BD%EA%B2%8C%EB%A7%90%ED%95%98%EA%B8%B0-%EC%95%88%EB%85%95%ED%95%98%EC%84%B8%EC%9A%94-%EC%9E%85%EB%A0%A5-%ED%94%84%EB%A1%AC%ED%94%84%ED%8A%B8%EB%A5%BC-%EA%B0%84%EC%86%8C%ED%99%94%ED%95%B4-llm%EC%9D%98-activity-7207057037820649472-521u?utm_source=share&utm_medium=member_desktop)

<LLM에게 쉽게 말하기>  
  
안녕하세요, 입력 프롬프트를 간소화해 LLM의 추론 성능을 개선하는 연구에 대해 소개하려고 합니다.  

Meta-Reasoning: Semantics-Symbol Deconstruction for Large Language Models  
[https://lnkd.in/dxDrgtgH](https://lnkd.in/dxDrgtgH)

1. 접근 방식
- Meta-Reasoning은 질문에서 추론에 불필요한 정보를 제거하고, 추론에 필요한 핵심 내용만을 남겨 기호적 표현으로 변환하는 방법입니다. 이를 통해 언어 모델이 다양한 시나리오에서 보다 일반화된 추론 패턴을 학습할 수 있게 됩니다.
- 구체적으로, Meta-Reasoning은 질문 내 개체(Entity)와 연산(Operation)을 추출해 기호로 변환하는 과정인 'Semantic Resolution'을 수행합니다. 이 과정에서 개체는 고유한 기호(예: A, B, C)로, 연산은 할당, 논리 연결, 산술 연산 등을 나타내는 기호(예: =, →, +, -, ×, ÷)로 매핑됩니다.
- Meta-Reasoning에는 두 가지 버전이 있습니다: (1) Completely-serial: 전체 질문에 대해 Semantic Resolution 후 Chain-of-Thought(CoT) 수행, (2) Crossly-serial: 질문을 여러 하위 단계로 분할하고, 각 단계에서 Semantic Resolution과 CoT를 순차적으로 수행.
- 제안된 방법의 효과를 검증하기 위해, 기존의 CoT 프롬프팅과 비교했습니다. 실험은 산술/기호/논리와 같은 일반 추론과, 다수의 agent가 참여하는 상호작용 추론(theory-of-mind 사용)으로 나누어 총 10개 이상의 데이터셋에서 수행되었습니다.

1. 주요 결과
- 일반적인 추론에서 Meta-Reasoning은 CoT 대비 적은 예시로 평균 20%의 성능 향상을 보였습니다. 
- Out-of-domain(OOD) 일반화 능력을 평가하기 위해 설계된 boundary test에서, Meta-Reasoning은 훈련 예시보다 더 깊은 추론을 요구하는 문제들에 대해서도 높은 정확도를 유지했습니다. 반면, CoT의 성능은 추론 단계가 깊어짐에 따라 급격히 저하되었습니다.
- Meta-Reasoning의 출력 토큰 수 분포는 CoT에 비해 훨씬 더 집중되어 있었습니다. 이는 Meta-Reasoning이 출력 안정성을 향상시켜 예기치 않은 상황을 줄이고 사용자 경험을 개선하는 데 도움이 될 수 있음을 의미합니다.
- 상호작용 추론에서도 Meta-Reasoning은 CoT를 크게 능가했습니다. CoT는 복잡한 추론에서 급격한 성능 저하를 보인 반면, Meta-Reasoning은 추론에 필요한 정보가 증가해도 안정적인 성능을 유지했습니다. 

1. 메모
- 쉽게 말하면 "질문의 핵심 내용만 간추려서 답하기"입니다. 간단해 보이지만 성능이 월등한 것을 보면, LLM에게 있어 일반적인 자연어 데이터는 '다룰 수 있는' 형식의 데이터일 뿐이지, '최적의 성능을 위한' 형식은 아닙니다.
- 본 연구는 자연어 형식을 갖춘 기호 표현을 사용하는 것이 기존의 Python, SQL 등을 사용하는 접근 방식과 차별화되는 점이라고 주장합니다. 그러나 이러한 차이점과 각 접근 방식의 장단점에 대해 심도 있게 다루지 않았습니다. 또한 추가적인 기호 활용 LLM 연구(예: Logic LM)와의 비교 분석이 이루어지지 않아서 아쉽습니다.
- Meta-Reasoning은 자연어 프롬프트를 간소화하기 위해 입력 프롬프트에 예시를 추가합니다. CoT와 비교했을 때 예시 개수가 적어도 성능이 높지만, 전체 토큰 수가 적은지는 언급이 없습니다. 기존의 Many-shot ICL 등과 같은 유의미한 연구들이 프롬프트 안에 많은 예시들을 필요로 하는 것을 보면, 적어도 연구 단계에서는 높은 토큰 비용이 전제될 것으로 보입니다.  
- 그러나 프롬프트에 들어가는 예시들이 어떻게 선정되었는지를 설명하는 연구들은 부족합니다. LLM이 입력 프롬프트의 내용/순서/형식 등에 민감하게 반응한다는 것(prompt consistency)이 넓리 알려져 있는데, 연구들에서 '프롬프트 내 예시들은 사전에 전문가들의 도움으로 작성됨'과 같이 언급하고 넘어간다면 제시한 기법들을 다른 분야에서 재현하기가 어렵습니다.
- 특히 본 연구에서는 semantic resolution(자연어 간소화 과정)에서 활용되는 프롬프트 내 예시 선정 방식은 핵심 요소임에도 불구하고, 자세한 논의가 이루어지지 않았습니다. 다른 벤치마크/작업에 활용하기 위해 적절한 예시를 찾는 과정에 지나치게 많은 비용이 발생한다면 실용적이지 못하겠죠.
- Meta-Reasoning 과정에서 발생하는 오류는 벤치마크에 따라 다른 양상으로 보이고 있습니다. MultiArith나 AddSub와 같은 수학 문제에서는 semantic resolution 단계에서, 그 외의 기호/논리 문제에서는 순수하게 추론 성능에서 오류를 보이고 있습니다. 다만 전반적인 통계 수치는 제공하고 있지만, 구체적인 오류 사례와 그에 대한 개선 방안은 다루고 있지 않아 아쉽네요.
- 저자들이 제안한 completely-serial과 crossly-serial에 대해, 동일 벤치마크에서의 비교 결과가 누락되어 있습니다. 