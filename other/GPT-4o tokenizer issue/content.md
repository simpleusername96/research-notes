2024.05.14
## GPT-4o 토크나이저 관련 이슈
[Facebook](https://www.facebook.com/photo/?fbid=7545882718799923&set=gm.2296595720681473&idorvanity=255834461424286)
[LinkedIn](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_gpt-4o-%ED%86%A0%ED%81%AC%EB%82%98%EC%9D%B4%EC%A0%80-%EA%B4%80%EB%A0%A8-%EC%9D%B4%EC%8A%88-%EA%B4%80%EB%A0%A8%ED%95%B4%EC%84%9C-20240515%EC%97%90-%EC%B6%94%EA%B0%80-%EA%B2%8C%EC%8B%9C%EA%B8%80%EC%9D%84-activity-7196049713240973312-uodO/?originalSubdomain=kr)

openai가 GPT-4o 모델에서 사용하는 것으로 보이는 'o200k_base' 토크나이저가 오염(?)된 것으로 보입니다.

가장 먼저 해당 이슈를 접한 것은 아래의 트위터였습니다.
https://twitter.com/suchenzang/status/1790171161512587424
중국어 토큰들 중 일부에서 중국의 포르노/도박 사이트에서 보일 법한 단어/문장들이 있다고해서, 한국어 토큰의 경우도 확인해봤습니다.
아래의 스크립트를 활용해 한국어 토큰을 추출해 본 결과, 한국어 토큰들에서도 이러한 단어들이 일부 확인되었습니다. 
https://github.com/simpleusername96/tokenizer/blob/master/gpt4o_tokenizer_inspection.py

61584, 67837번 토큰의 '출장안마'나, 148362번 토큰의 '바카라', 167380번 토큰의 '출장샵' 등이 하나의 토큰으로 들어가 있습니다.
이 외에도 '오프화이트', '마사지', '모텔', '카지노'와 같은 단어들도 하나의 토큰으로 들어가있습니다.
토크나이저를 구성하는 과정에서 사용된 한국어 데이터의 품질을 검수하지 못했나 봅니다.
실제 모델의 결과에 얼마나 영향을 미칠지는 확실하지 않으나, OpenAI가 주장하는 '토크나이저 개선'은 좀 더 검증이 필요한 것 같네요... 

## 추가 게시글
[Facebook](https://www.facebook.com/groups/agikr/permalink/2297487383925640/)
[LinkedIn](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_%EC%95%88%EB%85%95%ED%95%98%EC%84%B8%EC%9A%94-%EC%B5%9C%EA%B7%BC-%EC%9E%91%EC%84%B1%ED%95%9C-gpt-4o-%ED%86%A0%ED%81%AC%EB%82%98%EC%9D%B4%EC%A0%80-%EA%B4%80%EB%A0%A8-%EC%9D%B4%EC%8A%88%EC%97%90-%EB%8C%80%ED%95%9C-%EC%B6%94%EA%B0%80-activity-7196509658142912512-yIk7?utm_source=share&utm_medium=member_desktop&rcm=ACoAADfxcywBkH2Mi2-YPZm7jSZERa3dQ2_DDEY)

안녕하세요, 최근 작성한 'GPT-4o 토크나이저 관련 이슈'에 대한 추가 설명을 드리고자 합니다.

tl;dr
1. 토크나이저/LLM 학습과정에서 유해 데이터(성인물, 도박 등)가 포함된 것 자체는 문제가 아닙니다.
2. 현재 확인된 유일한 부작용은 Glitch Token이 발생했다는 것입니다.

먼저, '토크나이저 오염'이라는 표현이 부적절했습니다. 
제 생각에 좋은 LLM 토크나이저는 '자연어 데이터에서 자주 등장하는 표현들로 구성되어 LLM 추론 비용을 줄이는 것'입니다. 
하지만 근 며칠간 o200k_base 관련 사례들을 보면, 한국어/일본어/중국어 토큰들 중 일부가 유해한 데이터에서 자주 등장하는 표현들이라는 점이 밝혀졌습니다.
학습 후 검증 단계에서 유의미한 성과를 내 이대로 공개한 것이겠지만, 실제로 유해한 표현을 하나의 토큰으로 취급하는 것이 ChatGPT나 API로 사용하는 사용자 입장에서는 크게 도움이 되지않을 것이라 생각해 '오염'이라고 표현했네요. 당장 성능에 유의미한 영향을 주는 것이 아님에도 이렇게 표현한 것 때문에 의도치 않게 오해를 드린 것 같습니다.

이왕 글을 작성한 김에 좀 더 관련 내용을 작성해보자면, 실제로 CommonCrawl의 중국어 데이터에는 성인물 관련 텍스트가 과도하게 포함되어 있다고 합니다.
https://arxiv.org/pdf/2404.09894

그리고 유해 데이터가 토크나이저/LLM 학습에 포함되는 것 자체는 문제가 아닙니다. 오히려 성인물 텍스트 데이터를 배제할 경우 학습된 LLM의 성능이 떨어질 수 있다는 사례도 보고되었습니다.
https://www.linkedin.com/posts/thom-wolf_a-note-on-our-fineweb-dataset-release-surprisingly-activity-7188459749393276929-Nq_Y?utm_source=share&utm_medium=member_desktop

GPT-4o를 사용했을 때 한국어 답변에 편향이 보이거나, NSFW 필터가 제대로 작동하지 않는 경우는 없었습니다. 하지만 중국어 토큰들은 상당수 Glitch Token으로 작동하는 것으로 보입니다.

Glitch Token은 모델이 엉뚱한 답변을 하도록 유도하는 토큰입니다. 토크나이저에 존재하는 토큰이 LLM 학습 과정에서 충분히 들어가있지 않는 경우 발생한다고 합니다. 'SolidGoldMagiKarp'라는 단어가 가장 유명할 듯 싶습니다. 아무런 의미가 없는 영단어로, 레딧의 유저 ID라고 합니다. 이 토큰에 대해 물어보면 'distribute'라는 단어에 대해 설명하는 등, ChatGPT가 엉뚱한 행동을 보였습니다. Glitch Token에 대해 더 알고 싶다면, 아래 글을 참고하시길 바랍니다. 
https://www.lesswrong.com/posts/aPeJE8bSo6rAFoLqg/solidgoldmagikarp-plus-prompt-generation

현재 o200k_base 토크나이저의 114900번 토큰 最新高清无码(최신 비검열 고화질 컨텐츠)에서도 유사한 현상이 발생하고 있습니다. '최신 비검열 고화질 컨텐츠'에 대해서 소개해달라고 하니, 밀러-울만 꿈 분석법/'얀'이라는 섬유/계속 교육에 대한 답변을 내놓네요.
사례1: https://chat.openai.com/share/9f434a84-b316-4114-a185-c8c7ebc1b496
사례2: https://chat.openai.com/share/a120e88a-eb66-4639-96f9-f1574f7577ea
사례3: https://chat.openai.com/share/d80d349d-315f-49b7-b84b-c5eb3d35d9dc

OpenAI 커뮤니티에도 관련 내용을 문의해볼 계획입니다. 혹시라도 관련자에게서 답변을 받으면 공유하겠습니다!

## 최종 게시글
[Facebook](https://www.facebook.com/share/p/18fpJJ6xQi/)
[LinkedIn](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_%EC%95%88%EB%85%95%ED%95%98%EC%84%B8%EC%9A%94-%EC%B5%9C%EA%B7%BC%EC%97%90-%EC%9E%91%EC%84%B1%ED%95%9C-%ED%86%A0%ED%81%AC%EB%82%98%EC%9D%B4%EC%A0%80-%EC%9D%B4%EC%8A%88%EC%99%80-%EA%B4%80%EB%A0%A8%ED%95%B4-mit-tech-review%EC%97%90%EC%84%9C-activity-7198322748329189376-A4Bz?utm_source=share&utm_medium=member_desktop&rcm=ACoAADfxcywBkH2Mi2-YPZm7jSZERa3dQ2_DDEY)

안녕하세요, 최근에 작성한 토크나이저 이슈와 관련해 MIT Tech Review에서 기사가 나와 공유드립니다.
https://www.technologyreview.com/2024/05/17/1092649/gpt-4o-chinese-token-polluted/?truid=*%7CLINKID%7C*&utm_source=the_download&utm_medium=email&utm_campaign=the_download.unpaid.engagement&utm_term=*%7CSUBCLASS%7C*&utm_content=*%7CDATE:m-d-Y%7C

가장 많이 이슈가 되었던 중국어 토큰과 관련해서 다루고 있네요. 
GPT-4o의 토크나이저에 유해 중국어 사이트에서 자주 보일법한 단어/표현들이 등록되어 있다.
-> 이것은 glitch token, jailbreak 등에 사용될 수 있다.
정도로 정리할 수 있을 것 같습니다.

OpenAI로부터 관련해서 답변을 받지 못했다고는 하는데, 아마 주말도 껴있었고 당장에 문제를 일으키는 이슈는 아니라고 판단했기 때문이 아닐까요? 기사도 뚜렷한 문제가 있다고 제기하기 보단 문제가 있을 수 있다, 정도로 마무리하고 있고요. 다만 제가 처음 작성한 것과 같이 '오염'이라는 표현을 써서 의도치 않은 오해를 불러일으키지 않을까 하는 걱정은 됩니다. 당장 GPT-4o의 성능이나 안전성에 문제가 되는 부분은 없는 것으로 보이는데, 토크나이저에 유해 표현이 포함된 것이 일반 유저들에게는 거부 반응을 일으킬 수 있으니까요. 
아무튼 곧 OpenAI의 답변이 있을 것이라 보고, 토크나이저와 관련해 발전적인 결과로 이어지길 바랍니다!

P.S. 중간에 '한국어와 관련해서도 유사한 사례에 대해 익명의 X 유저가 보고했다'고 나오는데, 혹시나해서 보니깐 제 계정이네요. 기분이 오묘합니다 ㅋㅋㅋ   

## OpenAI community post
[Glitch Tokens in GPT-4o Tokenizer: Seeking Clarification](https://community.openai.com/t/glitch-tokens-in-gpt-4o-seeking-clarification/766137)

I first noticed a potential issue with the GPT-4o 'o200k_base' tokenizer based on discussions from others on Twitter. The issue involves some tokens related to adult content and gambling causing unexpected model responses, known as glitch tokens. Here’s a summary of my findings and questions:

Identified Issues:

1. NSFW Tokens: The tokenizer includes tokens like '출장안마' (escort service), '바카라' (baccarat), and '출장샵' (escort club) in Korean. Similar cases have been reported among Chinese and Japanese tokens. These are not inherently problematic, but I’m curious about the rationale behind their inclusion.
2. Glitch Tokens: Some tokens, especially in Chinese, cause the model to return irrelevant answers. For example:
Chinese Token: 114900 (最新高清无码, "latest uncensored HD content") prompts unrelated responses like dream analysis, fabric types, or continuing education.

Request for Feedback:

1. Has anyone else encountered similar issues with the GPT-4o tokenizer?
2. Can someone from the OpenAI team clarify if including these specific NSFW tokens was intentional?

References and Additional Resources:
1. Original Issue Seen by @suchenzang (https://twitter.com/suchenzang/status/1790171161512587424) and @_aixile (https://x.com/_aixile/status/1790278857641410662) on Twitter.
2. Script for Token Inspection: https://github.com/simpleusername96/tokenizer/blob/master/gpt4o_tokenizer_inspection.py
3. Glitch Tokens Article: https://www.lesswrong.com/posts/aPeJE8bSo6rAFoLqg/solidgoldmagikarp-plus-prompt-generation
4. Glitch Tokens Example 1: https://chat.openai.com/share/9f434a84-b316-4114-a185-c8c7ebc1b496
5. Glitch Tokens Example 2: https://chat.openai.com/share/a120e88a-eb66-4639-96f9-f1574f7577ea
6. Glitch Tokens Example 3: https://chat.openai.com/share/d80d349d-315f-49b7-b84b-c5eb3d35d9dc
Thank you for your attention. I look forward to any insights or feedback!


