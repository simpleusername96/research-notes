2024.03.14

[Facebook](https://www.facebook.com/groups/agikr/posts/2255751434765902/)

<스페이스 하나의 차이>

오픈소스 vision language model인 moondream을 개발하고 있는 분이 트위터에 공유한 사례가 흥미로워서 가져왔습니다.
출처: https://twitter.com/felix_red_panda/status/1768058447747051688

모델의 학습과 추론 과정에서 쓰인 프롬프트에서 아래와 같이 마지막 공백을 지웠더니 성능이 유의미하게 향상되었다고 하네요.
"<image>\n\n{chat_history}Question: {question}\n\nAnswer: " ->
"<image>\n\n{chat_history}Question: {question}\n\nAnswer:"

다른 사람들의 설명을 읽어보니 다음과 같이 정리해볼 수 있을 것 같습니다.
1)토크나이저는 공백을 기준으로 토큰을 분류
2)그래서 토큰들은 공백으로 시작한다고 여기는 경우가 많음
3)예시와 같이 질문 프롬프트의 마지막에 공백이 포함된다면, 뒤에 오는 토큰은 공백을 포함하기 힘듬
4)따라서 맥락에 맞는 글을 생성하려면 익숙하지 않은 토큰을 사용해야 되기 때문에 답변의 품질이 저하됨 

이 문제를 해결하기 위해서는 "Capital of France is<mask>." 대신 "Capital of France is <mask>."와 같이 학습시켜 토큰에 공백을 적게 포함시키는 방법이 있다고 합니다.

실제로 프롬프트 마지막에 공백에 차이를 두고 간단한 프롬프트를 여러 번 돌려봤더니, 답변의 수준은 모르겠지만 공백에 따라 log_prob값이 명확히 달라지는 걸 볼 수 있었습니다.

이런 걸 보니 Karpathy가 왜 "LLM의 모든 문제는 tokenization 때문에 발생한다"고 말했는지 이해가 되네요 ㅋㅋㅋ

