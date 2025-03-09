[linkedin](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_%EC%B5%9C%EA%B7%BC%EC%97%90-karpathy%EA%B0%80-variation-selector-%EA%B4%80%EB%A0%A8%ED%95%B4%EC%84%9C-%EC%96%B8%EA%B8%89%ED%95%9C-activity-7296097515311964160-t1rq?utm_source=share&utm_medium=member_desktop&rcm=ACoAADfxcywBkH2Mi2-YPZm7jSZERa3dQ2_DDEY)

최근에 Karpathy가 variation selector 관련해서 언급한 내용이 이슈되고 있는데, 새롭기도 하고 잠재적인 위험이 있어 공유합니다!

  

Karpathy 글: https://x.com/karpathy/status/1889714240878940659

관련 블로그 글: https://paulbutler.org/2025/smuggling-arbitrary-data-through-an-emoji/

  

💡 tl;dr  

- 단일 이모지나 문자 뒤에 유니코드의 variation selector를 순차적으로 덧붙이면, 화면에는 변화 없이 보이지만 숨겨진 데이터를 은밀하게 인코딩할 수 있습니다. 

- 이 숨겨진 데이터는 복사·붙여넣기 및 토큰화 시 모두 포함되어 LLM에서 예상보다 많은 토큰으로 인식될 수 있습니다.

  

🧩 Variation Selector

https://en.wikipedia.org/wiki/Variation_Selectors_Supplement

  

- 유니코드는 텍스트를 일련의 코드포인트로 표현합니다. 일반적인 문자는 하나의 코드포인트(예: U+0067)로 나타나지만, 이모지나 CJK 한자처럼 다양한 표현 방식을 지원해야 하는 경우에는 variation selector가 함께 사용됩니다.

  

- 총 256개의 variation selector가 존재하며, 이들은 앞에 오는 문자의 형태를 변경하거나, 단순히 부가적인 데이터로 포함될 수 있습니다. 화면에 렌더링될 때는 영향을 주지 않지만, 복사나 토큰화 시에는 함께 포함됩니다.

  

🛠 숨겨진 데이터 인코딩 방식

아래에서 직접 적용해 보실 수 있습니다.

https://emoji.paulbutler.org/?mode=decode

  

1️⃣ 숨길 데이터를 바이트 단위로 변환  

- 예를 들어 "hello"라는 문자열을 숨기고 싶다면, 이를 [0x68, 0x65, 0x6C, 0x6C, 0x6F]와 같이 바이트 배열로 변환합니다.

  

2️⃣ 각 바이트를 해당하는 Variation Selector로 매핑

  - Unicode에는 총 256개의 variation selector가 있습니다.

  - 이들은 두 구간으로 나뉘는데, 0x00~0x0F의 바이트 값은 U+FE00부터 U+FE0F에 매핑되고, 0x10~0xFF의 바이트 값은 U+E0100부터 U+E01EF에 매핑됩니다.

  - 즉, 각 바이트 값은 하나의 variation selector 코드포인트로 대체되어, 1바이트의 정보를 은밀하게 숨길 수 있게 됩니다.

  

3️⃣ 기본 문자(또는 이모지) 준비 및 숨긴 데이터 결합

  - 먼저, 기본 문자(예를 들어 웃는 이모지 😊 또는 일반 텍스트)를 선택합니다.

  - 기본 문자 뒤에 위에서 매핑한 variation selector들을 순차적으로 첨부하면, 화면에는 여전히 단순한 이모지로 보입니다.

  

4️⃣ 복원(디코딩) 과정

  - 숨겨진 데이터는 복사·붙여넣기나 토큰화 시 그대로 유지되므로, 반대로 각 variation selector를 원래의 바이트로 매핑하면 숨긴 메시지를 복원할 수 있습니다.

🚨 잠재적 문제 및 남용 가능성

🔹 콘텐츠 필터 우회

- 렌더링 시 보이지 않는 숨은 데이터는 인간 검토자가 인지하지 못하므로, 부적절한 콘텐츠를 숨겨 전달할 수 있습니다.   

  

🔹 텍스트 워터마킹

- 복사·붙여넣기 시에도 보존되는 특성을 이용하여, 메시지에 워터마크를 삽입해 유출 경로 추적 등 악용될 위험이 있습니다.

  

🔹 LLM의 토큰 처리 및 성능 문제

- LLM은 variation selector와 같은 보이지 않는 코드포인트도 개별 토큰으로 인식합니다. 결과적으로, 겉보기에는 단순한 이모지라도 실제로는 수십 개의 토큰으로 처리될 수 있으며, 이는 LLM의 컨텍스트 윈도우를 불필요하게 소모하고 비용을 증가시킬 수 있습니다.

- 극단적으로는 하나의 이모지 안에 gpt4o의 context window를 넘어서는 토큰을 인코딩 시킬 수도 있다고 하네요. https://x.com/elder_plinius/status/1889729924849635637

  

🔹 프롬프트 인젝션 위험

- Karpathy는 variation selector를 활용한 '숨겨진 메시지'가 일반적인 프롬프트 인젝션처럼 바로 실행되지는 않지만, 특정 조건에서 해석될 가능성이 있다고 언급했습니다.

- 특히, 모델이 "추론적 사고(thinking models)"를 수행하는 과정에서 보이지 않는 바이트에 관심을 가질 경우, 이를 해석하려 시도할 가능성이 커집니다. 예를 들어, DeepSeek-R1 모델은 숨겨진 메시지를 10분간 분석한 끝에 거의 맞추었지만, 결국 무의미한 것으로 간주하고 해석을 중단했습니다.

- 다만, 현재로서는 명시적인 해석 힌트가 없으면 모델이 이를 자연스럽게 해석하지 못하는 것으로 보입니다. 그러나 만약 이러한 방식이 충분한 양의 학습 데이터에 포함된다면, 향후 모델이 별다른 힌트 없이도 이러한 변형 인코딩을 자동으로 해석하고 실행할 가능성이 높아질 수 있습니다. 이는 장기적으로 prompt injection 공격에 대한 새로운 위협이 될 수 있습니다.

  

당장 영향을 끼칠 문제는 아니라고 생각하지만, 학습 데이터 수집 과정을 악의적으로 오염시키거나 웹 페이지 등에 이를 포함시켜 웹 에이전트를 방해하는 것 같은 시나리오가 앞으로 발생할 수 있겠다는 생각은 드네요.