[LinkedIn](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_llm-%EC%8B%A4%ED%8C%A8-%EC%82%AC%EB%A1%80-1311%EC%9D%B4-138%EB%B3%B4%EB%8B%A4-%ED%81%AC%EB%8B%A4%EA%B3%A0-%EC%83%9D%EA%B0%81%ED%95%98%EB%8A%94-gpt-4o-activity-7218853811015049217-r_4M?utm_source=share&utm_medium=member_desktop&rcm=ACoAADfxcywBkH2Mi2-YPZm7jSZERa3dQ2_DDEY)
# LLM 실패 사례: 13.11이 13.8보다 크다고 생각하는 GPT-4o

X를 보다 흥미로운 사례를 보게 되어 실험해봤습니다.
https://x.com/billyuchenlin/status/1812948314360541302
위 포스트에서는 OpenAI의 최신 모델인 GPT-4o가 13.11과 13.8 중 어느 것이 더 크냐는 질문에 13.11이 크다고 잘못 답변하는 것을 보여주고 있습니다. 댓글에는 유사한 사례들이 다수 공유되었습니다.

저도 직접 실험을 진행해본 결과, GPT-4o는 대부분의 경우 숫자를 정확히 비교하지만 특정 조건에서 오류를 범하는 경향이 있음을 확인했습니다: 
1. 두 숫자의 정수 부분이 같고,
2. 더 작은 숫자가 x.1x 형태(소수점 둘째 자리까지 표현되며 첫째 자리가 1)이고, 더 큰 숫자가 x.x 형태(소수점 첫째 자리까지만 표현)일 때
특히 오류가 자주 발생했습니다.

## 실험 사례
Case 1: 2.11과 2.2 비교
https://chatgpt.com/share/b6dddc69-d008-4855-9a3e-403a98a4a9a5

Case 2: 12.11과 12.2 비교
https://chatgpt.com/share/b1d89ccf-ad49-455c-925e-86d473a4fa46

Case 3: 2.11과 2.9 비교
https://chatgpt.com/share/d2f533f3-d1f8-4a2d-8be4-3872e362a4f1

Case 4: 3.11과 3.3 비교
https://chatgpt.com/share/7481fe47-cb48-4feb-898b-76fe5a3368c6

Case 5: 7.11과 7.3 비교
https://chatgpt.com/share/2b0b673f-53e1-4ebf-bf7a-0e9a91f7df46

이 오류는 일반적인 사용 사례에는 거의 영향을 미치지 않으며, 항상 발생하는 것도 아닙니다. 모델이 첫 답변에 대한 근거를 설명하는 경우 대개 스스로 오류를 수정합니다. 또한, 간단한 프롬프트 엔지니어링으로 이 문제를 해결할 수 있을 것으로 보입니다.

그러나 GPT-4나 Claude 3.5와 같은 최첨단 모델들이 이렇게 간단한 문제에서도 지속적으로 오류를 범하는 것을 보면, LLM을 일상생활에서 범용적으로 활용하기에는 아직 신뢰성이 부족하다는 생각이 듭니다.

이러한 사소한 오류는 대부분의 경우 큰 문제가 되지 않겠지만, 가장 발전된 AI 모델조차도 예상치 못한 약점이 있을 수 있다는 점을 상기시킵니다. 