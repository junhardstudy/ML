---

title: "Decision Tree(Rapid Miner)"
excerpt: "rapid miner를 이용하여 버섯 데이터를 식용 버섯과 독버섯으로 분류하는 decision tree모델을 만들고 테스트 데이터를 이용하여 모델을 검증 해 봅니다."
last_modified_at: 2020-08-08T11:27:01-05:00
order: 3
categories:
  - data_mining_course
  
use_math: true

---

target class를 포함하여 23개의 attribute를 가지고 있으며 record의 개수는 8,124개 입니다. 이 중 70퍼센트를 learning data, 나머지 30퍼센트를 test data로 활용합니다.
* [Data preparation](#data preparation)

* [Learning model](#learning model)

* [Apply test data](#test data)


***


<div id="data preparation"></div>
### Data preparation
예시 데이터셋의 경우, missing attribute, label이 없습니다.
<br>
![결정트리결과](/image/decision_tree_images/1.JPG)
<br>
우선, 모델에서 예측해야 할 target class를 정해주어야 합니다. 우리가 알고자하는 것은 식용 유무이므로 class가 target이 됩니다.
<br>
![rapid miner decision tree 1](/image/decision_tree_images/2.JPG)
<br>
따라서 rapid miner의 process 중에서 set role을 통해 class를 target class로 정해줍니다.
<br>
그리고 모델을 학습할 데이터와 테스트 할 데이터가 필요하므로, 전체 데이터의 70퍼센트를 학습용 데이터와 나머지 30퍼센트를 테스트 데이터로 분리해주는 
split data process를 적용 해 줍니다.
<br>
![결정트리결과](/image/decision_tree_images/4.JPG)
<br>
<br>
![결정트리결과](/image/decision_tree_images/3.JPG)
<br>

<div id="learning model"></div>
### Learning model
split process 블록에서 첫번째 out port가 70퍼센트의 데이터를 가지고 있으므로 decision tree의 tra port로 이어줍니다.
<br>
![결정트리결과](/image/decision_tree_images/5.JPG)
<br>
decision tree의 parameter는 다음과 같이 적용하였습니다.
<br>
![결정트리결과](/image/decision_tree_images/6.JPG)
<br>
앞 포스트에서 언급한 여러 parameter가 있으며, gain ratio를 적용 하였습니다.

<div id="test data"></div>
### Apply test data to model
decision tree 블록의 mod output port를 apply model 블록의 mod input port로, split 블록의 두번째 output port를 apply model의 unl input port로 이어줍니다.
<br>
![결정트리결과](/image/decision_tree_images/7.JPG)
<br>
버섯 데이터를 decision tree의 모델로 구현하면 아래와 같이 나오며
<br>
![결정트리결과](/image/decision_tree_images/result.JPG)
전체 2,437개의 example중에서 2,434개를 올바르게 예측 하였고, 나머지 3개의 example이 잘못된 예측을 하였습니다.
<br>
![결정트리 wrong 결과](/image/decision_tree_images/wrong.JPG)
<br>
버섯 데이터 출처 : https://www.kaggle.com/uciml/mushroom-classification/