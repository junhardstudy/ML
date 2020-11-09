# 개요

데이터 마이닝에서 supervised learning중 classification인 decision tree model을 이용하여 버섯 데이터를 학습한 뒤 식용 버섯인지 독 버섯인지 예측하여 봅니다.

## Decision Tree

가령 독버섯에 대한 decision tree 모델에 대해 예시를 들면 아래와 같습니다.
![decision_tree_example_image1](./image/decision_tree_ex1.jpg)


* root node: decision tree의 가장 상단에 있는 노드로서, 들어오는 edge는 없고 0개 이상의 edge(s)를 포함하는 노드 입니다.

* internal node : 정확히 하나의 들어오는 edge와 2개 또는 그 이상의 나가는 edges를 가지고 있습니다. 또한 데이터들이 가지는 특정 attribute에 대해 조건(<strong>condition</strong>)에 따라 나누는(split) 역할을 합니다. 

* leaf or terminal node : 단 하나의 들어오는 edge만 있고 나가는 edge는 없는 노드로서 <strong>class label</strong>을 대표합니다.

최적의 split attribute condition을 찾기 위한 attribute test condition방법에서 impurity가 사용됩니다.
특정 노드 t의 impurity값은

![impurity equations](./image/equations.png)

위 impurity 값이 최소가 되는 방향으로, 
또는 GAIN값이 최대가 되는 방향으로 split condition을 정하게 됩니다.

---



<div id="gini"></div>
## GINI index
CART, SLIQ, SPRINT 알고리즘에서 사용되며, impurity와 k개의 분할에 대한 Gini index에 가중치가 적용된 Gini split은 각각 아래와 같습니다.

![gini](./image/gini.png)

Example)

![결정트리예시사진2](/image/decision_tree_ex2.jpg)


왼쪽 leaf node를 t1, 오른쪽 leaf node를 t2라 하면,

<div style="border:2px solid; max-width: 500px;">
$GINI(t1) = 1 - ( (\frac{9}{11})^2 + (\frac{2}{11})^2) = 0.297$
<br>
$GINI(t2) = 1 - ( (\frac{3}{9})^2 + (\frac{6}{9})^2) = 0.444$
<br>
<br>
$GINI_{split} = \frac{11}{20} \times 0.297 + \frac{9}{20} \times 0.444 = 0.363$
<br>
</div>
<br>

<div id="entropy"></div>

***

## Entropy
ID3, C4.5 알고리즘에서 사용되며, impurity와 Gain split 값은 아래와 같습니다. 

![gini](./image/entropy.png)


###### ( Entropy(p) : before split )

<br>
<br>

#### Entropy가 가지는 의미
정보 이론에서 information gain(정보 이득량)은 해당 사건이 자주 발생하거나, 많이 분포되어 있다면 정보량(informational value)이 적다고 하고, 해당 사건의 분포가 적거나, 드물게, 또는 잘 일어
나지 않는다면 정보량이 많다고 합니다.
<br>
<br>
예를 들어, 주머니안에 [사탕, 사탕, 사탕, 초콜렛, 사탕]이 들어 있다면, 사탕이 나올 확률은 $\frac{4}{5}$, 초콜렛이 나올 확률은 $\frac{1}{5}$입니다.
<br>
이 때 사탕의 정보량은 : 0.097
<br>
초콜렛의 정보량은 : 0.699 로 초콜렛을 뽑을 사건이 더 정보량이 높게 나옵니다.
<br>
<br>
![information](./image/information.png)
<br>
<br>
정의에서 알수 있듯이, 좀 더 잘 일어날수 있는 사건(P(x))의 경우 높은 값을 가지고, 반대로 드물게 일어나는 사건일수록 낮은 값을 가짐을 알 수 있습니다.
<br>
여기서 entropy는 무작위 시행의 결과로 인한 기대되는 평균 정보량을 의미합니다. 즉 정보의 혼잡도를 뜻합니다.
>Entropy measures the expected (i.e., average) amount of information conveyed by identifying the outcome of a random trial.(from wikipedia)

<br>


Example)

![결정트리예시사진2](/image/decision_tree_ex2.jpg)


왼쪽 leaf node를 t1, 오른쪽 leaf node를 t2라 하면,

