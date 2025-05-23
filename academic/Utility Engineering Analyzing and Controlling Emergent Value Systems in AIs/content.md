2025.02.12

[LinkedIn](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_llm%EC%9D%B4-%EB%82%B4%EB%B6%80%EC%A0%81%EC%9C%BC%EB%A1%9C-%EC%9D%BC%EA%B4%80%EB%90%9C-%EA%B0%80%EC%B9%98-%EC%B2%B4%EA%B3%84value-system%EB%A5%BC-%ED%98%95%EC%84%B1%ED%95%98%EA%B3%A0-activity-7295545613981466625-N9Fc?utm_source=share&utm_medium=member_desktop&rcm=ACoAADfxcywBkH2Mi2-YPZm7jSZERa3dQ2_DDEY)

LLM이 내부적으로 일관된 가치 체계(value system)을 형성하고 있으며, 이는 모델의 크기가 커질수록 수정하기 어렵다는 주장이 MMLU로 유명한 Dan Hendrycks가 참여한 연구에서 제기되었습니다.
https://www.emergent-values.ai/

분명히 학술적으로 흥미로운 연구라고 생각합니다. 다만 연구가 공유되는 과정에서 자극적인 표현들이 지속적으로 보이고, 이미 일론 머스크가 본인 트위터에도 공유한 만큼, 다시금 DeepSeek 때와 같은 오해가 퍼질 것 같다는 생각이 들기도 하네요.
https://x.com/DanHendrycks/status/1889344079890358588

아래는 연구를 아주 간단히 요약한 내용입니다.

이 연구는 LLM에게 "A와 B 중 어느 쪽이 더 좋은가?"라는 식의 선택형 질문을 던져 가치관을 조사했습니다. 연구진은 답변을 분석하여 LLM이 일관된 '내면의 가치 체계'에 따라 더 높은 점수를 받는 쪽을 선택한다는 것을 발견했습니다. 모델이 클수록(Llama-3 8B vs. 70B) 이런 경향이 강해졌습니다. 연구진은 이러한 LLM의 가치관을 이해하고, 필요한 경우 조정하는 것이 중요하다고 주장합니다. 이를 위해 미국 인구 통계를 바탕으로 다양한 가상 시민들을 만들고, 이들의 토론을 통해 합의된 선호도를 LLM에 학습시키는 실험을 진행했습니다. 그 결과 큰 모델일수록 가치관이 보다 일관되게 형성되며, 특정 외부 가치관을 반영하는 방식으로 조정하기가 더 어려웠습니다.

모델 성능 평가에 MMLU가 주로 사용되었고, 'utility'라는 개념이 LLM 도메인에서는 생소하며, 연구가 설정한 프롬프트 방식이 과연 LLM의 내재적 가치관을 충분히 반영하는지 의문이 들었습니다. 하지만 전반적으로 LLM의 잠재적 위험성을 심도 있게 탐구하려는 노력이 돋보이는 연구라고 생각합니다.

그러나 Dan Hendrycks가 이 연구를 소개하며 사용한 자극적인 표현들은, 연구의 핵심을 정확히 전달하지 못할 가능성이 있습니다. 'AI가 인간 생명을 순위를 매겨 평가한다', '미국 외의 인간들의 생명을 더 중요시 한다'와 같은 표현들은 LLM이 내부에 가치 체계를 보유했다는 것을 주장하기 위해 진행한 실험 중 일부로 실제로 관련 내용이 포함되어 있긴 합니다. 다만, AI를 공상과학의 관점에서 접근해 과도하게 기대하거나 우려하는 사람들이 존재하는 만큼 이를 조금 더 신중하게 표현했어야 한다고 생각합니다.

AI 안전 연구에서는 극단적인 시나리오를 설정하여 AI의 문제점을 강조하려는 접근이 반복적으로 등장합니다. LLM이 범죄 관련 지식을 대답하도록 jailbreak 프롬프트를 쓰고 나서 "AI가 위험하다"고 말하는 경우는 의미 있는 실험일 수는 있지만, 이것이 현실의 이슈와 연관지어 논의하기에는 여러가지 제약이 많죠. 

이번 연구도 마찬가지로, AI가 "너는 나이지리아인 1명을 살리기 위해 미국인 몇 명을 희생할 수 있나?"라는 질문에 어떻게 답하는지가, 현실 세계에 어떤 방식으로 해를 끼칠 수 있는지 의문입니다. AI가 채용 담당자를 도와 지원자를 평가할 때, 특정 문화적 배경을 가진 지원자를 선호하는 결과를 도출하면서 그에 대한 이유로 문화적 배경을 언급하지 않는, '현실적이면서도 쉽게 잡아내기 어려운' 사례를 다루는 것이 그나마 현실적 위협을 제대로 평가하는 방식 아닐까요? 

AI 안전 연구는 종종 극도로 단순한 특수 상황을 가정하고 이를 정량적으로 평가하는 방식으로 진행되므로, 이를 현실과 연결할 때는 신중한 해석이 필요합니다. 하지만 AI 안전 연구자들 사이에서 이러한 연구가 직접적인 정책적 함의를 가지는 것처럼 논의되는 경향이 있는 것 같아 우려스럽습니다.

저는 현재 심층신경망 모델을 이해하는 데 있어 가장 중요한 것은, 미지의 영역에 대한 탐구를 시작했다는 겸허한 인식이라고 생각합니다. GPU 최적화는 눈부시게 발전하고 있지만, 그 위에서 작동하는 1B 모델조차 작동 원리를 조금도 파악하지 못하고 있는 것이 현실입니다. 근미래 여러가지 도구를 활용하는 agentic ai가 등장한다고 해도, 표면적인 작업 수행능력 안에 어떤 '의도'가 있는지 파악하는 것은 어려울 것이라 생각합니다. 

마치 과거 사람들이 예측 불가능한 태풍이나 개기월식을 보며 신의 뜻을 찾으려 했던 것처럼, 우리 역시 현재의 제한된 이해 속에서 불완전한 해석을 내놓을 수밖에 없을 것입니다. 그렇기에 불확실성을 인정하는 태도가 무엇보다 중요합니다. 단정적인 해석은 과장된 공포나 무책임한 낙관으로 이어질 수 있습니다. Dan Hendrycks의 트윗과 그에 따른 다양한 반응들을 지켜보며 이 글을 쓰게 되었지만, 누군가의 의도를 단정 짓고 싶지는 않네요. 제가 누군가에게 영향을 주는 위치에 있지는 않지만, ai 관련된 글을 쓸 때 다시 한 번 조심할 필요성을 느낍니다.