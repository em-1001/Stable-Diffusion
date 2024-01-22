# Stable Diffusion
## VAE

## DDPM




### 통계학
https://angeloyeo.github.io/2020/01/09/Bayes_rule.html

https://angeloyeo.github.io/2020/09/14/normal_distribution_derivation.html -> exponential 내부의 식 유도전까지 공부함

몬테카를로 시뮬레이션으로 배우는 확률통계 with 파이썬 : https://www.yes24.com/Product/Goods/117709828

#### MLE
https://modernflow.tistory.com/67  
https://everyday-tech.tistory.com/entry/%EC%B5%9C%EB%8C%80-%EC%9A%B0%EB%8F%84-%EC%B6%94%EC%A0%95%EB%B2%95Maximum-Likelihood-Estimation

#### 나이브 베이즈 분류기 
https://ko.wikipedia.org/wiki/%EB%82%98%EC%9D%B4%EB%B8%8C_%EB%B2%A0%EC%9D%B4%EC%A6%88_%EB%B6%84%EB%A5%98

## Stable-Diffusion 영상  
ELBO 식 : https://yonghyuc.wordpress.com/2019/09/26/elbo-evidence-of-lower-bound/  
ELBO 식에서 $\log p(x)$가 빠져나오는 이유는 $\int q(z) = 1$이기 때문인듯. 

VAE : https://huidea.tistory.com/296  , https://deepinsight.tistory.com/127 이걸로 낼 공부 ㄱㄱ

레전드 강의2 -> 33:10


## 목표 
stable-diffusion from scratch 영상을 따라 코딩해보고(직접 구현할 수 있는건 직접 구현), 원하는 이미지를 구해서 커스텀 학습시켜보기 
VAE -> CLIP -> U-Net -> DDPM 순으로 직접 구현해보기 

### VAE 구현
https://avandekleut.github.io/vae/
