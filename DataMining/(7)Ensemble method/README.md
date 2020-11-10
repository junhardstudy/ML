# 개요

여러 개의 weak base model들을 생성하고, 그 output들을 하나로 취합하여 하나의 output을 만드는 strong model 생성 방법 

![앙상블 매서드 사진](./image/ensemble_method.jpg)
<br>
### Ensemble modeling에서 중요 요구 사항

1. base model들은 서로 서로에 대해 independent 해야 함.

2. 각각의 model들은 binary classification을 할 때, <strong>잘 못 분류하는 error rate가 50% 보다는 반드시 작아야 함</strong>.
<br>
<br>
<hr>

### Approaches for constructing ensemble modeling

1). 각각의 base model에 대해, training data set을 바꾸기.

  * original data로부터 resempling을 거쳐 여러 개의 training set을 생성.
  
  * bagging, boosting등이 있음.

2). 각각의 base model에 대해, input attribute들을 바꾸기.

  * 각각의 training set을 형성할 때, 서로 다른 input attribute set이 선택됨.
  
  * training data가 많은 수의 attribute를 가졌을 때, 사용하기 좋음.
  
  * random forest가 있음.

3). 각각의 base model을 생성할 때, 서로 다른 model algorithm을 적용.

  * 단, training data set은 똑같음.

4). 각각의 base model에 대해, 서로 다른 parameter를 사용.
<br>
<br>
<hr>

### Bagging

* Bootstrap aggregation이라고 함.

* original data set으로부터, 무작위 복원 추출(random sampling with replacement)을 반복 시행.

* 이렇게 생성된 subset data를 bootstrap sample이라함.

* 각각의 bootstrap sample에 의해 classifier 생성.

* class 예측을 위해 vote 방식 사용.
<br>
<br>
<hr>

### Algorithm for Bagging

![algo](./image/algo.png)

<hr>

### Example applied by C programming

<table>
<th>x</th>
<th>0.1</th>
<th>0.2</th>
<th>0.3</th>
<th>0.4</th>
<th>0.5</th>
<th>0.6</th>
<th>0.7</th>
<th>0.8</th>
<th>0.9</th>
<th>1</th>
<tr>
	<td>y</td>
	<td>1</td>
	<td>1</td>
	<td>1</td>
	<td>-1</td>
	<td>-1</td>
	<td>-1</td>
	<td>-1</td>
	<td>1</td>
	<td>1</td>
	<td>1</td>
</tr>
</table>

전체적인 코드는 위와 같은 원본 데이터를 10번 반복하여 무작위 복원 추출하여 10개의 sample을 만든 뒤, bagging을 하고 각 base model에 대해 split 값을 구하는 것 입니다.
<br>
<br>
이 때, split을 하기위한 척도로는 entropy를 사용.
<br>

<br>

```c
void main() {
	int i;
	int j, k, rnd_index;
	double tmp_index;
	double index = 0.1;
	double tmp_entropy = 0.0;
	double entropy = 1.0;
	double model_split[10] = { 0.0 };

	srand((unsigned)time(NULL));

	double ori_x[10] = { 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1 };
	int ori_y[10] = { 1, 1, 1, -1, -1, -1, -1, 1, 1, 1 };
	data original;
	data* sample = malloc(sizeof(data));
	original.x = ori_x;
	original.y = ori_y;

	node tmp_left_node;
	node tmp_right_node;

	tmp_left_node.C1 = 0;
	tmp_left_node.C2 = 0;

	tmp_right_node.C1 = 0;
	tmp_right_node.C2 = 0;

	for (k = 0; k < 10; k++) {
		index = 0.1;
		tmp_entropy = 0.0;
		entropy = 1.0;
		resampling(original, sample);


		for (i = 0, tmp_index = 0.1; i < 10; i++) {
			for (j = 0; sample->x[j] < tmp_index && j < 10; j++) {
				if (sample->y[j] == 1)tmp_left_node.C1++;
				else tmp_left_node.C2++;
			}
			for (; j < 10; j++) {
				if (sample->y[j] == 1)tmp_right_node.C1++;
				else tmp_right_node.C2++;
			}
			tmp_entropy = calc_entropy(tmp_right_node, tmp_left_node);
			if (tmp_entropy < entropy) {
				entropy = tmp_entropy;
				index = tmp_index;
			}
			tmp_index += 0.1;
			/**각 노드에 분류된 클래스 개수 초기화 부분**/
			tmp_left_node.C1 = 0;
			tmp_left_node.C2 = 0;

			tmp_right_node.C1 = 0;
			tmp_right_node.C2 = 0;
		}

		printf("true answer : %f, index = %f\n", entropy, index);
		model_split[k] = index;
	}
	for (int i = 0; i < 10; i++) {
		printf("split : %f\n", model_split[i]);
		//printf("%f | ", sample->x[i]);
	}
	
}

```

<br>
<br>
resampling함수는 무작위로 원본데이터를 복원 추출하여 10개를 가져오는데, 가져오고 나서는 정렬이 필요함

