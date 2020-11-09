---

title: "Rule induction"
excerpt: "Rule induction에 대한 이론과 rapid miner를 이용한 실습"
last_modified_at: 2020-08-15T10:27:01-05:00
categories:
  - data_mining_course
use_math : true

---
앞의 decision tree와는 다르게, 특정 class label에 부합하기 위한 attribute 선행 조건들을 and로 묶은 rule set을 정합니다.
그리고 예측하고자 하는 example의 class label을 rule set을 통해 label을 판별하는 모델입니다.
<br>
<br>
예를 들어, 롤 유저들의 티어를 예측하는 rule set을 만든다면,
<br>
<div style="border:2px solid; max-width: 500px;">
(맵을 보는가 = No) $\wedge$ (플레이시 비속어 횟수 > 20) = 브론즈
<br>
(와드 설치 유무 = Yes) $\wedge$ (분당 cs 횟수 > 8) = 플레티넘
</div>
<br>
처럼 표현 할 수 있습니다.

### Definition

Rule i : $(Condition)_{i} \rightarrow$ $ y$ $ _{i}$
<br>
<br>
위와 같은 if...then...else로 이루어진 rule이 집합의 형태로 이루어져 있으며 이를 rule set이라 합니다.
이 rule set에 의해 record의 label을 판별하게 되는데
여기서 해당 rule이 특정 record의 조건에 부합한다면 해당 rule은 그 record를 <strong>cover</strong>한다고 하며, 
반대로 record는 해당 rule을 <strong>trigger</strong> 한다고 말할수도 있습니다.

***

### 용어설명

#### Accuracy : $\frac{\|A\|}{\|D\|}$
<br>
전체 데이터 중에서 해당 rule을 trigger하는 record의 비율을 의미

#### Coverage : $\frac{\|A\cap y\|}{\|A\|}$
<br>
trigger하는 record 중에서 해당 rule set의 label과 같은 record의 비율을 의미

Example)
<table>
	<th>Id</th>
	<th>와드 여부</th>
	<th>맵 활용 여부</th>
	<th>비속어 사용 횟수</th>
	<th>티어</th>
	<tr>
		<td>1</td>
		<td>Yes</td>
		<td>Yes</td>
		<td>2</td>
		<td>플레티넘</td>
	</tr>
	<tr>
		<td>2</td>
		<td>Yes</td>
		<td>No</td>
		<td>5</td>
		<td>브론즈</td>
	</tr>
	<tr>
		<td>3</td>
		<td>Yes</td>
		<td>No</td>
		<td>8</td>
		<td>브론즈</td>
	</tr>
	<tr style="color:red;">
		<td>4</td>
		<td>No</td>
		<td>No</td>
		<td>55</td>
		<td>브론즈</td>
	</tr>
	<tr style="color:red;">
		<td>5</td>
		<td>No</td>
		<td>No</td>
		<td>32</td>
		<td>브론즈</td>
	</tr>
	<tr>
		<td>6</td>
		<td>Yes</td>
		<td>Yes</td>
		<td>12</td>
		<td>플레티넘</td>
	</tr>
	<tr style="color:red;">
		<td>7</td>
		<td>No</td>
		<td>No</td>
		<td>35</td>
		<td>플레티넘</td>
	</tr>
	<tr>
		<td>8</td>
		<td>Yes</td>
		<td>Yes</td>
		<td>10</td>
		<td>플레티넘</td>
	</tr>
	<tr>
		<td>9</td>
		<td>Yes</td>
		<td>Yes</td>
		<td>27</td>
		<td>플레티넘</td>
	</tr>
	<tr>
		<td>10</td>
		<td>No</td>
		<td>Yes</td>
		<td>29</td>
		<td>브론즈</td>
	</tr>
</table>
Rule set : (와드 여부 = No) $\wedge$ (맵 활용 여부 = No) $\wedge$ (비속어 사용 횟수 > 20) = 브론즈
<br>
<br>
Coverage = $\frac{3}{10} = 0.3$
<br>
이 중에서 class label이 브론즈인 record는 2개 이므로,
<br>
<br>
Accuracy = $\frac{2}{3} = 0.6667$
<br>
<hr>
<br>
#### Mutually exclusive rule set
모든 instance들이 많아야 하나의 rule에 의해서만 cover 될 때
<br>
<br>
Not mutually exclusive rule ser일 때는,
<br>
1. ordered rules : rule들만의 우선순위에 따라 순위를 정의하여 순위가 높은 rule의 class로 분류, oreder rule set은 decision list로도 알려짐

2. unordered rules : majority 또는 weighted voting을 통해 class label을 결정
<br>
<br>

