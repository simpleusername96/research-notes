2025.03.21

[LinkedIn](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_openai%EA%B0%80-%EC%83%88%EB%A1%9C%EC%9A%B4-stttts-%EB%AA%A8%EB%8D%B8%EC%9D%84-api%EB%A1%9C-%EC%B6%9C%EC%8B%9C%ED%96%88%EC%8A%B5%EB%8B%88%EB%8B%A4-https-activity-7308656948667654145-az2M?utm_source=share&utm_medium=member_desktop&rcm=ACoAADfxcywBkH2Mi2-YPZm7jSZERa3dQ2_DDEY)

OpenAI가 새로운 STT/TTS 모델을 API로 출시했습니다!

https://openai.com/index/introducing-our-next-generation-audio-models/

- gpt-4o-transcribe/gpt-4o-mini-transcribe 모델은 Whisper 대비 단어 인식 오류가 감소했고, 다양한 악센트와 잡음이 있는 환경에서의 성능도 개선되었다고 합니다. gpt-4o-mini-tts 모델은 감정과 톤을 조절할 수 있는 'steerability'를 제공합니다. gpt-4o 기반 stt모델은 있는데 tts 모델은 없네요.
- gpt-4o/gpt-4o-mini 아키텍처 위에 음성에 특화된 대규모 데이터셋으로 pretrain되었습니다.
- STT에 RL을 적용했다고 하는데, 아무래도 STT가 TTS보다 정답이 명확한(verifiable) 구조다 보니 RL을 활용해 정밀도를 높일 수 있었던 것으로 보입니다.
- Distillation 과정에서 self-play 기법이 적용됐다고 하는데, 큰 모델과 작은 모델들이 서로 대화를 하면서 학습한 걸까요?
- 새로운 4o-mini 기반 TTS 모델을 체험할 수 있는 사이트도 함께 공개했습니다.
	https://www.openai.fm
	전반적으로 기계음이 거슬리는데, 그래도 다양한 목소리를 활용할 수 있고, 톤 조절도 확실히 반영됩니다. 한국어도 조금 어색하지만 꽤 잘되는 것 같네요.
- gpt-4o의 end-to-end 방식으로 음성을 처리하는 것보다는 훨씬 저렴한 것 같고 기존에 제공하던 Whisper와 TTS와 비슷한 가격대인 거 같습니다만, 가격이 직관적으로 제시되지 않아서 정확한 비교는 어려운 것 같습니다.
- 이번에 엔비디아에서도 음성 인식 모델 Canary를 상업적 사용 가능한 라이센스로 출시했던데, 출시 시기를 보니 아마 이 모델을 겨냥한 것 같기도 합니다 ㅋㅋㅋ
	https://huggingface.co/nvidia/canary-1b-flash