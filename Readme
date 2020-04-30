사용한 STL들-> sort , random_shuffle, pow, sqrt, 그리고 vector를 사용하였다.
코드 큰틀
Mian(){
	그래프 찍기위한 ofstream ifstream 사용
  data들 같은 종류끼리 벡터에 저장
  같은 종류의 data의 4가지 요소의 벡터에 저장 sort 및 count함수(직접만듬)로 해당 요소의 값에 대한 개수를 ofstram 및 따로 벡터에 저장.
	
  While(){
		사용자한테 값 입력받음 {0, 1, 2 }3개중 하나
    0 -> 3번문제인 By finding pmf 인 분류기 작동
    1 -> 4번문제인 By gaussian 인 분류기 작동
    2 - > 끝내기
  } 
}

아이디어
K_flod 교차검증때 검색결과 test 셋 없이 training, validation 만 가지고 검증을 해도 된다. 하여 최대한 주어진 data set을 활용하기위해 training 후 validation 으로 test 결과값 확인을 5번 반복 반복할 때 training set 과 validation 변경 이후 총 결과값으로 Precision Recall 구함
가우스의 결정 이론에 조금의 검정 아이디어 추가 (통계학떄 배운것들 응용)
Finding pmf 경우 윌콕슨 순위합검정 아이디어 추가(4개의 요소 뭉쳐서 사용위해)
Gaussian 경우 z검정의 아이디어 추가(4개 요소 뭉쳐서 사용위해)



	
3.	classify three classes of iris based by finding pmf
![finding pmf](https://user-images.githubusercontent.com/37677425/80693721-6647b580-8b0e-11ea-8fb9-5d78ffab3927.png)
위 코드서 K_Fold_cross(0)이 실행되면 위와 같은 구조로 함수가 돌아간다.
K_Fold_cross{
		Dataset 랜덤 셔플
		5번 반복{
			Training set, validation set 나누기
			Classify()
    }:	
}
Classify{
		Traing set으로 counting()
		Counting 된 값들로 getPosterior()
		얻은 Posterior와 validationg set 으로 ranktest()
}
Counting{
		training set의 iris 종류, 요소의 값에 따라 해당 개수를 세서 벡터저장
}
getPosterior{
		얻은 counting으로 공식그대로 사용하여 Posterior를 구함
}
Ranktest{
		Validation의 sL, sW, pL, pW의 값을 위에서 구한 Posterior을 사용
    확률들을 구한다. 여기서 베이즈의 결정 이론에 따르면 확률이 높은 친구가 결정되는데 4개의 요소가 존재하기 때문에 2개요소는 A 2개요소는 B를 골랐을 때 하나로 고를 수 있지 않음. 
    따라서 윌콕스 순위합검정 아이디어를 차용
    총 12개의 확률을 크기가 작은순대로 순위를 매김.
		만약 확률 크기가 같으면 순위의 평균값 사용
		예) 0 0 0 0 0.2 0.4 0.6 0.8 1  1  1  1 의 확률이 나왔을경우
		   1 2 3 4  5  6  7  8  9 10 11 12 의 순위가 나옴
		  (Setosa|X)의 확률이 4개요소 순서대로 1 0.6 1 0 일 경우
		  1->10.5 ,  0.6 -> 7  0-> 2.5 의 순위이므로 setosa의 총 점수 20
		  이렇게 3개의 종류 다 구하고 가장 큰 종류가 결정된다.
}
 
출력 결과물을 이렇게 나온다. TP FP FN은 맨 마지막 결과에서만 나오고 해당 validation set을 분류할때마다 그때 Posterior와 무엇으로 분류했는지 그게 맞았는지 나온다.

분류기에 대한 나의 평가
분류기에 대한 평가로 FP, FN이 아에 0이 나온적은 없다. 또한 3개중 꼭 하나로 결정해서 출력하기 때문에 틀렸을경우 FP와 FN이 같이 증가하게 된다.
위 결과물은 최악일 때 결과이며 보통 12~17사이로 나왔었다. 
최악의 경우에 80후반대의 Precision과 Recall이 나오므로 분류기로써 쓸만하다고 생각한다.
하지만 error는 존재하므로 error가 아예 나오지 않게 하려면 새로운 무언가가 추가되거나 접근법을 다르게 해야할 것 같다.


