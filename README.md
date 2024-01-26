# Stable Diffusion
## VAE
### Maximum Likelihood
VAE(Variational Autoencoders)는 Generative model로 Autoencoders와는 반대로 Decoder부분을 학습시키기 위해 만들어졌다. 
MLE(Maximum Likelihood Estimation)관점에서의 모델의 학습에 대해 먼저 설명하면 input $z$와 target $x$가 있을 때, $f_{\theta}(\cdot)$ 은 모델의 종류가 되고, 최종 목표는 정해진 확률분포에서 target이 나올 확률인 $p(x | f_{\theta}(z))$가 최대가 되도록 하는 것이다. 따라서 MLE에서는 학습전에 확률분포(가우시안, 베르누이.. )를 먼저 정하게 되고, 모델의 출력은 이 확률 분포를 정하기 위한 파라미터(가우시안의 경우 $\mu, \sigma^2$)라고 해석할 수 있다. 결과적으로 target을 잘 생성하는 모델 파라미터 $\theta$는 $\theta^* = \underset{\theta}{\arg\min} [-\log(p(x | f_{\theta}(z)))]$가 된다. 이렇게 찾은 $\theta^*$는 확률분포를 찾은 것이므로 결과에 대한 sampling이 가능하고, 이 sampling에 따라 다양한 이미지가 생성될 수 있는 것이다.

VAE의 Decoder도 위와 비슷하다. Encoder를 통해 sampling된 데이터 $z$ (Latent Variable)가 있고 Generator $g_{\theta}(\cdot)$와 Target $x$가 있을 때, training data에 있는 $x$가 나올 확률을 구하는 것을 목적으로 한다. 이때 $z$는 controller로서 생성될 이미지를 조정하는 역할을 할 수 있다. 예를 들면 고양이의 귀여움을 조정하여 더 귀여운 고양이 이미지를 생성하는 것이다.

다시 돌아와서 결과적으로 VAE의 목적은 모든 training data $x$에 대해 $x$가 나올 확률 $p(x)$를 구하는 것이 목적이다. 이때 training data에 있는 sample과 유사한 sample을 생성하기 위해서 prior 값을 이용하는데, 이 값이 Latent Variable인 $z$가 나올 확률 $p(z)$이고, $p(x)$는 $\int p(x | g_{\theta}(z))p(z) dz = p(x)$로 구해진다. 


### Prior Distribution 

<p align="center"><img src="https://github.com/em-1001/Stable-Diffusion/assets/80628552/2a1ac824-6398-43df-b68a-504785def59d"></p>

앞서 말했듯이 $z$는 controller 역할을 하기 때문에 $z$를 잘 조정할 수 있어야 한다. 이때 $z$는 고차원 input에 대한 manifold 상에서의 값들인데, generator의 input으로 들어가기 위해 sampling된 값이 이 manifold 공간을 잘 대표하는가? 라는 질문이 나온다. 이에 대한 답은 잘 대표한다는 것이다. 위 이미지의 예시처럼 normally-distributed 된 왼쪽에 $g(z) = \frac{z}{10} + \frac{z}{||z||}$를 적용하면 오른쪽의 ring 형태가 나오는걸 확인할 수 있다. 이처럼 간단한 변형으로 manifold를 대표할 수 있기 때문에 모델이 DNN 이라면, 학습해야 하는 manifold가 복잡하다 하더라도, DNN의 한 두개의 layer가 manifold를 찾기위한 역할로 사용될 수 있다. 따라서 Prior Distribution을 normal distribution과 같은 간단한 distribution으로 해도 상관없다.  


### Variational Inference
$p(x | g_{\theta}(z))$의 likelihood가 최대가 되는 것이 목표라면 Maximum Likelihood Estimation를 직접적으로 사용해서 구하면 될거 같은데 실제론 그렇지 않다. 그 이유는 가우시안 분포라 가정했을 때, $p(x | g_{\theta}(z))$의 log loss인 $-\log(p(x | g_{\theta}(z)))$는 Mean Squared Error와 같아진다. 즉, MSE의 관점에서 loss가 작은 것이 $p(x)$에 더 크게 관여하는데, MSE loss가 작은 이미지가 실제 의미적으로 더 가까운 이미지가 아닌 경우가 많기 때문에 올바른 방향으로 학습할 수가 없다. 

$$||x - z_{bad}||^2 < ||x - z_{good}||^2 \to p(x | g_{\theta}(z_{bad})) > p(x | g_{\theta}(z_{good}))$$

예를 들면 원래 고양이 이미지에서 일부분이 조금 잘린 이미지 a와 한 pixel씩 옆으로 이동한 이미지 b가 있다고 하면 b는 pixel만 옆으로 밀렸을 뿐 고양이 그대로 이지만 a는 이미지가 잘렸기 때문에 의미적으론 b가 a보다 고양이에 가까운데, MSE 관점에서는 b의 loss가 더 크게 나오게 된다. 



### ELBO





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

언데드 언럭(ㅋㄹ ㅈㅌㅍ

# Reference
## Paper
Tutorial on Variational Autoencoders : https://arxiv.org/pdf/1606.05908.pdf  
