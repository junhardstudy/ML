---

title: "What is Data Mining?"
excerpt: "Data Mining에 대한 정의와 관련된 개념, 실제 데이터를 이용한 예시"
last_modified_at: 2020-08-08T10:27:01-05:00
order: 1
categories:
  - data_mining_course

---

데이터 마이닝(Data Mining)은 이미 알고 있는 데이터의 특성(<strong>attributes</strong>)들을 이용하여 <strong>model</strong>을 학습하거나, 이용하여 추후, 또는 모르지만(unpredicted) 관심있는 특별한 데이터의 특징(<strong>target</strong>)을 예측(predict)하는 것입니다.

---
#### 목차
1. 특성(Attribute)
2. Data
3. Model

---
### 특성(Attribute)
특성을 이야기하기 전에, 아래와 같은 버섯에 관한 데이터가 존재할 때 각각의 행(row)을 record 또는 instance 라고 하는데

![이미지를 표시할수 없습니다](../../../../image/data_preview.png)


각 record의 column(열)들이 data의 attribute이며 버섯의 갓 모양이나 색깔등등, 명칭에서 알수 있듯이 버섯 데이터에 대한 여러 특성(attributes)들을 알려 줍니다.

특히, 이 특성들 중에서 예측하고자 하는 특별한 특성을 target이라 하며, 위 버섯데이터의 경우 독버섯과 식용버섯의 분류에 목적을 둔다면 class가 <strong>target</strong>이 됩니다.

### Data
데이터들은 이러한 특성들을 가진 1개 이상의 record의 집합(set)으로 이루어져 있습니다. 이 데이터 집합들을 이용하여 model을 학습하거나 이용하여 예측을 하게 되는데, 학습된 model에 대한
검증 및 테스트가 필요합니다. 따라서 모든 데이터를 학습에 사용하지는 않고 일정 비율로 나누어, 데이터를 model 학습에 필요한 <strong>training data</strong>와 model 평가에 필요한 <strong>test data</strong>로 각각 나누게 됩니다.


### Model
이렇게 준비 된 training data를 이용하여 예측을 위한 모델을 학습하게 됩니다.
예측하고자 하는 target에 대한 값인 <strong>label의 유무</strong>에 따라 model은 크게 <strong>supervised learning</strong>과 <strong>unsupervised learning</strong>으로 나뉩니다.

supervised learning은 예측하고자 하는 데이터의 target class의 label이 있는 경우로, 즉 어떤것을 예측해야 하는지 알려주면서 model을 학습하는 경우 입니다. 알아볼 model은 classification 입니다.

unsupervised learning은 예측하고자 하는 데이터의 target class의 label이 없는 경우로, 특정한 분류, label의 예측이 아닌 데이터 간의 관계나 데이터의 특성, 패턴등에 관심이 있는 경우 입니다. 알아볼 model은 clustering 입니다.



버섯 데이터 출처 : <https://www.kaggle.com/uciml/mushroom-classification>
