# 개요

* 통계와 확률에 기반

* 많은 적용 사례에서, attribute set과 class variable사이의 상관 관계는 <strong>non-deterministic</strong>함.

<br>
->why?) noise data나 data set에는 포함되지 않지만 분류하는데 영향을 주는 다른 요소에 의해, 어떤 test example의 attribute set이 training example의 attribute set과 동일하다고 해서 확정저긍로 똑같은 class labelss로 분류가 안될 수 있음.
	
<br>
결론적으로, <strong>attribute set과 class variable사이의 modeling probablistic relationship를 ananlysis하기 위한 적근 방법</strong>

<br>
<hr>

### Notations and terminologies

X : the evidence, factors, or attribute set.
<br> 
주의)Not individual attribute.
<br>
<br>
Y : the outcome, taget, or class label.
<br>
<br>
P(Y) : prior probability.
<br>
 * 주어진 data set에서 해당 outcome의  발생빈도.
 
 * data set으로부터 구할 수 있음.
<br>
<br>

P(Y\|X) : the conditional probability.
 * 조건부 확률로서, evidence X가 일어났을 때 outcome Y일 확률.
 * posterior probability라고도 불림.
<br>
<br>
<hr>

### Conditional probability 

->Bayes Theorem을 이용하여 계산
<br>
<br>

![image](./image/conditionalprob.png)

<br>

<br>
P(X|Y) : Class conditional probability
* 해당 class label Y일 때, attribute set이 특정값 evidence X일 확률
* data set으로부터 계산할 수 있음
* posterior probability 계산하는데 중요
<br>
<br>
<hr>

### Definition of general case

![image](./image/defgen.png)

<br>
<br>
(단, attribute set A에서 <strong>각 attribute는 independence</strong> 하다고 가정!)
<br>
<br>
![image](./image/defconclusion.png)
<br>
따라서 확률값이 큰 target condition일 때, 해당 target label로 분류.
<br>
<br>
<br>
만약, attribute value가 continuous하다면 probability density estimation을 이용
<br>
![image](./image/pdf.png)
<br>
<br>
-> 연속적인 값을 가지는 특정 attribute A가 class C일 때, 정규분포(normal distribution)를 따른다고 가정
<br>
<br>
<hr>

### Issues of Naive Bayes Classifier

조건부 확률 값 중, 하나 이상이 0이라면 전체 posterior probability는 0이 됨. 이와 같은 문제는 아래와 같이 term을 준 확률 계산으로 해결.
<br>
<br>
![image](./image/variousprob.png)
<br>
<hr>

### Naive Bayes classifie 특징

1. noise data에 robust.

2. missing value(s)를 가진 instance는 확률 계산과정에서 무시하고 계산 할 수 있음.

3. 전체적으로 attribute를 고려할 때, 계산 결과가 거의 균등하게 분포하므로 관계 없는 attribute에 robust함.

4. 만약 attribute 사이에 서로 <strong>not independent하다면</strong>, model의 performance가 낮아짐. 따라서 이럴 때는 다른 technique가 필요함.

<br>
<hr>
### Example

주어진 test record의 attribute가 X = (Refund = No, Divorced, Income = 10k)일때, Naive Bayes Classifier를 이용한다면 어느 class label로 분류?
<br>
<br>
-> P(Evade = Yes| X)와 P(Evade = No| X)중에서 값이 높은것
<br>
<table>
<th>Id</th>
<th>Refund</th>
<th>Marital Status</th>
<th>Taxable Income</th>
<th style="color:yellow">Evade</th>
<tr>
	<td>1</td>
	<td>Yes</td>
	<td>Single</td>
	<td>125K</td>
	<td style="color:red;">No</td>
</tr>
<tr>
	<td>2</td>
	<td>No</td>
	<td>Married</td>
	<td>100k</td>
	<td style="color:red;">No</td>
</tr>
<tr>
	<td>3</td>
	<td>No</td>
	<td>Single</td>
	<td>70K</td>
	<td style="color:red;">No</td>
</tr>
<tr>
	<td>4</td>
	<td>Yes</td>
	<td>Married</td>
	<td>120K</td>
	<td style="color:red;">No</td>
</tr>
<tr>
	<td>5</td>
	<td>No</td>
	<td>Divorced</td>
	<td>95K</td>
	<td style="color:red;">Yes</td>
</tr>
<tr>
	<td>6</td>
	<td>No</td>
	<td>Married</td>
	<td>60K</td>
	<td style="color:red;">No</td>
</tr>
<tr>
	<td>7</td>
	<td>Yes</td>
	<td>Divorced</td>
	<td>220K</td>
	<td style="color:red;">No</td>
</tr>
<tr>
	<td>8</td>
	<td>No</td>
	<td>Single</td>
	<td>85K</td>
	<td style="color:red;">Yes</td>
</tr>
<tr>
	<td>9</td>
	<td>No</td>
	<td>Married</td>
	<td>75K</td>
	<td style="color:red;">No</td>
</tr>
<tr>
	<td>10</td>
	<td>No</td>
	<td>Single</td>
	<td>90K</td>
	<td style="color:red;">Yes</td>
</tr>
</table>
<br>

![image](./image/sol1.png)
<br>
여기서 P(X)는 P(No|X)일 때도 동일한 값이므로, 단순히 확률값의 상대적인 크기 비교에는 필요가 없으므로 계산 X.
<br>
<br>
P(X|Yes) = P(Refund = No|Yes)P(Divorced|Yes)P(Income=120K|Yes)
<br>
<br>
![image](./image/sol2.png)
<br>
![image](./image/sol3.png)
<br>
<hr>
<br>
P(X|No) = P(Refund = No|No)P(Divorced|No)P(Income=120K|No)
<br>
<br>
![image](./image/sol4.png)

***

## Rapid Miner를 이용한 실습

## 출처 및 참고문헌

Naive Bayes model에 대한 이론 및 rapid miner 사용법 : 강의 PPT 자료



