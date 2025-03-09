[LinkedIn](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_prompt-chaining-activity-7234154601392783360-_Opc?utm_source=share&utm_medium=member_desktop&rcm=ACoAADfxcywBkH2Mi2-YPZm7jSZERa3dQ2_DDEY)
[Medium](https://medium.com/@simple0314/prompt-chaining-6758819cdb73)

안녕하세요, 프롬프트를 여러 단계로 나눠 순차적으로 실행하는 Prompt Chaining의 효과를 비교 분석하는 연구를 간단히 정리했습니다. 그리고 이러한 방법을 보다 깊이 탐구한 초기 연구 또한 같이 정리해봤습니다. 
## SELF-REFINE: Iterative Refinement with Self-Feedback
https://arxiv.org/abs/2303.17651

#### 연구 개요 및 목적

SELF-REFINE은 동일한 LLM을 사용하여 초기 출력을 생성하고, 그에 대한 피드백을 제공한 후, 해당 피드백을 바탕으로 출력을 개선하는 과정을 반복해 답변의 품질을 개선하는 방법입니다. 이는 인간의 자기 개선 과정을 모방한 것으로, 추가적인 훈련 데이터나 강화 학습 없이도 LLM의 성능을 향상시킬 수 있습니다.

#### 연구 방법 및 평가 방법

##### 연구 방법
1. **다양한 작업 선정**:
    - 기존 연구 기반 작업(5종): Dialogue Response Generation, Code Optimization, Code Readability Improvement, Math Reasoning, Sentiment Reversal 
    - 해당 연구에서 제안한 작업(2종): Acronym Generation,  Constrained Generation
2. **다양한 모델 사용**:
    - GPT-3.5 (text-davinci-003), ChatGPT (gpt-3.5-turbo), GPT-4를 주요 기본 모델로 사용
    - 코드 관련 작업에서는 CODEX (code-davinci-002)도 추가로 사용
3. **주요 구성 요소**
    - 각 작업별, 단계별 특화된 프롬프트를 활용했으며 few-shot 예시와 필요한 경우 추가로 지시사항이 포함되었습니다.
    ![[Prompt Chaining/images/figure 30.jpg]] ![[Prompt Chaining/images/figure 31.jpg]]
	![[Prompt Chaining/images/figure 32.jpg]]



	3.1 **초기 생성 (Initial Generation)**
    - Few-shot 예시와 지시사항을 참고하여 초기 출력을 생성합니다.
    - 예를 들어, Sentiment Reversal 작업에서는 "매우 긍정적인 리뷰를 부정적으로 바꾸세요"와 같은 지시와 함께 몇 가지 예시가 제공됩니다.
	3.2 **피드백 (Feedback)**
    - 모델은 초기 생성 결과물에 대해 구체적(specific)이고 실행 가능한(actionable) 피드백을 생성합니다.
    - Input-output-feedback 형식의 예시를 활용합니다.
	3.3 **개선 (Refine)**
    - 피드백을 바탕으로 초기 생성 결과물을 개선합니다.
    - Input-output-feedback-refined 형식의 예시를 활용합니다.
	3.4 **반복 (Iteration)**
    - 특정 기준(최대 4회의 반복 횟수 또는 목표 평가 점수)을 만족할 때까지 피드백과 개선 과정을 반복합니다.
    - 각 반복에서 이전의 모든 출력과 피드백이 프롬프트에 추가되어, 모델이 과거의 시도를 참고할 수 있게 합니다.

##### 평가 방법

1. **작업별 특화된 평가 지표**:
    - Math Reasoning: solve rate
    - Code Optimization: 최적화된 프로그램의 비율
    - Constrained Generation: 주어진 개념의 포함 비율(coverage)
2. **인간 선호도 조사**:
    - Dialogue Response Generation, Code Readability Improvement, Sentiment Reversal, Acronym Generation에서 사용
    - 블라인드 A/B 테스트 방식 채택: 평가자에게 두 가지 출력(SELF-REFINE과 기준 모델)을 제시하고 선호도 조사
    - 평가 기준: 작업 지시사항에 대한 더 나은 부합성
3. **GPT-4 기반 평가**:
    - GPT-4를 프롬프트하여 인간 평가자의 역할을 수행하게 하여 보조 수단으로 사용
    - 인간 평가와의 상관관계 분석: Sentiment Reversal(82%), Acronym Generation(68%), Dialogue Response Generation(71%)에서 높은 상관관계 관찰

#### 결과 및 해석

##### SELF-REFINE은 기존 모델들보다 일관되게 성능이 우수한가?
![[Prompt Chaining/images/table 1 1.jpg]]
1. **다양한 작업에서의 성능 향상**: SELF-REFINE은 각 작업에서 SELF-REFINE은 기본 모델 대비 상당한 성능 향상을 보였습니다.
2. **선호도 기반 작업에서의 뛰어난 성과**: 특히 Dialogue Response Generation, Sentiment Reversal, Acronym Generation과 같은 선호도 기반 작업에서 큰 폭의 성능 향상을 보였습니다.
3. **Constrained Generation 결과 개선**: 20-30개의 주어진 개념을 포함하는 문장 생성 작업에서 특히 뛰어난 성능을 보였습니다. 
4. **Math Reasoning에서의 미미한 효과**: Math Reasoning 작업에서는 SELF-REFINE의 성능 향상이 제한적이었습니다. 수학적 문제에서 오류를 정확히 식별하는 단계에서 어려움을 겪은 것으로 보여집니다. 그러나 LLM 외부에서 답안의 정답 여부를 판단해줄 경우(Oracle Feedback) 성능이 더 크게 향상되었습니다.
##### 모델 크기가 SELF-REFINE의 성능에 어떤 영향을 미치는가?
1. **모델 크기에 따른 성능 향상 패턴**: 일반적으로 GPT-4 > ChatGPT > GPT-3.5 순으로 SELF-REFINE을 통한 성능 향상이 크게 나타났습니다. 
2. **소규모 모델에서의 한계**: Vicuna-13b와 같은 비교적 작은 모델에서는 SELF-REFINE이 효과적으로 작동하지 않았습니다. 초기 출력 이후 피드백 생성과 개선 과정에서 어려움을 겪었습니다. 
##### 피드백의 품질이 SELF-REFINE의 성능에 어떤 영향을 미치는가?
![[Prompt Chaining/images/table 2 1.jpg]]
1. **구체적이고 실행 가능한 피드백의 효과**: 구체적(specific)이고 실행 가능한(actionable) 피드백이 일반적인(generic) 피드백보다 효과적이었습니다. 예를 들어, "더 긍정적인 표현을 사용하세요"보다는 "문장의 두 번째 부분에 '훌륭한', '뛰어난'과 같은 긍정적인 형용사를 추가하세요"와 같은 피드백이 더 좋은 결과를 가져왔습니다.
2. **피드백의 중요성**: 실험 결과 실패 사례의 33%가 오류가 발생한 위치를 잘못 파악해 발생했으며 61%는 잘못된 개선안을 제시한 피드백 때문이었습니다. 실패 사례 중 오직 6%만이 개선안 생성 단계에서 발생했습니다. 
3. **피드백 강건성**: 성공 사례의 33%에서는 부분적으로 잘못된 피드백에도 불구하고 모델이 올바르게 출력을 개선할 수 있었습니다.  
4. **피드백의 다면성**: 여러 측면을 고려한 다면적 피드백이 단일 측면의 피드백보다 더 효과적이었습니다. 예를 들어 Dialogue Response Generation 작업에서는 관련성, 정보성, 흥미로움, 일관성 등 여러 측면에 대한 피드백을 제공했을 때 더 좋은 결과를 얻었습니다.
##### 반복 횟수가 SELF-REFINE의 성능에 어떤 영향을 미치는가?
![[Prompt Chaining/images/figure 4 1.jpg]]

1. **반복에 따른 점진적 개선**: 대부분의 작업에서 반복 횟수가 증가할수록 출력의 품질이 점진적으로 향상되었습니다. 
2. **최적의 반복 횟수**: 대부분의 작업에서 3-4회의 반복 후에 성능 향상폭이 둔화되는 경향을 보였습니다. 
3. **작업별 차이**: Math Reasoning 작업에서는 반복 횟수 증가에 따른 성능 향상이 제한적이었습니다. 
##### SELF-REFINE은 다른 방법론들과 비교하여 어떤 장점이 있는가?
1. **추가 훈련 불필요**: SELF-REFINE은 추가적인 훈련 데이터나 모델 파라미터 업데이트 없이 다양한 작업에서의 성능 향상을 가능하게 합니다. 이는 강화학습이나 지도학습 기반 방법들이 각 작업이나 도메인에 대해 별도의 모델을 훈련시켜야 하는 것과 대조됩니다.

#### 메모
1. **프롬프트의 일관성 부재** 
- 연구에 활용되는 프롬프트들은 "초안 작성 - 피드백 생성 - 개선안 작성"이라는 일관된 흐름을 따르고 있습니다. 그러나 Appendix에 제시된 각 작업별 프롬프트는 형식과 내용 면에서 상당한 차이를 보입니다. 예를 들어, 일부 작업에서는 명시적인 지시사항(instruction)을 포함하거나 피드백 내에 점수를 제공하는 등의 차이가 있습니다. 사용된 프롬프트나 예시(exemplar)가 왜 사용되게 되었는지 과정에 대한 상세한 설명이 부족한 것은 LLM 관련 연구에서 자주 발견되는 문제점입니다. 이런 상황에서는 연구 결과가 특정 프롬프트에 의존적인 것인지, 아니면 제안된 방법론 자체의 효과인지 구분하기 어렵습니다.
1. 피드백의 영향
- 본 연구의 핵심은 피드백입니다. "정확하게 어떤 부분을 고칠지" 피드백을 제공하는 것이, 모호한 피드백보다 명확하게 도움이 되며, 최종 결과물이 잘못되었을 경우 94% 정도에서 피드백 생성 단계에서 오류가 발생했습니다. 다만 최종 결과물이 잘 개선되었을 경우에도 피드백 단계에서 30% 정도 오류를 목격했다고 합니다. 저자들은 이를 'resilience(회복력)'이라고 표현하고 있지만, 과연 잘못된 피드백을 생성하고도 그 다음 결과가 좋다는 것이 LLM에게 있어 바람직한 성질인지는 잘 모르겠습니다. Chain-of-Thought과 관련된 연구들에서도 중간 추론 과정에 틀림에도 결국 옳은 답을 도출하는 것을 'reliability'에 문제가 있다고 표현하는 것을 보면, 이런 현상을 마냥 좋게 바라볼 수는 없겠네요.
## Prompt Chaining or Stepwise Prompt? Refinement in Text Summarization
https://arxiv.org/pdf/2406.00507
#### 연구 개요 및 목적
이 연구는 LLM을 사용한 텍스트 요약 작업에서 'prompt chaining'과 'stepwise prompt' 두 가지 방법론을 비교합니다. 
#### 주요 개념 설명
![[Prompt Chaining/images/figure 1.jpg]]
##### Prompt Chaining
Prompt chaining은 요약 과정을 세 단계로 나누어 각 단계마다 별도의 프롬프트를 사용하는 방식입니다:
1. 초안 작성(Drafting): LLM이 주어진 텍스트의 초기 요약본을 생성합니다.
2. 비평(Critiquing): 첫 단계에서 생성된 요약본에 대한 피드백과 개선점을 제시합니다.
3. 수정(Refining): 비평을 바탕으로 초기 요약본을 개선합니다.
각 단계는 "독립적인" 프롬프트로 실행되며, 이전 단계의 출력이 다음 단계의 입력으로 사용됩니다. 이 방식은 각 단계에서 LLM이 특정 작업에 집중할 수 있게 하여, 복잡한 다중 작업의 부담을 줄일 수 있습니다.
##### Stepwise Prompt
Stepwise prompt는 위의 세 단계(drafting, critiquing, refining)를 하나의 긴 프롬프트에 모두 포함시켜 한 번에 처리하는 방식입니다. 이 방법은 단일 프롬프트로 수행되므로, 사용자 입장에서는 더 간단하고 편리할 수 있습니다. 그러나 LLM이 한 번에 여러 작업을 수행해야 하므로, 각 단계의 품질이 저하될 가능성이 있습니다.

#### 연구 방법
##### 데이터셋: InstruSum
이 연구에서는 InstruSum이라는 데이터셋을 사용했습니다. InstruSum은 LLM의 요약 능력을 평가하기 위해 특별히 설계된 데이터셋입니다.
- 100개의 기사-요구사항 쌍으로 구성
- 각 기사는 약 1000-1200단어로, BBC 뉴스 웹사이트에서 추출
- 요약 요구사항은 다음 세 가지 유형으로 분류됩니다:
    1. Informational requirement: 기사의 주제나 내용에 관한 구체적인 정보 요구
    2. Formatting requirement: 글머리 기호 목록 등 요약의 구조를 개선하여 가독성을 높이는 요구
    3. Meta requirement: 기사의 전반적인 개요를 요구
이는 독자들이 실제로 가질 수 있는 다양한 요구사항을 반영하고 있습니다.

##### 평가 기준
연구팀은 다음의 세 가지 평가 기준을 적용했습니다:
1. Overall quality: 요약 요구사항을 충족시키는 전반적인 요약의 품질
2. Missing information: 요약 요구사항과 관련된 중요한 정보의 누락 여부
3. Irrelevant information: 요약 요구사항과 관련 없는 정보의 포함 여부

#### 사용된 모델
- GPT-3.5 (gpt-3.5-turbo-0125)
- GPT-4 (gpt-4-0125-preview, gpt-4-1106-preview)
- Mixtral 8x7B

#### 평가 방법

##### LLMCompare 
- LLMCompare는 두 개의 요약본을 비교하여 우열을 가리는 벤치마크입니다. 
- GPT-4를 평가자로 사용하여 두 요약본을 비교합니다.
- GPT-4에게 원본 기사, 요약 요구사항, 두 개의 요약본을 제공합니다.
-  GPT-4는 제공된 정보를 바탕으로 두 요약본의 품질을 비교합니다.
- 'Win', 'Tie', 'Lose'의 세 가지 결과로 평가됩니다.
- 평가 과정에서 요약본 쌍의 순서를 무작위로 섞어 위치 편향을 방지합니다.

##### METACRITIQUE 
- METACRITIQUE는 precision, recall, f1 score을 활용해 critique의 품질을 평가하는 벤치마크입니다.

#### 주요 실험 및 결과
##### Exp I: Summarization Benchmark
![[Prompt Chaining/images/table 1 2.jpg]]
- 목적: 다양한 모델과 prompt 방식의 요약 성능을 비교합니다.
- 실험 방법:
1. GPT-4 (gpt-4-0125-preview)를 baseline으로 설정합니다.
2. 다른 모델들(GPT-3.5, Mixtral)과 각 모델에 prompt chaining과 stepwise prompt를 적용한 결과를 비교합니다.
3. LLMCompare를 사용하여 각 방법으로 생성된 요약을 baseline과 비교합니다.

- 결과:
1. 대부분의 경우에서 prompt chaining이 stepwise prompt보다 나은 성능을 보였습니다.
2. 특히 GPT-4에 prompt chaining을 적용한 경우 가장 큰 성능 향상을 보였습니다.
3. 모델의 성능이 좋을수록(예: GPT-4) prompt chaining의 효과가 더 크게 나타났습니다.
##### Exp II: Robustness
![[Prompt Chaining/images/figure 2.jpg]]
- 목적: 평가 모델의 변화에 따른 결과의 일관성을 검증합니다.
- 실험 방법:
1. GPT-4의 두 버전(gpt-4-1106-preview와 gpt-4-0125-preview)을 평가자로 사용합니다.
2. Prompt chaining과 stepwise prompt의 refined 결과를 비교합니다.
3. Overall quality, missing information, irrelevant information의 세 가지 측면에서 평가합니다.
- 결과:
1. Overall quality와 missing information 측면에서 prompt chaining이 일관되게 우수한 성능을 보였습니다.
2. Irrelevant information 측면에서는 stepwise prompt가 약간 더 나은 성능을 보였습니다.
3. 두 버전의 GPT-4에서 유사한 결과가 나타나, 연구 결과의 일관성을 확인할 수 있었습니다.
##### Exp III: Human Evaluation
![[Prompt Chaining/images/table 2 2.jpg]]
- 목적: LLM 기반 평가와 인간 평가 사이의 일치도를 확인하고, 실제 사용자 관점에서의 요약 품질을 평가합니다.
- 실험 방법:
1. 대학원생 2명이 평가를 수행합니다.
2. LLMCompare와 동일한 기준(overall quality, missing information, irrelevant information)으로 평가합니다.
3. Prompt chaining과 stepwise prompt의 결과를 직접 비교합니다.
- 결과:
1. 인간 평가에서도 prompt chaining이 stepwise prompt보다 우수한 성능을 보이는 경향이 확인되었습니다.
2. 그러나 샘플 크기가 작아 통계적 유의성을 확보하기는 어려웠습니다.
##### Exp IV: Critique Evaluation
![[Prompt Chaining/images/table 3 1.jpg]]
- 목적: 중간 단계인 critique의 품질을 평가하여, 각 방법의 내부 프로세스를 이해하고 최종 요약 품질과의 관계를 분석합니다.
- 실험 방법:
1. METACRITIQUE를 사용하여 precision, recall, F1 score를 비교합니다.
2. GPT-3.5와 Mixtral 모델에 대해 평가를 수행합니다.
3. GPT-4의 결과는 METACRITIQUE의 reference로 사용되어 제외되었습니다.
- 결과:
1. Stepwise prompt가 prompt chaining보다 더 높은 품질의 critique를 생성했습니다.
2. 그러나 이 결과가 최종(refined) 요약의 품질 향상으로 이어지지는 않았습니다.
#### 주요 발견 및 해석
1. **Prompt Chaining의 우수성** Prompt chaining이 대부분의 경우에서 stepwise prompt보다 나은 성능을 보였습니다. 이는 각 단계를 분리함으로써 LLM이 각 작업에 더 집중할 수 있게 되어, 전체적인 요약 품질이 향상된 것으로 해석됩니다.
2. **"Simulated Refinement Process"** LLM이 한 번에 초안, 비평, 수정 단계의 답변을 생성하지만, 각 단계가 유기적으로 연결되지 않아 최종 요약의 품질 향상으로 이어지지 않았습니다. 이는 critique quality와 final summary quality 사이의 불일치를 통해 더욱 명확히 드러납니다. Stepwise prompt가 prompt chaining보다 더 좋은 critique를 생성했음에도 불구하고, 최종 요약의 품질은 오히려 더 낮았습니다. 이는 stepwise prompt에서 생성된 좋은 critique가 최종 요약에 효과적으로 반영되지 못하고 있음을 보여줍니다. 이러한 현상은 stepwise prompt 방식이 표면적으로는 refinement process를 모방하고 있지만, 실제로는 각 단계의 출력이 다음 단계에 제대로 활용되지 않는 "simulated(허위)" 프로세스를 만들어내고 있다 볼 수 있습니다.
3. **모델 성능 차이** 실험 결과, GPT-4에서도 다른 모델들(GPT-3.5, Mixtral)과 같이 prompt chaining의 효과가 나타났습니다. 모델의 성능과 관계없이, 복잡한 작업을 단계별로 나누어 처리하는 접근 방식이 전반적으로 더 효과적일 수 있음을 의미합니다.

#### 메모
1. Anthropic에서도 Prompt Chaining과 관련해서 예시와 함께 소개하고 있으니, 관심 있으신 분들은 참고하시면 좋을 것 같습니다 (https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/chain-prompts ).
2. Prompt Chaining이 더 좋은 이유가 "LLM이 각 작업에 더 집중할 수 있기 때문" 정도로 설명되고 있는 것 같고, 다른 연구에서도 더 깊게 다뤄지고 있지는 않는 것 같습니다. HCI 관점에서 인간 유저가 더 쉽게 LLM system을 조작할 수 있도록 하는 연구 외에는 더 찾아볼 수 없었습니다 (https://arxiv.org/pdf/2110.01691, https://arxiv.org/pdf/2203.06566). 
3. 실시간성이 중요한 chat interface에는 당장 적용하기 어렵겠지만, 결과물의 품질이나 디버깅이 중요한 작업에서는 고려해 볼 법하다고 생각합니다. 다만 프롬프트를 나누는 단계에서는 아직 뚜렷한 기준이 없기 때문에 직접 실험해보면서 찾는 수 밖에 없어 보이네요.   
4. 보다 큰 작업을 "더 이상 나눌 수 없는" 세부 작업으로 나누고, 그 중 자체적인 기준을 만족하지 못하는 부분에 대해서만 데이터를 생성해 fine-tuning을 진행하는 등의 방법도 적용해볼 수 있을 것 같네요. 