<div style="border:2px solid; max-width: 600px;">
$Entropy(t1) = - (\frac{9}{11}log_{2}\frac{9}{11} + \frac{2}{11}log_{2}\frac{2}{11} ) = 0.684$
<br>
$Entropy(t2) = - (\frac{3}{9}log_{2}\frac{3}{9} + \frac{6}{9}log_{2}\frac{6}{9}) = 0.918$
<br>
<br>
$Entropy(p) = - (\frac{12}{20}log_{2}\frac{12}{20} + \frac{8}{20}log_{2}\frac{8}{20} ) = 0.971$
$Gain_{split} = 0.971 - (\frac{11}{20} \times 0.684 + \frac{9}{20} \times 0.918) = 0.1817$
<br>
</div>
<br>

![solve1](./image/solve1.png)


<div id="error"></div>

***

## Misclassification Error

![missclassification](./image/missclassification.png)

Example)

![결정트리예시사진2](/image/decision_tree_ex2.jpg)


왼쪽 leaf node를 t1, 오른쪽 leaf node를 t2라 하면,

<div style="border:2px solid; max-width: 800px;">
$Error(t1) = classification Error(t1) = 1 - max( \frac{9}{11}, \frac{2}{11}) = 1 - \frac{9}{11} = 0.1818$
<br>
$Error(t2) = classification Error(t1) = 1 - max( \frac{3}{9}, \frac{6}{9}) = 1 - \frac{6}{9} = 0.333$
<br>
<br>
$Error(Split) = \frac{11}{20} \times 0.182 + \frac{9}{20} \times 0.333 = 0.250$

</div>
<br>

***

문제는 이러한 impurity 값을 작게 하기 위해 최대한 split 가지수를 많이 늘릴수도 있습니다. 가령, 극단적인 예시의 경우 데이터에서 각각의 record가 고유한 id를 가질 때 
split condition으로 id에 대해 record개수 만큼 split을 하게 된다면 impurity값은 작아지겠지만 우리가 얻고자 하는 유의미한 모델은 아니게 됩니다. 따라서 이러한
split에 대해 제약을 주기 위해 SplitINFO가 사용 됩니다.

![splitinfo](./image/splitinfo.png)

따라서 split에 대한 제약이 적용된 gain은

![gainratio](./image/gainratio.png)

Example)GINI index 예제에 splitINFO를 적용하면,

<div style="border:2px solid; max-width: 500px;">
$ SplitINFO = - ( \frac{11}{20}log_{2}\frac{11}{20} + \frac{9}{20}log_{2}\frac{9}{20} ) = 0.993$
<br>
$ GINI(p) = 1 - ((\frac{11}{20})^2 + (\frac{9}{20})^2) = 0.495$
<br>
$ GINI_{split} = 0.363$
<br>
<br>
$ GainRATIO_{split} = \frac{0.132}{0.993} = 0.1329$
</div>
<br>

![solve2](./image/solve2.png)

***

## Overfitting, Underfitting problems

#### 용어 설명

1. Training Errors : training data에 대해 잘 못 예측(분류)하는 비율로 측정

2. Generalization errors : test 데이터에 대한 잘 못 예측(분류)하는 비율로 측정

#### Overfitting

모델의 complexity가 복잡하거나 data의 noise(missing value, outlier등등)의 존재, 또는 충분치 않은 학습 데이터의 개수에 의해서 발생하거나 
training data에만 치우치게 학습하여 test data에 대해서는
잘 예측을 하지 못하여 generalization error가 큰 경우 입니다.

#### Underfitting

overfitting과는 반대로 모델의 complexity가 낮아져 training error가 높은 경우 입니다.

***

그렇다면, decision tree model을 만들 때 언제 sub tree 생성을 멈춰야 하는가가 또 문제입니다.
두 가지 방법으로,
* pre-pruning : tree를 만들면서 stop을 확인하는 경우, 다음과 같은 기준이 적용 될수 있음

	* information gain threshold를 만족하는 attribute가 없을 때
	* tree가 최대 깊이에 도달 했을 때
	* 현재 sub tree에 있는 example의 수가 일정 개수 이상의 example 기준을 못 넘겼을 때



* post-pruning : 될수 있는 한 tree를 깊게 만든 뒤, 효율적이지 못한 sub tree에 대해 가지치기를 적용

이러한 방법들은 overfitting을 방지하기 위해 사용 될 수 있습니다.