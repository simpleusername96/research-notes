2025.02.08

[Linkedin](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_%EC%96%B4%EC%A0%9C-%EC%B9%98%EB%A4%84%EC%A7%84-%EB%AF%B8%EA%B5%AD-%EC%88%98%ED%95%99%EA%B2%BD%EC%8B%9C%EB%8C%80%ED%9A%8C-aime-2025-part1%EC%9D%84-%EA%B8%B0%EB%B0%98%EC%9C%BC%EB%A1%9C-llm%EB%93%A4%EC%9D%98-activity-7293903404089675776-b--k?utm_source=share&utm_medium=member_desktop)

어제 치뤄진 미국 수학경시대회 AIME 2025 part1을 기반으로 LLM들의 수학 추론 능력을 평가한 결과가 공개되었습니다. 

AIME 2025 part1 문제: https://olympiads.us/past-exams/2025-aime-i
MathArena 결과: https://matharena.ai/
담당자의 SNS 게시글: https://x.com/mbalunovic/status/1887962694659060204

o3-mini(high)가 가장 뛰어났으며, o1 (medium), o3-mini (medium), DeepSeek-R1가 뒤따르고 있습니다.
R1에서 distill된 모델들도 상당히 뛰어난 결과를 보였는데, DeepSeek-R1-Distill-Qwen-1.5B이 gpt-4o나 claude-3.5-sonnet보다 월등히 높은 점수를 받았습니다.

별 이슈가 없다면 '추론 능력에 특화된 LLM은 단순히 학습 데이터에 있는 답을 외우는 것이 아니라 새로운 문제에 대해 일반화할 수 있고, distill을 적용한 작은 모델들은 범용적인 모델들 보다 더 뛰어난 성과를 보일 수 있다'라고 결론을 내릴 수 있겠지만, 조금 더 추이를 봐야할 것 같네요.

Microsoft Research 소속의 Dimitris Papailiopoulos가 OpenAI의 Deep Research를 활용해 검색했더니, 인터넷 상에 AIME 2025 Q1/Q3/Q7와 유사한 문제가 있음을 확인했습니다. 
https://x.com/DimitrisPapail/status/1887977460664352795

AIME 정도면 이런 논란이 없을 줄 알았는데, 예상 외로 데이터 오염 이슈가 나왔네요. 그래도 일부 문제가 인터넷에 있었다고 해도, distill 모델들이 기대 이상으로 좋은 성과를 낸 건 꽤 인상적입니다. 앞으로도 더 작은 모델들이 어디까지 발전할 수 있을지 지켜볼 만하겠네요.




