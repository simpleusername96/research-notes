[Medium](https://medium.com/@simple0314/llm%EA%B3%BC-%EA%B8%B0%ED%98%B8-c7caf48143dd)

Introduction
자연어로 된 프롬프트를 활용해 Large Language Model(LLM)의 출력을 원하는 방향으로 조정하는 과정을 프롬프트 엔지니어링이라고 합니다. 하지만 인간의 언어는 본질적으로 모호하기 때문에, 의미를 명확히 전달하는 데 한계가 있습니다. 이로 인해 실제로 사용자의 의도대로 LLM을 제어하기 위해서는 수많은 시행착오와 운이 필요한 상황입니다. 더욱이 인간의 관점에서는 사소한 차이로 여겨질 수 있는 프롬프트의 변화도 LLM에게는 큰 영향을 미칠 수 있기 때문에, 프롬프트 엔지니어링은 일반화하기 어려운 분야로 여겨집니다1).


이러한 프롬프트 엔지니어링의 어려움을 해결하기 위해 매일같이 새로운 연구들이 발표되고 있습니다. 그 중 가장 주목할 만한 연구들로는 다음과 같은 것들이 있습니다:

1. Chain-of-Thought Prompting Elicits Reasoning in Large Language Models
 (https://arxiv.org/abs/2201.11903): 해결하고자 하는 문제와 유사한 예시 문제 및 그 해결 과정과 답을 함께 제공해 LLM의 추론 능력을 향상시키는 방법입니다.
2. Large Language Models Are Human-Level Prompt Engineers
 (https://arxiv.org/abs/2211.01910): LLM 스스로 프롬프트 엔지니어링을 수행하도록 하는 방법입니다.
3. Self-Consistency (https://arxiv.org/abs/2203.11171): 주어진 문제에 대해 LLM이 생성한 다양한 답변 중 가장 빈번하게 등장하는 답변을 최종 출력으로 선택하는 방법입니다.

최근에는 LLM의 context window 크기가 2M 이상으로 확대되면서, Many-Shot In-Context Learning(https://arxiv.org/abs/2404.11018)과 같은 방법론도 주목받고 있습니다.

하지만 이외의 연구들 중 상당수는 실제로 새로운 아이디어를 제시하기보다는 기존 아이디어를 약간 변형하거나 조합하는 수준에 그치는 경우가 많았습니다. 또한 성능 검증에 사용된 벤치마크 점수의 개선폭이 작거나 벤치마크 자체의 신뢰도 문제로 인해, 제안된 방법론의 실효성에 의문이 제기되기도 했습니다. 이로 인해 프롬프트 엔지니어링 관련 연구에 대한 관심이 다소 줄어들었던 것이 사실입니다.

그러던 중 LinkedIn에서 "Faithful Logical Reasoning via Symbolic Chain-of-Thought"라는 논문을 접하게 되었고, 이 논문에서 제안한 SymbCot이라는 기법이 기존의 CoT 방법론보다 추론 벤치마크에서 월등히 우수한 성능을 보인다는 것을 알게 되었습니다.

SymbCot의 핵심 아이디어는 자연어로 표현된 문제 상황을 First-Order Logic(FOL) 같은 기호 문법으로 변환하고, 계획-해결-검증의 단계를 거치는 것입니다. 사실 이러한 접근 방식 자체는 완전히 새로운 것은 아니었고, 기존 연구에서도 유사한 시도가 있었습니다. 하지만 해당 연구를 계기로 기호적 표현을 활용해 LLM의 추론 능력을 향상시키려는 일련의 연구 흐름에 대해 보다 깊이 있게 알아보게 되었고, 이에 대한 다양한 생각을 정리해보고자 합니다.

다음은 LLM의 성능 개선을 위해 체계적 언어 체계인 '기호(symbol)'을 활용한 연구들의 목록입니다.

### SymbCoT (Symbolic Chain-of-Thought)
Faithful Logical Reasoning via Symbolic Chain-of-Thought
https://arxiv.org/abs/2405.18357

접근 방식
- Translator - Planner - Solver - Verifier 
- Translator 모듈은 자연어로 된 전제와 질문을 First-Order Logic (FOL) 또는 Constraint Optimization (CO) 기호 표현으로 변환하여 구조화된 논리적 추론을 준비합니다.
- Planner 모듈은 문제를 작은 하위 문제로 분할하고, 전제에서 질문으로 이어지는 추론 과정을 자연어와 기호로 단계별로 계획합니다.
- Solver 모듈은 계획에 따라 순차적 논리 추론을 통해 답을 도출합니다. 관련 전제를 선택하고, 기호 추론 규칙(예: modus ponens, modus tollens)을 기반으로 추론합니다.
- Verifier 모듈은 두 가지 역할을 합니다:
a) 기호 번역과 원래 자연어의 의미적 동등성을 검증하여 정확성을 보장합니다.
b) Solver의 단계별 논리 추론을 검증하여 타당한 논리 규칙을 따르는지 확인합니다. 만약 논리적 오류가 발견되면, Verifier는 올바른 논리 규칙을 사용하여 추론을 수정합니다. 이를 통해 정제된 답을 도출할 수 있습니다.

주요 발견
- SymbCoT는 기존 Chain-of-Thought (CoT) 프롬프팅보다 논리적 추론 능력을 크게 향상시킵니다. GPT-3.5와 GPT-4로 5개 데이터셋에서 최신 성능을 능가합니다.
- 논리 추론 과제가 복잡할수록 SymbCoT의 개선 효과가 두드러지며, 검증 메커니즘이 추론 과정의 신뢰성(faithfulness, 추론 과정에 부합한 답을 도출하는지 여부)을 보장합니다.
- 기호와 자연어 표현을 결합하여 명확한 논리 계산이 가능하고, 함축된 정보와 풍부한 맥락을 완전히 해석할 수 있습니다.
- Plan-then-solve 아키텍처와 사후 검증이 CoT보다 추론 과정의 신뢰성을 높입니다.
- SymbCoT는 번역 오류에 강인하며, 외부 추론기(external solver)에 의존하는 방법보다 이해하기 쉬운 설명을 제공하고, 문제해결에 필요한 정보를 충분히 활용합니다.


### LOGIC-LM
Logic-LM: Empowering Large Language Models with Symbolic Solvers for Faithful Logical Reasoning
https://arxiv.org/abs/2305.12295

접근 방식
- Translator 모듈은 과제별로 적절한 기호 표현(FOL, CSP, SAT 등)을 사용하여 자연어 문제와 질문을 기호로 변환하여 기호 해결기(symbolic solver)가 논리적 추론을 할 수 있도록 준비합니다.
- Symbolic Reasoner 모듈은 과제별 기호 해결기를 사용하여 생성된 기호 표현에서 논리적 추론을 수행합니다. 이 때 사용되는 기호 해결기는 LLM과 달리 결정론적(deterministic)으로 답을 도출합니다.
- Result Interpreter 모듈은 기호 해결기가 반환한 기호로 표현된 답을 자연어로 번역합니다. 이는 사전 정의된 규칙이나 LLM 기반 해석기를 통해 이루어집니다.
- Self-Refiner 모듈은 기호 해결기의 오류 메시지를 피드백으로 사용하여 기호 표현을 반복적으로 개선합니다. 잘못된 기호 형식, 오류 메시지, 일반적인 오류 사례와 해결책을 LLM에 제공하여 수정된 표현을 생성하도록 유도합니다.

주요 발견
- LOGIC-LM은 5개 논리 추론 데이터셋에서 기본 프롬프트와 CoT 프롬프트보다 GPT-3.5로는 평균 39.2%, 18.4%, GPT-4로는 24.98%, 10.44% 더 높은 성능을 보입니다.
- LLM은 합성 문제(synthetic problem)를 실행 가능한 기호 형식으로 번역하는 데 뛰어나지만, 복잡한 실제 문제를 논리 형식으로 변환하는 데 어려움을 겪습니다.
- LOGIC-LM의 장점은 깊이 있는 추론 문제에서 두드러집니다. 다단계 추론을 강력한 기호 해결기에 맡기고, LLM은 문제를 기호로 표현하기만 하면 되기 때문입니다.
- 기호 해결기가 제공하는 오류 메시지를 활용한 자기 개선은 실행 불가능한 기호 형식을 수정하는 데 효과적이며 전반적으로 정확도(accuracy)를 개선하지만, 적절한 기호 문법으로 번역돼 기호 번역기에서 실행은 가능하지만 잘못된 의미로 번역되는 경우도 있으므로 더 나은 피드백 방식이 필요합니다.
- 오류 분석 결과, GPT-4도 여전히 적절한 술어를 일관되게 정의하고, 표현을 정확히 해석하며, 기호 표현 생성 시 FOL 문법을 완전히 이해하는 데 어려움을 겪습니다. 이는 기호 문법의 복잡함과 자연어의 유연함 때문입니다.

## SATLM (Satisfiability-Aided Language Models Using Declarative Prompting)
SatLM: Satisfiability-Aided Language Models Using Declarative Prompting
https://arxiv.org/abs/2305.09656

접근 방식
- 자연어로 표현된 추론 문제를 해결하기 위해 LLM을 활용하여 문제를 선언적 형식(declarative form)의 논리 제약 조건(logical constraint) 집합으로 파싱합니다. 
- 파싱된 논리 제약 조건을 기반으로 SAT 해결기를 활용하여 다단계 추론을 계획하고 실행함으로써 최종 답안의 정확성을 보장합니다. SAT 해결기는 논리 제약 조건의 충족 가능성(satisfiability)을 판단하고, 가능한 해를 찾아내는 자동화된 정리 증명 도구입니다.
- 이러한 접근 방식은 기존의 Chain-of-Thought 이나 PAL(program-aided language, 후술 예정)과 달리, LLM이 추론 과정 전체를 명시적으로 생성하는 대신 선언적 문제 명세에 집중하도록 유도합니다.

주요 발견
- SATLM은 산술 추론, 논리 추론, 기호 추론 등 다양한 유형의 추론 작업을 포함하는 8개의 데이터셋에서 기존의 연쇄적 사고 프롬프트와 프로그램 보조 언어 모델보다 우수한 성능을 보여주었습니다. 특히 GSM-SYS 데이터셋에서는 23%의 성능 향상을 보였으며, LSAT와 BOARDGAMEQA 데이터셋에서는 기존 모델의 최고 성능을 갱신했습니다.
- 선언적 프롬프팅을 통해 LLM이 추론 과정을 직접 생성하는 대신 문제를 선언적 형식으로 파싱하도록 유도하는 것이 전반적인 성능 향상에 도움이 되는 것으로 나타났습니다. 이는 선언적 형식이 자연어 문제 서술과 더 가깝기 때문에 LLM이 문제를 더 잘 이해하고 핵심 정보를 추출할 수 있기 때문입니다. 실제로 저자들은 로그 우도(log likelihood)를 통해 선언적 형식으로 파싱하는 것이 명령형 해결 절차를 생성하는 것보다 LLM에게 더 자연스러운 작업임을 확인했습니다.
- SAT 해결기를 활용함으로써 파싱된 문제가 불가능(unsatisfiable)하거나 모호한(ambiguous) 경우를 탐지하고, 그에 따른 오류 메시지를 출력할 수 있습니다. 이를 통해 SATLM은 잘못된 예측을 하지 않고 문제를 풀 수 없는 상황을 인지할 수 있게 됩니다. 실험 결과 SATLM이 기준 모델에 비해 잘못된 예측을 더 효과적으로 피할 수 있음을 확인했습니다.
- 제안된 접근 방식은 다양한 언어 모델(code-davinci-002, gpt-3.5-turbo, text-davinci-003)에 적용 가능하며, 각 언어 모델의 고유한 특성과 관계없이 우수한 성능을 보여주었습니다. 또한 few-shot example의 선택에 크게 영향을 받지 않는 것으로 나타나, 접근 방식의 일반화 가능성을 확인할 수 있었습니다.

### LINC (Logical Inference via Neurosymbolic Computation)
LINC: A Neurosymbolic Approach for Logical Reasoning by Combining Language Models with First-Order Logic Provers
https://arxiv.org/abs/2310.15164

접근 방식
- LINC(Logical Inference via Neurosymbolic Computation)은 신경망과 기호 추론을 결합한 접근 방식으로, 논리 추론 작업을 두 단계로 해결합니다:
	- 언어 모델이 의미 파서(semantic parser) 역할을 하여 전제와 결론을 자연어에서 일차 논리(FOL) 표현으로 번역합니다.
	- FOL 표현은 기성 자동 정리 증명기(automated theorem prover)인 Prover9에 전달되어, 주어진 전제를 바탕으로 결론의 진리치를 연역적으로 추론합니다.
- 실제로 LINC는 성능 향상을 위해 K-way 다수결 투표 단계를 추가로 도입하였는데, 여기서 K개의 독립동일분포(i.i.d.) 샘플을 모델에서 추출하고 최빈값을 최종 예측으로 사용합니다.
- LINC는 세 가지 기준 전략과 비교됩니다: Naïve(직접 정답 예측), Scratchpad(FOL 표현 생성 후 정답 예측), Chain-of-Thought 프롬프팅(단계별 자연어 추론).
- 실험은 FOLIO와 ProofWriter 데이터셋의 균형 잡힌 하위 집합에 대해 세 가지 언어 모델(StarCoder+ (15.5B 파라미터), GPT-3.5, GPT-4)을 사용하여 수행되었습니다.

주요 발견
- LINC는 거의 모든 실험 환경에서 기준 모델들에 비해 유의미한 성능 향상을 보였습니다. 단, FOLIO에서의 GPT-4의 경우 차이가 통계적으로 유의하지 않았습니다.
- ProofWriter에서 LINC는 모델들이 few-shot 예시에 제시된 것보다 더 큰 전제 집합에 대한 추론으로 일반화할 수 있게 해주었습니다. 필요한 증명 깊이가 깊어져도 성능이 높게 유지된 반면, 기준 모델들은 어려움을 겪었습니다.
- 정성적 오류 분석 결과, LINC와 Chain-of-Thought는 서로 다른 실패 양상을 보였습니다:
	- LINC는 FOL 변환 시 암시적 정보 포착, 손실이 있는 표현, 구문 오류 등의 어려움을 겪었습니다.
	- Chain-of-Thought는 추론이 암시하는 바와 반대되는 결론 도출, 잘못된 논리적 추론, 복잡한 추론 경로 발견 실패 등의 실수를 범했습니다.
- 정량 분석 결과, Chain-of-Thought에 비해 LINC는 True/False 예측에서 재현율(recall)은 낮지만 정밀도(precision)는 더 높았습니다. 두 방법은 또한 상당 부분 다른 예시 하위 집합에서 오예측을 보였습니다.
- LINC는 다른 맥락 학습(in-context learning) 기법들보다 기저 모델의 오류 패턴을 더 크게 변화시켰는데, 이는 LINC의 오예측과 기준 모델 오예측 간 유사도가 더 낮은 것으로 확인되었습니다.
- LINC의 효과는 다양한 언어 모델(StarCoder+, GPT-3.5, GPT-4)에 걸쳐 견고했으며, few-shot 예시 선택에 상대적으로 둔감했습니다.

### PAL (Program-Aided Language Models)
PAL: Program-aided Language Models
https://arxiv.org/abs/2211.10435

접근 방식
- PAL은 자연어 문제를 읽고 중간 추론 단계로 프로그램을 생성하도록 LLM을 사용하지만, 실제 계산과 일부 추론은 외부 Python 인터프리터에 맡깁니다.
- PAL은 자연어 문제를 이해하고 프로그램 단계로 분해하는 데는 LLM을 활용하지만, 생성된 프로그램을 실행하여 최종 답을 얻는 역할은 Python 인터프리터가 수행합니다.
- PAL은 변수 이름에 의미 있는 이름을 사용하여 LLM이 변수와 문제의 개체를 더 잘 연결할 수 있도록 합니다.

주요 발견
- PAL은 BIG-Bench Hard 및 기타 벤치마크의 13개 작업에서 프로그램 생성과 Python 인터프리터 실행을 통해 COT를 사용하는 PaLM-540B와 같은 더 큰 LLM보다 뛰어난 성능을 보이며 최신 SOTA 정확도를 달성했습니다.
- 의미 있는 변수 이름을 사용하면 LLM이 변수와 문제의 개체를 더 잘 연결할 수 있어 성능이 크게 향상됩니다. 무의미한 변수 이름을 사용하면 PAL의 성능이 COT 수준으로 떨어집니다.
- 단순히 더 나은 프롬프트 때문이 아니라, Python 인터프리터와의 시너지 효과로 인해 PAL의 주요 성능 향상이 발생합니다.
- PAL은 코드 생성에 특화된 모델뿐만 아니라 자연어에 대해 훈련된 언어 모델에서도 작동하며, 모델의 코딩 능력이 충분히 높으면 COT보다 더 나은 성능을 보입니다.

### LLM+P (Large Language Models + Classical Planners)
LLM+P: Empowering Large Language Models with Optimal Planning Proficiency
https://arxiv.org/abs/2304.11477

접근 방식
- LLM+P은 LLM을 사용하여 자연어로 주어진 계획 문제를 PDDL(Planning Domain Definition Language) 형식으로 변환한 다음, 고전적인 계획기를 활용하여 해를 빠르게 찾고, 찾은 해를 다시 자연어로 변환하는 프레임워크입니다.
- LLM+P에서는 LLM이 자연어 문제 기술을 이해하고 프로그램 단계로 분해하는 역할을 하지만, 실제 계획 수립과 실행은 고전적인 계획기에 맡깁니다.
- LLM+P은 도메인에 대한 배경 지식을 PDDL 도메인 파일 형태로 제공받고, 해당 도메인 내 간단한 예시 문제와 그에 대응하는 PDDL 파일을 문맥(context)으로 활용하여 LLM의 few-shot 학습 능력을 활용합니다.

주요 발견
- LLM+P는 7개의 로봇 계획 도메인과 20개의 자동 생성된 작업에 대해 실험을 수행했으며, 대부분의 문제에 대해 최적의 해를 제공할 수 있었습니다. 반면 LLM 자체만으로는 대부분의 문제에 대해 실행 가능한 계획도 제공하지 못했습니다.
- LLM에 주어지는 예시 문제와 그에 대응하는 PDDL 파일이 LLM+P의 성공에 중요한 역할을 합니다. 이러한 문맥 정보 없이는 LLM이 정확한 PDDL 파일을 생성하지 못했습니다.
- LLM+P을 서비스 로봇에 적용하여 사용자가 자연어로 지정한 복잡한 정리 작업을 최적으로 수행할 수 있음을 시연했습니다. LLM 자체만으로는 차선의 계획만 생성한 반면, LLM+P은 최적의 계획을 찾아냈습니다.

### Scratchpads
Show Your Work: Scratchpads for Intermediate Computation with Language Models
https://arxiv.org/abs/2112.00114

접근 방식
- Scratchpad 기법은 LLM이 중간 계산 결과를 기록할 수 있는 작업 공간(buffer)을 제공하여 LLM이 multi-step 연산을 수행할 수 있도록 합니다.
- Scratchpad에 중간 계산 과정을 명시적으로 쓰도록 모델을 훈련시키고, 표준 지도 학습을 사용합니다.
- 중간 상태를 샘플링하여 생성 모델에서 구체화함으로써 작은 오차가 누적되는 것을 방지합니다.
- 스크래치패드 포맷을 수정하여 모델의 일반적인 오류를 해석하고 수정합니다.

주요 발견
- Scratchpad을 사용하면 덧셈, 다항식 계산, Python 코드 실행 등 다양한 작업에서 LLM의 성능이 크게 향상됩니다.
- Scratchpad 사용 시 학습 데이터의 분포를 벗어난 더 큰 문제 인스턴스(자릿수가 더 많은 덧셈 등)로 일반화되는 능력도 개선됩니다.
- Scratchpad은 few-shot 및 fine-tuning 레짐 모두에서 성능 향상을 보였습니다.
- 특히 Python 코드 실행에서는 프로그램에서 샘플링한 추가 데이터로 훈련된 scratchpad 모델이 직접 실행 모델보다 3배 이상 높은 정확도를 보였습니다.
- 대규모 데이터셋에서 추가로 학습하면 scratchpad 성능이 더욱 개선되어, 모델이 42%의 테스트 케이스에 대해 완벽하게 실행 흔적을 예측할 수 있었습니다.
### DSR-LM 
Improved Logical Reasoning of Language Models via Differentiable
Symbolic Programming
https://arxiv.org/abs/2305.03742

접근 방식
- 사전학습된 언어모델이 사실적 지식의 인식을 담당하고 기호적 모듈이 연역적 추론을 수행하는 방식으로, 차별가능한 기호적 추론 모듈을 사전학습된 언어모델과 엔드투엔드 방식으로 통합함.
- 수작업으로 제작된 규칙에 의존하지 않고 자동으로 논리 규칙을 유도하고 미세조정하도록 프레임워크를 개선함. 언어모델을 프롬프트하여 규칙 가중치를 초기화하고 이를 더 최적화함.
- 예측된 엔티티 관계와 규칙에 대한 논리적 무결성 제약조건에서 얻은 의미론적 손실을 통합하여 논리적 일관성을 개선함.

주요 발견
- DSR-LM은 사전학습된 언어모델의 논리적 추론 능력을 크게 향상시켜, 연역적 추론 벤치마크에서 20% 이상의 정확도 향상을 보임.
- 더 작은 RoBERTa 백본을 사용하고 명시적인 지도없이도, DSR-LM은 다양한 베이스라인보다 큰 폭으로 우수한 성능을 보임.
- DSR-LM은 고차 술어만 제공된 상태에서 사람이 해석가능한 논리 규칙을 유도하여 결정을 설명할 수 있음.
- 더 긴 추론 연쇄에 대해 체계적으로 테스트했을 때, DSR-LM은 합성적으로 추론하는데 실패하는 사전학습 언어모델이나 다른 신경망 접근법에 비해 훨씬 더 강건한 일반화를 보임.
### Symbol-LLM
Symbol-LLM: Towards Foundational Symbol-centric Interface For Large Language Models
https://arxiv.org/abs/2311.09278

방법
기존 벤치마크, GPT-4 프롬프팅, 그리고 기호 간 관계를 포착하기 위한 기호 진화 전략을 통해 약 20개의 기호 형식을 다루는 34개의 텍스트-기호 생성 작업을 수집합니다.
기호 지식을 LLM에 주입하는 Injection 단계와 기호와 자연어 능력을 균형 있게 유지하는 Infusion 단계를 포함한 두 단계 튜닝 프레임워크를 제안합니다.
LLaMA에서 초기화되고 기호 수집 및 프레임워크로 최적화된 Symbol-LLM 시리즈 모델을 소개합니다.
주요 발견
Symbol-LLM은 다양한 기호 작업에서 LLaMA보다 49-56% 향상된 성능을 보여줍니다.
Symbol-LLM의 통합 모델링은 단일 기호 미세 조정보다 다양한 기호 형식 간의 내재적 관계를 더 잘 포착합니다.
Symbol-LLM은 일반 자연어 작업에서도 LLaMA와 경쟁력 있는 성능을 유지합니다.
Symbol+Delegation 패러다임 하에서 Symbol-LLM은 수학 단어 문제와 같은 작업에서, 심지어 제로샷 설정에서도 GPT-3.5와 Claude에 비해 우수하거나 경쟁력 있는 결과를 보입니다.
분석 결과, Symbol-LLM은 임베딩 공간에서 기호의 특이성과 표현력을 최적화하고 기호 간의 관계를 잘 포착하는 데 뛰어남을 보여줍니다.

Methods
- Curates a collection of 34 text-to-symbol generation tasks covering ~20 symbolic forms from existing benchmarks, GPT-4 prompting, and a symbol-evol strategy to capture symbol interrelations
- Proposes a two-stage tuning framework: Injection stage to inject symbolic knowledge into LLMs, and Infusion stage to balance symbolic and natural language abilities
- Introduces Symbol-LLM series models initialized from LLaMA and optimized with the symbolic collection and framework

1) "Evaluation and Structured Outputs"  (https://huggingface.co/blog/evaluation-structured-outputs)에서 프롬프트 내의 사소한 템플릿 차이가 LLM의 성능에 미치는 영향에 대해 자세히 설명하고 있습니다. 저 역시 간단한 수학 문제의 프롬프트를 약간만 변형하는 것으로 LLM의 정답률이 크게 변화하는 것을 확인한 바 있습니다.(https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_18-prompt-brittleness-%EC%95%84-%EB%8B%A4%EB%A5%B4%EA%B3%A0-%EC%96%B4-%EB%8B%A4%EB%A5%B4%EB%8B%A4-activity-7185269968853204993-98S-?utm_source=share&utm_medium=member_desktop)
