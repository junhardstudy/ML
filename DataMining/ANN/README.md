---

title: "Artificial Neural Network"
excerpt: "Artificial Neural Network에 대한 이론과 rapid miner를 이용한 실습"
last_modified_at: 2020-08-15T10:27:01-05:00
categories:
  - data_mining_course
use_math: true
---

인간 두뇌의 신경계에 착안한, 수학.계산적 모델

<hr>

### Perceptron

![퍼셉트론 모델 사진](/image/perceptron.jpg)

* Simple neural network architectrue for learning a binary claasifier.

* input으로부터 weighted sum을 한 다음, threshold 값인 t를 빼고 그 값의 sign을 조사.

#### Definition

$Y = I(\sum{i}W_{i}X_{i} \; - \; t) = sign(\sum{i}W_{i}X_{i} \; - \; t)$
<br>
따라서 일반적인 case에 대한 perceptron model의 output은, $\hat{Y} = sign(w_{i}x_{i} \; + \; w_{i-1}x_{i-1} \; + \; ... \; + \; w_{0}x_{0}) = sign(\overrightarrow{w}\cdot\overrightarrow{x})$
<br>
<br>
여기서 아래 수식을 일정 기준 동안 반복하며 학습하는 동안 weight(w)값이 갱심된.
<br>
<br>
<div style="border:2px solid; max-width: 500px;">
<font size="5px"><strong>$w_{j}^{(k+1)} = w_{j}^{k} + \lambda(y_{i} - \hat{y_{i}}^{(k)})x_{ij}$</strong></font>
<br>
<br>
<font size="2px">
$w_{j}^{(k+1)}$:new weight parameter
<br> 
$w_{j}^{(k)}$:old weight parameter 
<br>
$\lambda$ : Learning rate 
<br>
$y_{i} - \hat{y_{i}}^{(k)}$ : prediction error 
<br>
$x_{ij}$ : value of the jth attribute
</font>
</div>
<br>
<br>

가질 수 있는 값의 변화로는...
1. prediction correct, weight is unchanged.

2. prediction error, $y \; = \; +1, \hat{y} \; = \; 0$ : 예측이 +1로 가기위해 weight가 증가하는 방향으로 변화.

3. prediction error, $y \; = \; 0, \hat{y} \; = \; +1$ : 예측이 0으로 가기위해 weight가 감소하는 방향으로 변화.

<br>
<br>

#### Learning rate $\lambda$

* $\lambda$값이 0에 가까울수록 새로운 weight값은 과거의 weight값에 영향을 많이 받음.

* $\lambda$값이 1에 가까울수록 새로운 weight값은 현재 weight값을 조정하는 iteration에 좀 더 민감.
<br>
<br>
<hr>

### Characteristic of Perceptron model

* Linearly separable classification problem인 경우, 최적의 해로 수렴하는 것을 보장해줌.

* 그러나 not linearly separable 한 경우, algorithm은 수렴하지 않음.
<br>
<br>
<hr>
### Building Perceptron model

1. Topology와 activation function 정하기.

2. Initialize w, $\lambda$, and t. -> 보통 randomly하게 정함.

3. Error 값 계산.(Error = $Y \; - \; \hat{Y}$)

4. weight 값 조정하는 것을 특정 기준을 만족하거나 수렴할 때까지 반복.
<br>
<br>
<hr>
Example)두 개의 Input에 대한 AND 문제

<br>
<br>
<hr>
### Multilayer perceptron

XOR 문제와 같이, 기존의 perceptron으로는 선형적으로 분리할 수 없는 문제를 해결할 수 없음
<br>
![MLP 예시 사진](/image/MLP.jpg)
<br>
* 한 개 이상의 hidden layer를 거쳐서 output을 생성.

* 보통 ANN는 이와같이 복잡하고, 비선형적인 입력과 출력의 관계를 modeling하는데 사용.
<br>
<br>
<hr>
### Building MLP

1. Initialize the weights.

2. Training example의 class와 일치하도록 weight값을 조정(학습).

   * objective function : $E(\overrightarrow{w}) = \frac{1}{2}\sum_{i=1}^{N}(y_{i} - \hat{y_{i}})^{2}$.
  
   * weight를 갱신할 때는 objective function을 최소화 하는 방향으로 진행. 
  
   * 이 때, backpropagation algorithm이나 경사 하강법(gradient descent method)등을 이용.
   
   * Gradient descent : $w_{j} \leftarrow w_{j} - \lambda \frac{\partial E(\overrightarrow{w})}{\partial w_{j}}$
   
   * ![경사하강법 예시그림](/image/gradient_descent.jpg)
  
<br>
<br>
<hr>
### Characteristic of ANN

* 경사 하강법의 경우, 종종 local minimum으로 수렴하는 경우가 있음. 이럴 경우, weight를 갱신할 때 momentum term을 주어 해결 가능.

* ANN를 학습하는데는 시간이 매우 오래 걸리지만, 한 번 model이 형성되면 test examples에 대해서는 빠르게 분류할 수 있음.

* Numeric data가 input data에 적합. numeric data가 아닐 경우 사전 작업이 필요. 
  





