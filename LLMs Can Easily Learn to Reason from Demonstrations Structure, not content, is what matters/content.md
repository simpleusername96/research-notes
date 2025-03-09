[Linkedin](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_llm%EC%9D%98-%EC%B6%94%EB%A1%A0-%ED%95%99%EC%8A%B5-%ED%95%99%EC%8A%B5-%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%9D%98-%EC%9D%BC%EA%B4%80%EC%84%B1%EA%B3%BC-%EA%B8%B8%EC%9D%B4%EA%B0%80-%EC%A4%91%EC%9A%94%ED%95%98%EB%8B%A4-llm%EC%9D%B4-%EB%B3%B5%EC%9E%A1%ED%95%9C-activity-7296759021066231808-GhfP?utm_source=share&utm_medium=member_desktop&rcm=ACoAADfxcywBkH2Mi2-YPZm7jSZERa3dQ2_DDEY)

LLM의 추론 학습, 학습 데이터의 일관성과 길이가 중요하다

LLM이 복잡한 문제를 효과적으로 해결하도록 학습하려면 사고 과정의 논리적 구조를 학습하는 것이 더 중요하다고 주장하는 연구가 발표되었습니다. 연구진은 RL 없이 단 17,000개의 샘플을 fine tuing 하는 것으로 기존보다 훨씬 강력한 추론 성능을 이끌어낼 수 있음을 보였습니다.

🔗 Arxiv: https://arxiv.org/pdf/2502.07374
🔗 Github: https://github.com/NovaSky-AI/SkyThought
🔗 Huggingface: https://huggingface.co/NovaSky-AI

---

 연구 내용 및 실험 결과

✅ 논리적 구조(Consistency)가 정확성보다 중요한 이유

🔹 잘못된 정답을 학습해도 성능 하락은 크지 않음
- 학습 데이터에서 정답을 틀리게 주더라도 모델의 성능은 3.2% 감소하는 데 그쳤습니다.
- 이는 모델이 정답 자체를 단순히 암기하는 것이 아니라, 추론 과정의 구조를 학습하고 있음을 의미합니다.

🔹 논리적 구조를 망가뜨리면 성능이 크게 감소
- 추론 과정의 순서를 무작위로 섞었을 때, 모델의 성능이 13% 이상 하락했습니다.
- 연구진은 "Step 1 → Step 2 → Step 3" 같은 체계적인 사고 과정이 유지될 때 모델이 더 강한 추론 능력을 갖춘다는 점을 보였습니다.
- 반면, reasoning step이 뒤섞이거나 빠지면, 모델은 표면적으로는 논리적인 답을 만들지만, 실제 문제 해결에는 실패하는 경향이 있었습니다.
- 즉, 정확한 답을 가르치는 것보다 논리적으로 올바른 reasoning 패턴을 학습하는 것이 더 중요합니다.

✅ LoRA fine-tuning으로도 충분
- 모델 파라미터 5% 미만만 업데이트하는 LoRA 방식이 full fine-tuning과 거의 동일한 성능 향상 제공

✅ Long CoT이 Short CoT보다 효과적
- Long CoT을 학습한 모델이 난이도 높은 문제에서 특히 뛰어난 성능을 보임
- 단순한 reasoning보다 자기 검토(reflection), 역추적(backtracking), 오류 수정(self-validation)을 포함한 긴 reasoning이 더 효과적

✅ 비추론 작업 성능 유지
- MMLU, ARC-C, IEval, MGSM 등 다양한 벤치마크에서 fine-tuning 후에도 원래 모델의 일반적인 언어 이해 능력이 유지됨을 확인

✅ Best-of-N 샘플링보다 효율적
- Long CoT fine-tuning은 Best-of-16 샘플링과 유사하거나 더 나은 성능을 보이며, 실제 추론 작업에서 더 효율적임을 확인

✅ 다양한 모델에서도 효과 검증
- 7B부터 32B까지 다양한 모델에서 동일한 fine-tuning 기법이 효과적임을 입증
- 하지만 작은 모델에서는 성능 향상의 폭이 상대적으로 크지 않음

---

데이터셋 구성 및 CoT 생성 방식

연구진은 DeepSeek-R1과 QwQ-32B 같은 오픈소스 LLM을 활용하여 기존 문제-정답 데이터에 추가적인 step-by-step reasoning을 생성하여 학습 데이터셋을 구축

📌 Short CoT vs. Long CoT
- Short CoT: 기존의 NuminaMath-CoT 데이터셋에서 가져온 데이터로, 비교적 간단한 reasoning 단계만 포함
- Long CoT: 연구진이 DeepSeek-R1과 QwQ-32B를 이용해 System Prompt를 설정한 후 새롭게 생성

📌 Long CoT System Prompt 예시
> "질문을 체계적으로 탐구하는 과정이 필요합니다. 답을 내리기 전에 논리적인 분석, 재검토(reflection), 역추적(backtracking), 오류 수정(self-validation) 등을 포함하여 사고 과정을 상세히 기술하세요."

이렇게 가이드를 주어 Short CoT보다 훨씬 깊이 있는 reasoning을 생성하도록 했습니다.

📌 데이터셋 구성
- 수학 데이터셋: AIME, AMC, OlympiadBench, Math500
- 코딩 데이터셋: LiveCodeBench, APPS, TACO
- 데이터 필터링: 난이도를 분류한 뒤 어려운 문제만 선별
- 정답 검증: 수학 문제는 정답과 비교, 코딩 문제는 실행 결과 확인

최종적으로 12,000개의 수학 문제와 5,000개의 코딩 문제를 포함한 학습 데이터셋을 구축했습니다.

---

마무리

Deepseek R1이나 OpenAI의 o 시리즈 모델처럼 rl을 적용하지 않고, 순수히 길고 일관된 논리를 전개하는 데이터셋을 fine tuning하는 것으로 추론 능력이 상승되었음을 보이는 연구입니다.
R1 distill 모델들이 추론 이외의 영역에서는 성능 저하를 보인다는 여러 사례들이 보고되었지만, 이 연구에서는 추론 이외의 벤치마크에서 점수가 유지된다는 것을 보여줍니다.
그리고 distillation과 fine tuning 간의 용어 정리가 시급하네요. 연구에서도 이를 혼용해서 활용하고 있는데, 결국 같은 말이라면 하나로 통일했으면 합니다. 
Test Time Scaling이 LLM의 reasoning을 확실하게 높여주는 것은 맞지만, 그렇다고 해서 이것이 유일한 길은 또 아닌 것 같습니다.
Reasoning 관련된 연구자들이 트위터 등에 남긴 코멘트를 보면 이를 인지하고 다양한 방법을 시도하고 있는 것 같고요.
그리고 연구에서 이를 parameter efficient라고 해서 전체 자원의 차원에서 더 효율적이다, 라고 받아들이면 안 될 것 같습니다.
Long CoT가 기존 CoT에 비해 8배~20배 가량 토큰 수가 많은 것을 보면, data corpus의 수를 늘리는 것으로 성능 향상을 이끌어낸 것으로 보입니다.
아무튼 모델과 학습 데이터가 전부 공개되어 있으니, 관심있으신 분들에게는 정말 도움이 될 것 같아 공유합니다!