4.	classify three classes of iris based by gaussian distribute

<img src="https://user-images.githubusercontent.com/37677425/80693724-6778e280-8b0e-11ea-96b9-368c9efad163.png" width="90%"></img>
K_Fold_cross(1)일경우 위와 같은 구성으로 코드가 돌아간다.
K_Fold_cross{
		Dataset 랜덤 셔플
		5번 반복{
			Training set, validation set 나누기
			gaussianClassify()
		}
}
GaussianClassify{
		Traing set으로 findMeananda()
		얻은 Mean과 표준편차, validationg set 으로 GaussianCheck()
}
GaussinCheck{
	Finding pmf 와 같이 정규분포를 통해 확률을 구하여도 4가지 요소를 일일히 비교하여 한 종류를 고를경우 2:2의 상황일 경우 하나로 특정하는데 에러가 생긴다.
	따라서 여기서 z검정 아이디어를 차용해왔다. 
	x-mean/a(표준편차) 가 유의수준 b%안에서 맞다 할 수 있으려면 특정 수치를 넘기면 안된다. 1.645 보다 클 경우 10%안에서 해당 값은 평균을 가지는 해당 종류라 할 수 없다. 
	예로 setosa의 sepal length의 평균이 4.1 이고 표준편차가 1 일때
	x값이 5.9 이라면  5.9 – 4.1 / 1 = 1.8로 1.645 보다 크므로 유의수준 10퍼 안에서 sepal length값이 5.9인 iris는 setosa라 할 수 없다.
	하지만 이렇게 딱 잘라서 누군 맞고 안맞고 하다보니 FP 와 FN이 너무 높게 나와 특별한 나만의 공식을 사용하기로 했다.
	유의수준 10퍼, 5퍼, 1퍼, 0.1퍼 총 4개의 값을 가지고 해당 유의수준에서 특정 종류라고 할 수 없는 값이 나올경우 그 범위에 따라 차등 점수를 지급 그 후 총 점수가 일정치 이하일 경우 특정 종류라고 하자! 라는 것이다.
	유의수준의 숫자가 작을수록 관대한 것이기에 해당 점수는 이렇게 차등으로 주었다.
	0.1% -> 12   1% -> 6   5% -> 3    10% -> 1 
	또한 4가지 요소를 가지므로 유의수준에 들어오지 못한 요소 의 개수만큼 해당 점수를 곱해주었다.
	즉 sL하나가 5퍼에 못들었으며 pW가 1퍼에 들지 못했으면 (3+6)*2 = 18점
	또한 총 점수가 15점을 넘기 않을때만 해당 종류라고 결정하였다.
}


분류기에 대한 나의 평가
이 분류기는 FP와 FN이 다른 상황에서 각자 한 개씩 증가한다. 그렇기에 공식대로 둘은 서로 trade-off 관계로 늘었다가 줄어들었다가 했다. 그렇기에 최대한 둘이 비슷한 숫자가 나올 수 있는 숫자들로 gaussianCheck를 했다. 그 결과 둘다 15~25 사이의 값을 가지며 나타났다. 위 결과값은 FP가 최상의 결과가 나타난 것이다.
보통 Precision과 Recall 이 0.85에서 줄거나 증가하므로 최대한 좋은 결과가 나온게 아닌가 싶다. 분류기로써 괜찮게 작동하는 것 같다. 다만 완벽한 분류기 또는 둘다 90퍼 이상이 나오게 할 수는 없을까 정말 많이 고민했지만 해당 숫자를 변경하는것으로는 한계가 있다는 것을 알 수 있었으며 더 좋은 분류기를 위해서는 분류결정 코드를 다른 방식으로 접근해야 할 것 같다.