```c
void resampling(data original, data* sample) {
	double tmp;
	int index;
	int tmp_index;
	int i, j;

	sample->x = malloc(sizeof(double) * 10);
	sample->y = malloc(sizeof(int) * 10);

	for (i = 0; i < 10; i++) {

		index = rand() % 10;
		(sample->x)[i] = original.x[index];
		(sample->y)[i] = original.y[index];
	}

	for (i = 0; i < 9; i++) {
		for (j = 0; j < 9 - i; j++) {
			if ((sample->x)[j] > (sample->x)[j + 1]) {
				tmp = (sample->x)[j];
				tmp_index = (sample->y)[j];
				(sample->x)[j] = (sample->x)[j + 1];
				(sample->y)[j] = (sample->y)[j + 1];
				(sample->x)[j + 1] = tmp;
				(sample->y)[j + 1] = tmp_index;
			}
		}
	}
	printf("\n\n\n");
	for (int i = 0; i < 10; i++) {

		printf("%f | ", sample->x[i]);

	}
	printf("\n\n");
}
```

<br>
<br>
그 외에는 밑이 2인 log함수와 entropy 계산 수식을 위한 함수.

```c
double calc_entropy(node Rnode, node Lnode) {
	double tmp1, tmp2;
	double term1, term2, term3, term4;
	double entropy;
	tmp1 = (double)(Rnode.C1 + Rnode.C2);
	tmp2 = (double)(Lnode.C1 + Lnode.C2);

	term1 = Rnode.C1 / tmp1;
	term2 = Rnode.C2 / tmp1;

	term3 = Lnode.C1 / tmp2;
	term4 = Lnode.C2 / tmp2;
	entropy = -tmp1 / 10.0 * (term1 * log_2(term1) + term2 * log_2(term2)) - tmp2 / 10.0 * (term3 * log_2(term3) + term4 * log_2(term4));

	
	return entropy;
}

double log_2(double x) {
	if (x <= 0.0) {
		return 0.0;
	}
	return log(x) / log(2);
}
``` 
<br>
<br>
<br>
<hr>

### Boosting

* Training data만 조정해서 ensemble model을 만드는 또다른 방법.

&nbsp;&nbsp;&nbsp;Bagging과 마찬가지로, training data에 의한 bias를 최소화하여 여러개의 weak learner를 하나의 strong learner로 만드는 방법.
<br>

* Bagging은 training set을 sampling할 때 오로지 random하게만 선택하는데, 이와 다르게 Boosting은 특정 training data에 weight를 부여함으로써 더욱 집중할 수 있음.

&nbsp;&nbsp;&nbsp;즉, training set중에서 분류 하기 어려운 training data에 좀 더 집중하게 함.

>An iterative and sequential procedure of adoptively changing the distribution of training examples for learning base classifiers

<br>
<br>
<hr>

### Boosting Procedure

(1).처음에는 모든 N개의 training examples에 대해 똑같은 weight값을 할당(이는 모든 training data가 똑같은 빈도수로 선택됨.)
<br><br>
(2).training example의 weight를 바탕으로 <strong>boosting sample 생성</strong>.
<br><br>
(3).boosting sample로부터 classifier를 생성하고, original training data의 모든 example에 대하여 분류 수행.
<br><br>
(4).boosting round의 마지막에 training example의 <strong>weight를 갱신</strong>.
<br><br>

	매 boosting round마다, 옳게 예측한 것은 weight를 줄이는 방향으로 진행하고 반대로 잘 못 예측하는 경우에는 weight를 증가하는 방향으로 진행하여 classifier가
	좀 더 분류하기 어려운 training example에 집중 할 수 있게 해줌.

<br><br>

(5).2~4 단계를 원하는 개수의 base model을 얻을 때 까지 반복.
<br><br>
(6).각각의 base classifier의 <strong>결과들을 모두 취합(aggregating)</strong>.

<br>
<br>
<hr>

### AdaBoost(Adaptive Boosting)

* Model의 성능에 기반해 base model에 weight(or importance)인 $\alpha$를 할당. $\rightarrow$ model의 error rate가 낮다면, weight를 높게 줌.

* 예측, 정확도에 기반하여, training records의 weight를 바꿈.

* N개의 training example이 있고, K개의 Base classifiers가 있을 때, 각 각의 Base classifier error rate는

![image](./image/adaepsil.png)

<br>

* error rate에 기반한 base classifier의 importance는,

![image](./image/alpha.png)

training example의 weight를 update하는데 이용

![image](./image/errorini.png)


<br>

* j번째 boosting round에서 i번째 training record에 대한 weight 값 변화는 다음과 같음.<strong>(weight값 갱신)</strong>

![image](./image/weightada.png)

![image](./image/normal.png)

***

## Rapid Miner를 이용한 실습

1. AdaBoost

![image](./image/2.png)

![image](./image/2-2.png)

2. Random forest

![image](./image/1.png)

![image](./image/1-1.png)

***

## 출처 및 참고문헌

Ensemble method에 대한 이론 및 rapid miner 사용법 : 강의 PPT 자료




