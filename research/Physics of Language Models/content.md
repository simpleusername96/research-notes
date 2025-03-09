[linkedin](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_llm%EC%9D%B4-%EB%B8%94%EB%9E%99%EB%B0%95%EC%8A%A4%EB%9D%BC%EA%B3%A0-%EB%B6%88%EB%A6%AC%EB%8A%94-%EC%9D%B4%EC%9C%A0%EB%8A%94-%EB%8B%A8%EC%88%9C%ED%9E%88-%EA%B7%B8-%EB%82%B4%EB%B6%80-%EC%9B%90%EB%A6%AC%EA%B0%80-%EB%AA%85%ED%99%95%ED%9E%88-%EB%B0%9D%ED%98%80%EC%A7%80%EC%A7%80-activity-7244739867836874755-wE4A?utm_source=share&utm_medium=member_desktop)
[facebook](https://www.facebook.com/share/p/17SFHesvxf/)
[medium](https://medium.com/@simple0314/physics-of-language-model-39284ee10923)

LLM이 "블랙박스"라고 불리는 이유는 단순히 그 내부 원리가 명확히 밝혀지지 않았기 때문만이 아닙니다. 상업적으로 제공되는 모델은 물론, 오픈소스 모델조차도 학습 데이터나 학습 방법에 대한 정보가 제대로 공개되지 않습니다. 이런 상황에서 LLM 연구는 수학적 이론을 엄밀하게 증명하는 방식보다는, 마치 동물행동학자가 동물의 행동을 관찰하듯이, 모델의 겉으로 드러나는 결과만을 가지고 이루어지고 있습니다.

이런 문제의식을 바탕으로 Meta의 FAIR AI 소속 Zeyuan Allen-Zhu 연구팀은 '언어 모델에 대한 통제된 실험'의 필요성을 역설하며 대규모 실험을 진행했습니다. GPT2 정도로 작은 모델을 처음부터 합성 데이터를 활용해 학습시키고, 언어 모델의 '지능'을 지식, 추론, 언어 구조라는 세 가지 요소로 분리하여 접근했습니다.

총 6개의 논문으로 이루어졌을 정도로 그 내용이 방대해 모두 다루기는 어렵지만, 일부 흥미로운 결과들을 간단히 공유하고자 합니다. 연구의 결과 뿐 아니라 접근 방법도 인상 깊습니다. 비록 당장 실무에 적용할 수 있는 내용은 아니나 LLM에 대해 깊이 있는 이해를 원하는 분들이라면 꼭 관심을 가져보시길 추천 드립니다.

Youtube: https://www.youtube.com/@Zeyuan-AllenZhu
Project page: https://physics.allen-zhu.com/home
Huggingface: https://huggingface.co/zhuzeyuan

### Pretraining과 데이터 다양성의 중요성
![Part 3.1 Knowledge Storage and Extraction (1).PNG](<images/Part 3.1 Knowledge Storage and Extraction (1).PNG>)
![Part 3.1 Knowledge Storage and Extraction (3).PNG](<images/Part 3.1 Knowledge Storage and Extraction (3).PNG>)
실험 결과 언어 모델의 성능은 pretraining 단계에서 사용된 데이터의 질과 다양성에 크게 좌우된다는 것이 확인되었습니다. Pretraining 과정에서 QA 데이터셋을 포함시키면 모델의 지식 추출 능력이 크게 향상되었으나, Fine-tuning 단계에서 포함시킬 경우 테스트 데이터에서 0%에 가까운 성능을 보였습니다. 

![Part 3.1 Knowledge Storage and Extraction (2).PNG](<images/Part 3.1 Knowledge Storage and Extraction (2).PNG>)
데이터의 다양성 또한 모델 성능에 중요한 영향을 미칩니다. 연구에서는 동일한 정보를 문장 구조를 바꾸거나 다른 언어로 번역해 pretrain 데이터에 포함할 경우, 모델의 성능이 현저히 개선되는 것을 확인했습니다. 이를 '데이터 증강(Data Augmentation)'이라고 하는데, 모든 데이터가 증강 될 필요는 없었습니다. 가수나 정치인처럼 데이터가 많은 인물의 정보가 다른 형태로 학습 데이터에 포함된다면, 보다 데이터가 적은 인물에 대해서도 정확하게 정보를 추출할 수 있었습니다. 

### Reversal Curse 
![Part 3.2 Knowledge Manipulation.PNG](<images/Part 3.2 Knowledge Manipulation.PNG>)
언어 모델이 "A는 B다"라는 정보를 학습했을 때, "B는 A다"라는 반대 관계를 자동으로 추론하지 못하는 "Reversal curse" 현상을 다시금 확인했습니다. 모델의 구조나 학습 방법을 다양하게 시도해도 이는 완화되지 않았으며, 오로지 "B는 A다"라는 정보가 학습 데이터에 포함된 경우에만 모델이 인지할 수 있었습니다. 

### 저품질 데이터의 영향
![Part 3.3 Knowledge Capacity Scaling Laws.PNG](<images/Part 3.3 Knowledge Capacity Scaling Laws.PNG>)
저품질 데이터를 포함한 학습은 오히려 모델 성능을 저하시킬 수 있습니다. 이를 해결하기 위한 방법으로 고품질 데이터의 출처를 명시하는 도메인 태그를 활용하는 방안이 제시되었습니다. 예를 들어 위키피디아 데이터 앞에 "<Wikipedia.org>"와 같은 태그를 추가하면, 모델이 학습 과정에서 고품질 데이터를 우선적으로 처리하게 되어 성능 저하를 방지할 수 있었습니다.

### 추론 과정에서의 실수
![Part 2.2 How to Learn From Mistakes on Grade-School Math Problems.PNG](<images/Part 2.2 How to Learn From Mistakes on Grade-School Math Problems.PNG>)
언어 모델은 추론 과정에서 가끔 불필요한 파라미터를 계산하거나, 아직 계산할 수 없는 파라미터를 정의하려다 실수를 저지릅니다. 이러한 실수는 모델이 문제를 풀기 전 내부에서 이미 예측할 수 있었습니다. 이는 실수가 언어 모델의 확률적 생성 과정에서 발생하는 것이 아니라, 추론 자체에서 비롯된 체계적인 문제임을 보여줍니다.

이를 해결하기 위해 연구팀은 모델이 스스로 실수를 인식하고 수정할 수 있는 능력을 학습하도록 설계했습니다. 실수를 했으니 수정하라는 의미의 '[BACK]' 토큰이 포함된 데이터를 모델에 학습시키면, 모델은 실수를 줄이고 더 정확한 추론을 수행할 수 있게 되었습니다. 학습 데이터의 반 정도가 실수와 관련되어도 모델의 성능은 더욱 개선되었으며, 가짜 실수를 활용한 경우에도 어느 정도 성능 개선이 이루어졌습니다.
