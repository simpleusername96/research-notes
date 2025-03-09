[LinkedIn](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_microsoft%EC%9D%98-phi-%EB%AA%A8%EB%8D%B8-%EC%8B%9C%EB%A6%AC%EC%A6%88%EA%B0%80-%ED%95%A9%EC%84%B1-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%A1%9C-%ED%9B%88%EB%A0%A8%EB%90%98%EC%96%B4-%EB%86%80%EB%9D%BC%EC%9A%B4-%EC%84%B1%EB%8A%A5%EC%9D%84-activity-7221919789428924416-sn8K?utm_source=share&utm_medium=member_desktop)


Microsoft의 Phi 모델 시리즈가 합성 데이터로 훈련되어 놀라운 성능을 보여주었습니다. 웹 상의 데이터가 부족해짐에 따라 SOTA 모델들도 합성 데이터를 활용하고 있는 추세죠. 하지만 대부분의 훈련 데이터와 방법은 공개되지 않고 있습니다. 이러한 과정을 재현하려는 프로젝트가 있어 소개해드리려고 합니다. 

Hugginface Blog: https://huggingface.co/blog/cosmopedia
GitHub: https://github.com/huggingface/cosmopedia

개요
Cosmopedia는 250억 토큰 규모의 데이터셋으로, 80%의 웹 크롤링 데이터와 20%의 정제된 교육 자료로 구성되어 있습니다. Mixtral-8x7B-Instruct-v0.1 모델을 사용해 3천만 개 이상의 합성 텍스트 문서를 생성했으며, 일반 상식과 관련된 데이터들은 UltraChat과 OpenHermes2.5 데이터셋을 활용했습니다.

주요 특징
- 데이터 정제: 웹 데이터를 145개 주제로 클러스터링, 교육적 가치가 낮은 콘텐츠 제외
- 데이터 다양성 확보 및 중복 최소화를 위해 프롬프트 엔지니어링에 가장 많은 시간 투입
- 각 문서별 12개 버전 생성 (4가지 대상 독자 x 3가지 스타일)
- 벤치마크 오염 방지: 10-gram 중복 검사 및 추가 필터링으로 테스트 데이터 유출 방지
- 'cosmo-1b' 모델: Llama2 아키텍처 기반, 10억 매개변수, Cosmopedia 데이터로 학습
- ⭐️오픈소스: 프로젝트의 모든 코드, 데이터셋, 상세 정보를 GitHub에 공개

