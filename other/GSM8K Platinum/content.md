2025.03.08

[LinkedIn](https://www.linkedin.com/posts/byeongheon-lee-2b83aa222_gsm8k%EC%9D%98-%EB%9D%BC%EB%B2%A8-%EC%98%A4%EB%A5%98%EB%A5%BC-%EC%88%98%EC%A0%95%ED%95%9C-%EC%83%88%EB%A1%9C%EC%9A%B4-%EB%B2%A4%EC%B9%98%EB%A7%88%ED%81%AC-gsm8k-platinum%EC%9D%B4-activity-7304007749288239105-21sF?utm_source=social_share_send&utm_medium=member_desktop_web&rcm=ACoAADfxcywBkH2Mi2-YPZm7jSZERa3dQ2_DDEY)

GSM8K의 오류를 수정한 새로운 벤치마크, GSM8K-Platinum이 공개되었습니다! ✨

기존 GSM8K에서는 SOTA 모델들도 정확도가 약 95% 수준에서 더 이상 오르지 못하는 현상이 나타났는데요. 연구진은 이 원인이 벤치마크 자체의 애매하거나 잘못된 문제 때문이었다고 밝혔습니다. 

연구 블로그: https://gradientscience.org/gsm8k-platinum/  
연구자 X 링크: https://x.com/aleks_madry/status/1897753562824032512  
GSM8K Platinum huggingface: https://huggingface.co/datasets/madrylab/gsm8k-platinum  

연구진은 이를 보다 자세히 검증하기 위해 여러 최상위 LLM을 사용해 기존 GSM8K의 모든 문제를 다시 검증했습니다. 이때, 하나 이상의 모델이 정답과 다른 결과를 낸 문제들을 ‘의심스러운 질문’으로 선정했습니다. 이렇게 선정된 219개의 질문을 연구진이 다시 수작업으로 검토하여, 최종적으로 110개는 삭제하고 10개는 잘못된 정답을 수정했습니다. 

그 결과 잘 드러나지 않았던 모델 간 성능 차이가 명확히 드러났는데요. 예를 들어 Claude 3.7 Sonnet과 Llama 405B는 원래 GSM8K에서는 오류 개수가 45개로 동일했지만, Platinum 버전에서는 Claude 3.7 Sonnet이 2개, Llama가 17개로 차이가 확연히 드러났습니다. 

다들 벤치마크가 중요하다고는 하지만, 정작 대규모 데이터셋들은 제대로 검수가 안되고 있는 것이 현실인 것 같습니다. 누굴 탓할 수도 없는 게 워낙 양이 많고, 주로 어떤 회사에 의해 관리되는 경우보다는 연구의 결과물로 공개되고 그 이후로는 명확한 관리 주체가 없는 경우가 많더라구요. 

그래서 MMLU나 GSM8K 등이 연구에서 인용은 되고 있지만 정작 연구자들도 크게 신뢰를 하지 못하는 아이러니한 상황이 벌어지기도 하고요. 정적인  다지선다 문제라는 벤치마크 형식 자체가 LLM을 잘 평가하지 못한다는 지적과는 별개로, 벤치마크를 관리하는 사람들을 위한 동기 부여가 제공되면 합니다.   

GSM8K-Platinum은 기존 GSM8K와 동일한 포맷으로 HuggingFace에 올라와 있어 바로 사용할 수 있습니다. 더 정확한 모델 성능 평가를 원하신다면 지금 바로 업데이트해보세요! 📊🔍