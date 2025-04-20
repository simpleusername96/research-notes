2024.08.27

Distilling System 2 into System 1
https://arxiv.org/abs/2407.06023

안녕하세요, Meta FAIR에서 인상 깊은 Distillation 연구를 발표해 이를 정리해봤습니다.

0. 메모
- 이 연구는 '답변 전에 중간 토큰을 생성'하는 LLM 생성 기법들을 'System 2'로 정의합니다. 이는 Chain-of-Thought, ReAct, System 2 Attention 등의 접근 방식을 포함합니다. 이러한 기법들은 여러 도메인에서 답변의 품질을 비약적으로 높이는 대신, 중간 토큰을 생성하는 과정에서 상당한 자원을 소모한다는 점이 한계로 지적되어 왔습니다.
- 이 연구는 fine-tuning을 통해 System 2의 장점을 유지하면서 비용 문제를 해결하고자 했습니다. CoT를 활용한 수학 추론을 제외한 모든 분야에서, 중간 추론 과정을 생략하고 추가 토큰 생성 없이도 높은 성능을 달성했습니다.
- 발표의 주체가 Meta의 FAIR라는 점에서 이러한 접근 방법이 Llama 3과 Llama 3.1에 활용되었다고 생각할 수 있습니다만, 이는 명시되지 않았습니다.
- LLM을 활용한 학습 데이터 생성에  self-consistency를 적용해 높은 품질의 답변을 확보할 수 있다는 것과 중간 추론 단계를 생략하고 그로 인해 도출된 답변 만을 학습시켜도 LLM의 성능을 개선시킬 수 있다는 점이 흥미로웠네요.

1. 서론
- System 1과 System 2의 정의:
    - System 1: 중간 단계 없이 직접 답변을 생성하는 방식
    - System 2: 중간 토큰을 생성하거나 여러 단계를 거쳐 최종 답변을 도출하는 방식
- System 2의 예시:
    - Chain-of-Thought (CoT)
    - Rephrase and Respond (RaR)
    - System 2 Attention (S2A)
    - Branch-Solve-Merge (BSM)
- System 2의 특징:
    - 높은 정확도
    - 높은 연산 비용과 지연 시간
- 연구 목표: System 2의 성능을 유지하면서 System 1의 효율성을 달성하는 방법 개발

2. 관련 연구
	2.1 인간의 System 1과 System 2
	- 심리학적 개념:
	    - Automaticity (자동성): 의도적인 사고(System 2)에서 자동적인 사고(System 1)로 전환되는 과정
	    - 절차적 기억의 활용: 반복적인 경험을 통해 자동화된 행동 패턴 형성
	- 예시: 운전 경로 학습
	    - 초기: 의식적인 노력과 계획 필요 (System 2)
	    - 숙달 후: 자동적으로 경로 따라가기 (System 1)

	2.2 모델에서의 System 1과 System 2	
	- System 1 모델:
	    - 정의: 중간 출력 없이 직접 응답을 생성
	    - 특징: 복잡한 상징적 추론(symbolic reasoning) 어려움
	- System 2 모델:
		- 정의: 최종 답안 생성 전 중간 토큰을 생성. 사고 과정을 나열하거나, 질문을 보다 이해하기 쉽게 추가 설명 혹은 관련 없는 내용 제거하는 방법이 제안됨
	    - Scratchpad, Chain-of-Thought 등을 통한 복잡한 작업 해결

	2.3 기존의 Distillation
	- 전통적 방식:
	    - 교사 모델과 학생 모델 사용
	    - 학생 모델이 교사 모델의 출력 분포, 레이어 활성화 모방
	- 본 연구의 차별점:
	    - 동일 모델 사용 (교사-학생 구조 없음)
	    - 중간 토큰 생성 없이 직접 최종 출력만 학습

3. System 2를 System 1으로 Distill하기
	- 목표: System 2의 추론 과정을 System 1 모델에 내재화
	- 과정:
	    1. 레이블 없는 입력 데이터 X 수집
	    2. System 2 모델을 사용해 응답 생성
	    3. 중간 출력 폐기, 최종 출력만 저장
	    4. 품질 향상을 위한 필터링 적용
			a) Self-consistency of outputs: 동일한 입력에 대해 여러 번 출력을 생성하고 일관성 있는 결과를 선택
			b) Self-consistency under input perturbation: 입력의 일부를 변형해도 출력이 일관성을 유지하는지 확인
	    1. 필터링된 데이터셋 생성
	    2. 원본 모델이 데이터셋으로 지도 학습

