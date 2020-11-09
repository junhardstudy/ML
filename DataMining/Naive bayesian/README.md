---

title: "Naive Bayesian"
excerpt: "Naive Bayesian에 대한 이론과 rapid miner를 이용한 실습"
last_modified_at: 2020-08-15T10:27:01-05:00
categories:
  - data_mining_course
use_math : true

---

* 통계와 확률에 기반

* 많은 적용 사례에서, attribute set과 class variable사이의 상관 관계는 <strong>non-deterministic</strong>함.
<br>
&nbsp;&nbsp;->why?) noise data나 data set에는 포함되지 않지만 분류하는데 영향을 주는 다른 요소에 의해, 어떤 test example의 attribute set이
	training example의 attribute set과 동일하다고 해서 확정저긍로 똑같은 class labelss로 분류가 안될 수 있음.
	
<br>
결론적으로, <strong>attribute set과 class variable사이의 modeling probablistic relationship를 ananlysis하기 위한 적근 방법</strong>

> 예를 들어, 경대 캠퍼스를 지나가다 아무나 조사했을 때, it대 소속 학부생일 확률이 20%라고 가정. 여기서 '손에 선형 대수책을 들고 있다' 거나, '돌아다니는 경로가 정문쪽이다' 와 
> 같은 추가적인 정보는 이 학생이 it대 소속일 확률을 점점 높여줌. 즉, 결과에 영향을 주는 통계 확률이론에 기반해 결과를 추측하는 것.

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
<div>
$P(Y|X) = \frac{P(Y) \; \times \; P(X|Y)}{P(X)} \;\; \leftarrow \; P(X, Y) = P(X) \; \times \; P(Y|X) = P(Y) \; \times \; P(X|Y)$
<br>
</div>
<br>
P(X|Y) : Class conditional probability
* 해당 class label Y일 때, attribute set이 특정값 evidence X일 확률
* data set으로부터 계산할 수 있음
* posterior probability 계산하는데 중요
<br>
<br>
<hr>
### Definition of general case

$A $= { $ A_{1}, \,A_{2}, \,A_{3}, ..., \,A_{n}$ }
<br>
$P(C_{j}|A) = P(C_{j}|A_{1}A_{2}A_{3}...A{n}) = \frac{P(C_{j})P(A_{1}A_{2}A_{3}...A_{n}|C_{j})}{P(A_{1}A_{2}A_{3}...A_{n})}$
<br>
<br>
(단, attribute set A에서 <font color="red"><strong>각 attribute는 independence</strong></font> 하다고 가정!)
<br>
<br>
따라서, $\,P(A_{1}A_{2}A_{3}...A_{n}|C_{j}) = P(A_{1}|C_{j})P(A_{2}|C_{j})P(A_{3}|C_{j})...P(A_{n}|C_{j}) =  \prod_{i=1}^{n}P(A_{i}|C_{j})$
<br>

$\therefore) P(C_{j}\|A) = \frac{P(C_{j}) \, \times \, \prod_{i=1}^{n}P(A_{i}\|C_{j})}{P(A)}$
<br>
여기서 $P(C_{j}) \, \times \, \prod_{i=1}^{n}P(A_{i}\|C_{j})$가 최대값일 때, 해당 data는 $C_{j}$로 class label을 분류할 수 있음.
<br>
<br>
<br>
만약, attribute value가 continuous하다면 probability density estimation을 이용
<br>
$P(A_{i}|C_{j}) = \frac{1}{\sqrt{2\pi\sigma_{ij}^{2}}} \, e^{-\frac{(A_{i}-\mu_{i}{j})^{2}}{2\sigma_{ij}^{2}}}$
<br>
$\mu$ : sample mean
<br>
$\sigma$ : sample standard deviation
<br>
<br>
-> 연속적인 값을 가지는 특정 attribute $A_{i}$가 class $C_{j}$일 때 정규분포(normal distribution)를 따른다고 가정
<br>
-> 주의) class $C_{j}$일 때, $\; \mu,\; \sigma$ 값
<br>
<br>
<hr>

### Issues of Naive Bayes Classifier

조건부 확률 값 중, 하나 이상이 0이라면 전체 posterior probability는 0이 됨. 이와 같은 문제는 아래와 같이 term을 준 확률 계산으로 해결.
<br>
<br>
* Original : $P(A_{i}\|C) = \frac{N_{ic}}{N_{c}}$

* Laplace : $P(A_{i}\|C) = \frac{N_{ic} + 1}{N_{c} + C}\;\;$ (C는 class의 개수)

* m-estimate : $P(A_{i}\|C) = \frac{N_{ic} + mp}{N_{c} + m}\;\;$  (p : prior probability, m : parameter)

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

$P(Yes|X) = \frac{P(Yes) \; \times \; P(X|Yes)}{P(X)}$
<br>
여기서 P(X)는 P(No|X)일 때도 동일한 값이므로, 단순히 결과 값 비교에도 필요가 없으므로 계산 X.
<br>
<br>
P(X|Yes) = P(Refund = No|Yes)P(Divorced|Yes)P(Income=120K|Yes)
<br>
<br>
$P(Yes) = \frac{3}{10}$
<br>
$P(Refund = No|Yes) = \frac{3}{3} = 1$
<br>
$P(Divorced|Yes) = \frac{1}{3}$
<br>
$P(Income = 120K|Yes) = \frac{1}{\sqrt{2\pi}\times 5} e^{-\frac{(120 - 90)^{2}}{2\times 25}} = $
<div style="border:2px solid; max-width: 550px;">
continuous한 값인 income의 확률을 구하기 위한 표본 평균과 표준 편차
<br>
$\mu = \frac{95+85+90}{3} = 90$
<br>
$\sigma^{2} = 25$ 
<br>
</div>
<br>
$P(Yes|X) = P(Yes) \times P(X|Yes) = \frac{3}{10} \times 1 \times \frac{1}{3} \times \frac{}{} = $
<br>
<hr style="border:dashed">
<br>
$P(No|X) = {P(No) \; \times \; P(X|No)}$
<br>
P(X|No) = P(Refund = No|No)P(Divorced|No)P(Income=120K|No)
<br>
<br>
<br>
$P(No) = \frac{7}{10}$
<br>
$P(Refund = No|No) = \frac{4}{7}$
<br>
$P(Divorced|No) = \frac{1}{7}$
<br>
$P(Income = 120K|No) = \frac{1}{\sqrt{2\pi}\times 54.54} e^{-\frac{(120 - 110)^{2}}{2\times 2975}} = $
<div style="border:2px solid; max-width: 550px;">
continuous한 값인 income의 확률을 구하기 위한 표본 평균과 표준 편차
<br>
$\mu = 110$
<br>
$\sigma^{2} = 2975$ 
<br>
</div>
<br>
$P(No|X) = P(No) \times P(X|No) = \frac{7}{10} \times \frac{4}{7} \times \frac{1}{7} \times \frac{}{} = $