#### Exhaustive rule set
모든 instance들이 최소한 하나 이상의 rule에 의해서만 cover 될 때
<br>
<br>
Not exhaustive rule set일 때, default rule과 default class를 이용
<br>
* default class : 보통은 모델내에 현재 존재하는 rule에 의해 cover가 되지 않는 training record들 중에서 다수인 class를 default class로 채택
<br>

<h4>따라서 mutually exclusive & exhaustive rule set은 <U>모든 instance들</U>이 <U>하나의 rule</U>에 의해서만 cover 된다는 것을 의미</h4>
<br>


***

### Building classification rules



* Direct method : data set으로 부터 바로 rule 생성

* Indirect method : 다른 분류 모델로 부터 rule을 생성

#### Direct method

sequential covering approach
> grow rules in a greedy fashion based on evaluation measure
> extract the rules one class at a time.

1. Start from an empty rule.
2. Grow a rule using the <strong>Learn - one - rule function</strong>.
3. Remove training instaces coverd by the rule.
4. Repeat step (2) and (3) untile stopping criterion is method

##### What is Learn-one-rule function? <br> 최대한 많은 positive examples를, 또는 최대한 적은 negative example을 가지도록 하는 rule을 찾아 내는 것.

그렇다면 어떤 rule이 best rule인지 평가할 수 있는 척도가 필요함.
<br>

***

### Metrics for rule evaluation

rule에 대한 평가를 하기 위한 척도
<div style="border:2px solid; width: 700px;">
1. Accuracy = $\frac{n_{c}}{n}$
<br>
2. Laplace = $\frac{n_{c} + 1}{n + k}$
<br>
3. M-estimate =  $\frac{n_{c} + kp}{n + k}$
<br>
4. FOIL's information gaine = $P_{1} \times (log_{2}\frac{P_{1}}{P_{1} + n_{1}} - log_{2}\frac{P_{0}}{P_{0} + n_{0}})$
<br>
</div>
<div style="border:2px solid; width: 700px;">
$ n $: rule에 의해 cover된 instance의 개수 
<br>
$ n_{c} $: rule에 의해 cover된 instance들 중에서 올바르게 분류된 instance의 개수
<br>
$ k $: class의 개수 
<br>
$ p $: prior probability 
<br>
$ P_{0} $: (before adding a new conjunct) positive instance 개수
<br>
$ n_{0} $: (before adding a new conjunct) negative instance 개수
<br>
$ P_{1} $: (after adding a new conjunct) positive instance 개수
<br>
$ n_{1} $: (after adding a new conjunct) negative instance 개수
</div>
<br>
Example)
<div style="border:2px solid; width: 700px;">
Training set은 50개의 positive 와 100개의 negative example을 가지며, 아래와 같은 rule이 있을 때 각 matrics의 value는?
<br>
<br>
 * r1 : covers 20 positive and 5 negative examples
 <br>
 * r2 : covers 5 positive and 1 negative examples
 <br>
 <br>
 Accuracy$_{r1} = \frac{20}{25} = 0.8$
 <br>
 Accuracy$_{r2} = \frac{5}{6} = $
 <br>
 <br>
 Laplace$_{r1} = \frac{20 + 1}{25 + 2} = \frac{21}{27} = $
 <br>
 Laplace$_{r2} = \frac{5 + 1}{6 + 2} = \frac{6}{8} = 0.75$
 <br>
 <br>
 FOIL's information gain$_{r1} = 20 \times (log_{2}\frac{20}{20+5} - log_{2}\frac{50}{50+100})= $
 <br>
 FOIL's information gain$_{r2} = 5 \times (log_{5}\frac{20}{5+1} - log_{2}\frac{50}{50+100}) = $
 
</div>
<br>
마지막으로 그렇다면, rule 생성을 언제 까지 해야 하는가?
<br>

***

### Stopping criterion and rule pruning

언제 rule 생성을 그만 두어야 하는가?  
1. Gain 이나 accuracy를 계산

2. 계산된 gain, 또는 accuracy에 변화가 없을 때 stop하거나 다른 rule 생성을 시작

#### Rule pruning
* generalization errors를 개선하는데 이용될 수 있음.
* decision tree의 post-pruning과 비슷
<br>

***

### Rule growing strategy

* General to spcific : empty rule에서 시작해서 특정 기준을 만족 할 때까지 rule의 quality를 개선 시키는 방향으로 rule을 추가
![예시그림]()


* Specific to general : 무작위로 positive example 하나를 선택해서 특정 기준을 만족할 때 까지, 더 많은 positive example을 포함 하는 방향으로 rule을 제거
![예시그림]()