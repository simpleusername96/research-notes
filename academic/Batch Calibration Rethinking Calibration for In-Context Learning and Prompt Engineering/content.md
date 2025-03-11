2024.04.08

[Facebook](https://www.facebook.com/groups/agikr/posts/2271661406508238/?__cft__[0]=AZVRuDUyQpLghrhtXhcdkcBwCjLmZs-DQDuhF9t7PhDE7y-DkPE-bh7QaricvyPotJUeQzfU-fdedmRBVS4TtBQjEGp1QEeCYuY_1rAIqekM4hhnVXe-dCiCkoPQpvSV9wVRmBbihQA5HK7RF4iJsRIcusO2NDunzFNB-VCMnQ2GAOPC0xKRw_74EuUe7o5a4exoGHsk8DSh0BiDw5md0Gh3BzARsEZtxWKzgmbbi-bsKrSBXoyTsBt9EjX_xlvfB1o&__tn__=%2CO%2CP-y-R)

안녕하세요, LLM 답변의 신뢰도를 향상시킬 수 있는 Batch Calibration이라는 방법론을 접하게 되어 소개해드리고자 합니다!

Batch Calibration: Rethinking Calibration for In-Context Learning and Prompt Engineering
논문: [https://arxiv.org/pdf/2309.17249.pdf](https://arxiv.org/pdf/2309.17249.pdf?fbclid=IwZXh0bgNhZW0CMTAAAR1XNsGV8jefxUigfgl-9-KTHgDc4D1s14hAnfq5qBrJYjEb12IBc75JeXo_aem_ffxj8Yo12YF3B63KNqB2zg)

저자 X: [https://twitter.com/hanzhou032](https://twitter.com/hanzhou032?fbclid=IwZXh0bgNhZW0CMTAAAR1XNsGV8jefxUigfgl-9-KTHgDc4D1s14hAnfq5qBrJYjEb12IBc75JeXo_aem_ffxj8Yo12YF3B63KNqB2zg)

In-context learning (ICL)은 LLM의 성능을 끌어올리는 방법이지만, 프롬프트의 작은 변화에도 성능이 크게 요동칠 수 있다는 한계가 있습니다. 이를 해결하기 위해 그동안 content-free token이나 random token을 활용한 교정(calibration) 방법들이 제안되어 왔습니다.

하지만 저자들은 기존 방법들을 분석하면서 몇 가지 문제점을 발견했고, 이를 해결하기 위해 Batch Calibration (BC)을 고안했습니다. BC의 핵심은 ICL 내 지시문과 예시문들의 문맥 편향(contextual bias)을 측정하고 보정하는 것이죠.

예를 들어 "긍정 0.6, 부정 0.4"와 같이 영화 리뷰의 감성을 분석하는 작업이 있습니다. 우선, 각 리뷰에 대해 모델이 예측한 긍정/부정 확률의 평균을 구해 편향을 추정합니다. 그리고 각 리뷰의 확률에서 이 편향을 빼주는 식으로 점수를 보정합니다. 마지막으로 보정된 점수를 정규화해서 최종 예측을 내립니다. 레이블된 데이터가 조금이라도 있다면, 좀 더 정교하게 교정할 수 있다고 합니다.

BC는 분류처럼 정답이 뚜렷한 작업에 국한되어 적용할 수 있습니다. 하지만 이런 작업에서 만큼은 거의 추가 비용 없이 편향을 제거해 답변을 해석하는 것을 보다 쉽게 만들어 줄 것으로 기대됩니다.