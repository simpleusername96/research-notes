# research-notes

독립적으로 탐구하며 읽은 자료들을 기록하는 용도의 저장소입니다. LLM의 유연성과 불확실성에 매료되어 공부를 시작하게 되었으며, 이 자료들은 제 학습을 돕고 실무에 참고하기 위해 정리되었습니다.

**주의**: 본 자료는 독학하며 정리한 내용이므로 일부 내용에 오류가 있을 수 있습니다. 피드백과 토론은 언제든지 환영합니다!

주로 다음과 같은 내용을 다룹니다.
- inference 단계에서 다양한 입력 형식이 정량적 지표와 downstream task에 미치는 영향과 그 원인
- pre/post-training 과정에서 데이터셋 구성 방식이 정량적 지표와 downstream task에 미치는 영향과 그 원인
- LLM을 활용해 인간 집단의 상호작용을 모사하는 방법과 LLM 기반 시스템이 사회에 미치는 영향
- LLM의 내부 작동 기전과 reasoning, hallucination, generalization, sycophancy, planning, alignment 등의 행동 패턴 관찰

ENG
<details>

This repository is dedicated to arxiving materials that I've independently explored. My studies began from a fascination with the flexibility and uncertainty inherent in LLMs, and these documents are compiled to assist my learning and practical applications.

**Note**: Since this repository consists of self-organized materials, some content may contain inaccuracies. Feedback and discussions are always welcome!

Specifically, this repository covers:
- How various forms of input data during inference impact quantitative metrics and downstream tasks, and the underlying reasons.
- How dataset composition methods during pre- and post-training affect quantitative metrics and downstream tasks, and the reasons for these effects.
- Techniques for simulating human group interactions using LLMs, and implications of LLM-based systems on society.
- Observations of LLMs' internal mechanisms and behavioral patterns, including reasoning, hallucination, generalization, sycophancy, planning, and alignment.
- 
</details>


Repo Structure
	
- `/research`
	- LLM과 관련된 다양한 연구 논문과 기사에 대한 리뷰와 분석을 포함합니다. 
	- This section contains reviews and analyses of various research papers and articles related to LLMs.​
	- [Variation selector](<other/Variation selector/content>), [GSM8K Platinum](<other/GSM8K Platinum/content>), [Developer Messages](<other/Developer Messages/content>) etc.

- `/other`: This section encompasses a variety of topics, including:​
    - LLM 애플리케이션 리뷰 (Reviews of LLM applications)
    - LLM 커뮤니티 내 토론 및 뉴스​ (Discussions and news within the LLM community​)
	- 모델 및 벤치마크 평가​ (Evaluations of models and benchmarks​)
	- LLM의 예외적 행동 사례 분석​ (Analysis of Exceptional LLM Behavior)
	- [Critique Fine-Tuning](<research/Critique Fine-Tuning Learning to Critique is More Effective than Learning to Imitate/content>), [Emergent Misalignment](<research/Emergent Misalignment Narrow finetuning can produce broadly misaligned LLMs/content>), [Emergent Value Systems](<research/Utility Engineering Analyzing and Controlling Emergent Value Systems in AIs/content>) etc.
	- 