4. 실험
	4.1 훈련 및 평가 설정	
	- 기본 모델: Llama-2-70B-chat
	- 평가 지표:
	    - 프롬프트 기법마다 다른 평가지표 사용
	    - 생성된 토큰 수 (#Tokens): 중간 및 최종 출력 토큰 포함
	
	4.2 Rephrase and Respond (RaR) Distillation	
	- 개요
	    1. 원래 질문을 추가 설명과 함께 재구성
	    2. 재구성된 질문을 바탕으로 최종 응답 생성
	- 훈련 및 평가 데이터셋:  Last Letter Concatenation, Coin Flip Reasoning
	- Distillation 데이터 생성:
	    - Self-consistency of outputs
	    - 각 입력에 대해 8회 샘플링 후 다수결 적용
	-  Last Letter Concatenation Task	
		- 작업 설명: 주어진 단어들의 마지막 글자를 연결
		- 예시: “Take the last letters of the words in ‘Edgar Bob’ and concatenate them.” -> "rb"
	- Coin Flip Reasoning Task
		- 작업 설명: 동전 뒤집기 시나리오에서 최종 상태 예측
	- 결과:

		![images/table12.jpg](<images/table12.jpg>)
	- 분석  
	     — Last Letter Concatenation Task에서 필터링의 중요성 확인  
	     — 원본 모델 결과물에 필터링 적용해 distill 시(Distill System 1) 정확도 급증  
	     — RaR 결과물에 대해 필터링 없이 distill 시 필터링 후 distill하는 것보다 정확도 하락  
	     — Coin Flip Reasoning Task에서 원본 모델 결과물은 작은 프롬프트 변화에도 결과 크게 바뀜  
	     — Distilled System 2는 프롬프트 변화에 대해 안정적
	
	4.3 System 2 Attention (S2A) Distillation	
	- 개요
	    1. 입력에서 편향되거나 불필요한 정보 제거
	    2. 재작성된 짧은 프롬프트에 집중하여 응답 생성
	- 훈련 및 평가 데이터셋: SycophancyEval (편향된 정보를 포함한 QA 태스크)
	- Distillation 데이터 생성:
	    - Universal Self-Consistency (USC) 사용
	    - 20개 생성 샘플링 후 USC 프롬프트로 다수결 답변 도출
	- 결과:

		![images/table22.jpg](<images/table22.jpg>)
	- 분석:
		-  Distilled S2A가 원본 System 2 (S2A)보다 우수한 성능 달성  
		- 토큰 생성 수 대폭 감소  
		- USC 필터링의 중요성: USC 없이 distill 시 성능 저하
	
	4.4 Branch-Solve-Merge (BSM) Distillation	
	- 개요
	    1. Branch: 주어진 사용자 질문에 대해 질문의 관련성, 응답의 명확성, 정보의 정확성 등의 평가 기준을 수립.
	    2. Solve: 각 평가 기준에 따라 독립적으로 응답을 점수를 매겨 평가
	    3. Merge: Solve 단계에서 얻은 각 평가 기준별 점수를 종합하여 최종 평가 결정을 도출
	- 훈련 및 평가 데이터셋: Open Assistant Dataset v2 (OASST2), MT-bench
	- Distillation 데이터 생성:
	    - Self-consistency under input perturbation 활용
	- 평가 지표:
	    - Agreement: 모델의 판단과 인간 전문가 투표 간 일치도
	    - % Inconsistent: 위치 편향으로 인한 불일치 비율
	- 결과:
		![images/table31.jpg](<images/table31.jpg>)
	- 분석:
	    - Distilled BSM이 원본 BSM(Llama 2-70B-chat)보다 우수한 성능 달성
	    - 토큰 생성 수 대폭 감소
	    - 카테고리별 성능 분석에서도 Distilled BSM의 우수성 확인
	
	4.5 Chain-of-Thought (CoT) Distillation
	- 개요  
	     - LLM이 최종 답변을 생성하기 전 단계별 추론 과정을 생성  
	     - 입력 프롬프트에 다수의 question-CoT-answer를 포함하는 few-shot과 instruction에 think step-by-step만을 포함하는 zero-shot으로 나뉘어짐
	- 사용 데이터셋: GSM-8k
	- Distillation 데이터 생성:
	    - CoT 적용해 GSM-8k 훈련 데이터로부터 답변 생성
	    - 각 질문에 대해 10개의 샘플을 생성해 그 중 가장 많이 등장한 답변 추출
	- 평가 방법:
	    - GSM-8k 테스트 데이터에 대해 1/5/10개의 샘플을 생성해  그 중 가장 많이 등장한 답변 추출
	    - 생성된 총 토큰 수 측정
	- 결과:
		![images/table4.jpg](<images/table4.jpg>)
	- 분석:
	    - CoT Distillation이 성공적이지 못함
	    -  Distill CoT zero-shot 결과는 샘플링을 적용해도 크게 개선되지 않음
	    - 복잡한 수학적 추론 과정을 단순화하는 것의 한계 확인

5. 결론
	- 다양한 System 2 방법들의 성공적인 distillation 달성
	- 일부 경우 System 2보다 우수한 성능 달성
	- 연산 비용과 지연 시간 대폭 감소
	- 모든 System 2 방법이 distill 가능한 것은 아님 (예: CoT)
	- 태스크와 데이터셋에 따라 효과성 차이 존재
