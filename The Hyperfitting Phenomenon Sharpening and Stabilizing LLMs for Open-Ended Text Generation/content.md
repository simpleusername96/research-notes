[LinkedIn](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_%EA%B3%BC%EC%A0%81%ED%95%A9%EC%9D%B4-%EC%98%A4%ED%9E%88%EB%A0%A4-llm-%EC%84%B1%EB%8A%A5%EC%9D%84-%EB%86%92%EC%9D%B8%EB%8B%A4-hyperfitting-%ED%98%84%EC%83%81-activity-7287865122071449600-Wv75?utm_source=share&utm_medium=member_desktop)

📢 과적합이 오히려 LLM 성능을 높인다? "Hyperfitting" 현상 소개  
최근 LLM 학습에서 과적합이 오히려 생성 텍스트의 품질을 향상시킬 수 있다는 흥미로운 연구 결과가 발표되어 간단히 정리해봤습니다!  
  
"The Hyperfitting Phenomenon: Sharpening and Stabilizing LLMs for Open-Ended Text Generation"  
🔗 [https://arxiv.org/pdf/2412.04318](https://arxiv.org/pdf/2412.04318)  
  
🔍 Hyperfitting이란?  
일반적으로 머신러닝에서는 모델이 훈련 데이터에 지나치게 맞춰지는 '과적합'을 피해야 한다고 알려져 있습니다. 하지만 Hyperfitting은 역설적으로 LLM을 소량의 데이터(2,000개 샘플)에 20epoch 이상 반복 학습하여 Training Loss를 0에 가깝게 과적합시키는 전략입니다. 기존의 LLM 평가지표인 Perplexity와 Validation Loss는 악화되지만, 답변 생성 시 더 다양한 어휘를 안정적으로 사용해 인간 평가자들에 의해 더 선호되었습니다.  
  
💡 Hyperfitting의 핵심 효과: 4가지  
1️⃣ 어휘 다양성 증가  
→ Hyperfitting된 모델은 생성 텍스트에서 훨씬 다양하고 풍부한 어휘를 사용합니다.  
→ 원본 모델: "그는 말했다. 그는 말했다. 그는..."  
→ Hyperfitting 모델: "그는 말했다. 그녀는 고개를 끄덕이며 동의를 표했다. 그리고 방 안의 분위기가 바뀌었다..."  
  
2️⃣ 선명해진 예측 분포  
→ 모델이 예측하는 확률 분포가 뾰족해집니다. 즉, 가장 높은 확률을 가진 토큰(Top-1)에 대한 예측 확률이 크게 증가합니다.  
→ 이는 모델이 특정 답변에 대해 강한 확신을 갖게 된다는 것을 의미합니다.  
→ 틀린 답변에 대해서도 강한 확신을 가지고 답변하는 경향을 보입니다.  
  
3️⃣ 긴 텍스트 생성 시 안정성 향상  
→ 문장이 길어질수록 동일한 구절을 반복하는 현상이 줄어들어, 더 일관되고 자연스러운 텍스트를 생성합니다.  
  
4️⃣ 데이터 의존성 약화  
→ Hyperfitting에 사용된 데이터의 순서를 바꾸거나, 다른 도메인의 데이터를 사용해도 유사한 수준의 성능 향상을 보였습니다.  
  
❗ Top-Rank Encouragement  
→ Hyperfitting된 모델은 훈련 데이터에 극단적으로 과적합되기때문에 Validation Loss와 Perplexity가 크게 증가하지만, 인간 평가자들은 이를 더 선호합니다.  
→ 연구진은 Hyperfitting이 모델로 하여금 단순히 훈련 데이터의 다음 토큰을 잘 예측하는 것을 넘어서, 사람이 보기에 더 자연스럽고 적절한 텍스트를 생성하도록 유도한다고 주장합니다.  
→ 이를 설명하기 위해 Top-Rank Encouragement 가설을 제안하며, Top-rank에 사람이 보기에 더 자연스럽고 적절한 토큰들이 위치하도록 학습되는 것은 낮은 training loss를 통해 학습할 때 나타나는 현상으로 보인다고 설명합니다.  
  
🚨 Hyperfitting과 기존 현상들과의 차이점  
→ Hyperfitting: Training Loss가 0에 가까워질 때 빠르게 성능이 향상되며, 거대하고 사전 학습된 LLM의 Sequence Generation에서 나타납니다. Validation Loss는 지속적으로 상승하며 Weight Decay를 적용하지 않습니다.  
→ Grokking: Training Loss가 0에 수렴한 후 한참 뒤에 Validation Loss가 급격히 감소합니다. Weight Decay가 핵심 역할을 하는 것으로 추정됩니다.  
→ Double Descent: Training Loss와는 직접적인 관련 없이, 모델/데이터 크기에 따라 Test Error가 일시적 상승 후 다시 하락하는 현상입니다.  
  
📌마무리  
→ 코드를 공개하지 않아, 직접 동일한 설정으로 재현해봤더니 확실히 문장 반복이 줄고 다양한 어휘를 사용했습니다. 아직 밝혀진 것이 별로 없긴 해도 분명히 흥미롭고 재현하기 쉬우니 논문 읽어보시는 것 추천드립니다!

