2025.03.14

[LinkedIn](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_gemma3-%EB%9D%BC%EC%9D%B4%EC%84%BC%EC%8A%A4%EB%8A%94-apache-20%EB%82%98-mit%EA%B0%80-%EC%95%84%EB%8B%99%EB%8B%88%EB%8B%A4-gemma%EB%9D%BC%EB%8A%94-activity-7306176567766827008-UaFJ?utm_source=share&utm_medium=member_desktop&rcm=ACoAADfxcywBkH2Mi2-YPZm7jSZERa3dQ2_DDEY)

Gemma3 라이센스는 Apache 2.0나 MIT가 아닙니다!
'gemma'라는 별개의 라이센스를 적용하고 있고, 이에 따르면 구글은 자신들이 세운 규칙에 어긋난다고 판단하면 언제든지 Gemma3와 이를 수정한 모델, 모델 결과물로부터 학습한 증류(distil) 모델에 대해 '배포를 중단하고 삭제하도록' 요구할 수 있습니다.

Gemma Terms of Use: https://ai.google.dev/gemma/terms
Gemma Prohibited Use Policy: https://ai.google.dev/gemma/prohibited_use_policy

Gemma3부터 적용된 라이센스는 아닙니다. Gemma2에도 같은 라이센스가 적용되고 있습니다. 또한 Llama의 라이센스도 비슷한 내용을 담고 있죠. 실제로 이에 기반해 구글이나 메타가 모델 배포 및 사용 중단을 요구하는 것이 가능한 지에 대해서는 논란의 여지가 있겠습니다만, 일단 관심을 가지게 된 김에 조금 정리해봤습니다.

모델이 생성한 결과물은 'output'으로, 증류 모델을 포함한 수정된 모델들은 'model derivatives'로 구분하고 있고, 'output'에 대해서는 '아무런 권리를 주장하지 않지만', 'model derivatives'에 대해서는 '권리를 행사할 수 있다'라는 해석은 신선하네요.

'Gemma Terms of Use'에는 '3.2 Use Restrictions'라는 항목이 있고, 안에는 'Gemma Prohibited Use Policy'를 포함하고 있습니다. 

명백한 범죄 외에도, 
- 의료, 금융, 법률 등 민감한 영역에서 자동화된 결정(automated decision)을 내리는 경우 ✅
- 모델의 안전 조치(safey filter)를 우회하거나 덮어 씌우는 경우
- 다른 사람에게 불공정하거나 악한 영향을 끼칠 수 있는 경우
- 스팸을 생성하거나 유통하는 경우
등을  금지하는 내용이 세세하게 포함되어 있습니다. 자동화 된 결정을 금지하는 부분이 흥미롭네요.

'4.5 Term, Termination, and Survival'에는 이를 어길 경우 사용과 배포를 전부 중단해야 한다고 명시합니다.

이렇게라도 모델을 공개해주는 것에 대해 고맙고, 모델을 악용하는 것을 막는 것이 중요하다고 생각하며, 라이센스에 의해 실제로 영향 받은 사례는 아직 없는 것 같습니다. 다만 원본 모델 뿐 아니라 광범위한 모델에 대해 권리를 행사하고, 사용처를 규제하고 이를 어길 시 적극적으로 개입할 수 있다고 명시한 것은 기업 입장에서는 우려할 만하네요. 원격으로 삭제할 수 있는 코드를 삽입한 것은 아니지만, 구글과 법적 분쟁에 얽히고 싶은 기업은 아무도 없을테니까요.

LLM을 보수적으로 접근하시는 분들은 이런 선제적 규제를 환영하시겠습니다만, 안전을 명분으로 경쟁자를 제어하고 오픈소스 생태계에 영향을 미치는 행위가 아닌지...

처음에 오픈소스와 오픈웨이트를 구분해야 한다고 주장하던 분들이 조금 과민반응하는 거 아닌가 하는 생각도 들었습니다. 하지만 이번 Gemma3의 라이센스를 훑어보고나니, 쉽게 어떤 모델을 오픈소스라고 부르는 것을 자제해야겠다는 생각이 드네요.

모델이 고도화되고 경량화 될 수록 이런 규제와 사상적 갈등이 현실화될 것으로 보입니다. 정치처럼 극단으로 치닫지 않고, 서로 대화를 이어나가며 절충안을 만들어냈으면 합니다.