2024.04.04

[Facebook](https://www.facebook.com/byeongheon.lee.98/posts/pfbid02VyPApHYJMyAJgoXjrLzxRbFpp9pCvAjWX8A1SbBaheJkP3LhBGGHv5UQthb1rZrgl)

[https://huggingface.co/blog/embedding-quantization...](https://huggingface.co/blog/embedding-quantization?fbclid=IwZXh0bgNhZW0CMTAAAR2n601oc6RoiNMCkroqfXizEGzc2IrO06Za9cyP1Xo6Onxvl7X4aTw-HL4_aem_8kfFjatblILTRcb9mX6wHA#improving-scalability)

보통 vector search에 사용되는 text embedding들은 float32 형태로 저장되는데요, 이 경우 상당히 많은 메모리를 차지한다는 단점이 있습니다.

이를 해결하고자 아래와 같은 방법이 제기되고 있습니다.

1) bit로 구성된 embedding을 따로 저장해 initial search

2) 상위 결과에 대해서 reranking/rescoring 할 때 flaot32 embdding을 사용

그 결과 ~40배의 검색 속도와 메모리 사용량 감소를 이루어냈고, 반면 성능은 ~96%를 유지했다고 합니다.

더 큰 dimension을 사용하는 embedding들에 더 효과적이었고, bit가 아닌 int8까지만 줄여도 유의미한 효과를 볼 수 있었다고 하네요.

직접 시험해보지 않으면 모르고, 한국어 관련 task에서 결과가 나온 것은 아니지만, 충분히 실험해 볼만한 주제라고 생각됩니다!