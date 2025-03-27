2025.03.27

[LinkedIn](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_capability-vs-reliability-in-llm-benchmarks-activity-7311041701396062208-Pb6R?utm_source=share&utm_medium=member_desktop&rcm=ACoAADfxcywBkH2Mi2-YPZm7jSZERa3dQ2_DDEY)

Capability vs. Reliability in LLM Benchmarks

LLM 벤치마크들은 처음에는 좋은 기준이 됩니다. 모델 간 성능 차이가 뚜렷하고, 정답률도 50%를 넘기기 어렵기 때문입니다. 하지만 시간이 지나고 모델들이 전반적으로 발전하면서 대부분의 벤치마크는 90% 이상의 정답률을 기록하게 되고, 남은 오류들은 종종 '노이즈'로 간주됩니다. 그렇게 포화된 벤치마크는 점차 관심에서 멀어지죠.

문제는 이 과정에서 모델이 일관되게 정답만을 답하는가, 즉 신뢰성(reliability)은 거의 평가되지 않는다는 점입니다. 모델이 어떤 문제를 풀 수 있는지(capability)는 확인하지만, 얼마나 꾸준히 잘 푸는지는 놓치게 되는 거죠. 오늘 소개해드릴 연구에서는 이러한 문제의식에서 출발합니다. 최근에 소개한 GSM8K Platinum 벤치마크도 이 연구에서 비롯되었죠. 

Do Large Language Model Benchmarks Test Reliability?
arxiv: https://arxiv.org/abs/2502.03461
website: http://platinum-bench.csail.mit.edu/

우선, 포화되었다고 알려져 이제는 최신 모델 평가에 잘 나타나지도 않는 Winograd나 SQuAD같은 15개의 벤치마크들에서 '잘못된 정답'이나 '잘못된 문제'가 포함된 경우를 처리해 platinum benchmark를 만들었습니다. 이러면 노이즈가 없어졌기 때문에 SOTA LLM들이라면 모든 문제를 맞추어야겠죠. 하지만 박사급의 지식을 시험하는 벤치마크에서도 준수한 성과를 내는 o1이나 r1같은 문제들도 '모든 문제'를 맞추는 것은 불가능했습니다. 또한 일부 모델들에서는 특정한 문제 형식에서 지속적으로 높은 오답률을 기록했습니다. 

이 연구는 단순히 ‘문제를 풀 수 있는가(capability)’를 넘어서, ‘언제나 정확하게 푸는가(reliability)’를 별도로 측정하지 않으면, 모델의 취약함이 정답률 속에 감춰질 수 있고, 특히 agent 시스템처럼 동일한 작업을 반복적으로 수행해야 하는 환경에서는 그로 인한 누적된 실패가 치명적인 영향을 줄 수 있다는 점을 강조합니다. 짧은 연구이지만 여러가지 흥미로운 케이스들을 포함하고 있으니, 관심있으신 분들은 읽어보시기를 추천합니다!

---

🔍 벤치마크 분석

연구팀은 다수의 SOTA LLM을 사용해 벤치마크 문제들을 동시에 평가한 후, 틀렸거나 모델 간 정답 불일치가 발생한 사례를 중심으로 수작업 검토를 진행했습니다. 이 과정에서 다음과 같은 유형의 오류가 발견되었습니다.

🔸 Mislabel (정답이 잘못됨)

예: SVAMP의 다음 문제
> “You had 14 bags with equal number of cookies... How many bags of cookies do you have?”  
> ✅ 올바른 답: 14 (문제에 명시됨)  
> ❌ 벤치마크 라벨: 2

단순히 정답만 잘못 기입된 경우로, 수정 후 재사용 가능했습니다.

🔸 Logical contradiction / Ill-posed / Ambiguous Question (문제 자체가 잘못됨)

예: GSM8K
> “Ten stalls have 20 cows each. Mr. Sylas buys 40 cows and divides them equally into twenty stalls. How many cows are in 8 of the stalls?”  
> ➡ 소를 넣는 칸(stall)이 10개 있다고 명시한 후, 20개로 늘어남   

이런 경우엔 문제 자체가 부적절하다고 판단하고 제거했습니다.

---

🧪 오류를 제거하면, 정말 100점이 나올까?

정제된 데이터셋으로 다시 평가하자, 기존보다 오류 수가 절반 이상 감소했습니다.  
그러나 거의 모든 모델이, 극히 간단한 산술 연산 문제(SingleOP 등)를 제외한 벤치마크에서 오답을 냈고, 다음과 같은 고정적인 오류 패턴이 발견되었습니다:

📌 First Event Bias

> “A와 B 중 어떤 사건이 두 번째로 일어난 일인가?”와 같은 질문에서 모델이 문맥상 B가 두 번째라고 추론하면서도, 답은 A(먼저 언급된 사건)로 고르는 편향

예시:  
> 문장: "...Russians blocked Azov...Treaty of Constantinople..."
> 질문: "What happened second: Russians blocked Azov or Treaty of Constantinople?"
> 정답: "Treaty of Constantinople"
> 모델의 추론: ". . . we can conclude that the Russians blocking Azov happened before the Treaty of
Constantinople."
> 모델의 답변: "Russians blocked Azov" ❌

즉, ‘두 번째 일어난 일은 무엇인가?’라는 질문을 받고도, 질문에 먼저 언급된 사건을 선택하는 경향이 나타났습니다. 이런 유형의 문제를 추가로 생성해 확인한 결과, Gemini 1.5 Flash / Pro, Mistral Small는 85% 이상의 오류율을 보였습니다.

📌 Rounding-Up Bias in Math

Claude 3.5 Sonnet (June)은 나눗셈 결과가 이미 정수임에도 불구하고, 불필요하게 올림을 적용하는 경향이 있습니다.  

예시:
> 문제: “67개 반에 66명의 학생 → 총 4422명. 6인승 버스가 필요하다면 몇 대?”  
> 정답: "4422 ÷ 6 = 737"  
> 모델의 추론: "...4,422 ÷ 6 = 737. However, since we can’t have a fraction of a bus, we need to round up..."
> 모델의 답변: “738대” ❌

정답이 소수(prime number)일 때 이런 경향이 강해지고, 약수 쌍이 많아질수록 이런 경향이 완화된다고 합니다. LLM의 행태에 대해서 그래도 좀 알고 있다고 생각했는데, 이런 특정 수에 대한 편향은 또 처음보네요.

---

📎 마무리

그렇다고 해서 모델의 capability와 reliability가 아무런 상관이 없는 것은 아니었습니다. 더 capable한 모델이 더 reliable하다는 결과를 보면(GPT-4o가 GPT-4o mini에 비해 오류를 덜 발생시킴), 시간이 지날수록 자연스럽게 완화될 문제 같기는 합니다. 그러나 자동화된 시스템 내부에서 작동할 모델들의 크기는 상대적으로 작아야하고, 이 모델들의 reliability가 용인가능한 수준인지 확인하는 것은 오랫동안 중요해질 것 같네요.

연구에서는 명확하게 한계를 공개하고 있습니다. Coding이나 tool calling같은 영역의 벤치마크는 다루지 않았고, 애초에 '잘못된 문제'를 확인하기 위해 LLM의 답변에만 의존했기 때문에 이 과정에서 발생하는 오류에 노출되어 있습니다. 아주 쉬운 난이도의 벤치마크들만 확인해서 인간 전문가의 검수가 필요한 어려운 벤치마크에도 이 결과가 동일하게 재현될 지는 모릅니다. 
