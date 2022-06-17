# 최적화 알고리즘을 이용한 회귀식 추정 - Regression estimation by Optimization Algorithm
___
202101612 안재일<br/>
**2022-Spring Semester<br/>
컴퓨터알고리즘(0009485001) 기말고사**
___
## 시작에 앞서
이번에는 **최적화 알고리즘**을 이용하여 **회귀식 추정**을 하는 것이 최종적인 목표이지만, 이 과정에서 고려해야할 부분이 많다. 무엇보다 **회귀**라는 개념을 처음 접하다보니 **회귀**와 **회귀 분석** 등을 먼저 이해하고 나서, 본격적으로 *유전 알고리즘*으로 **최적화 알고리즘** 구현을 하려고 했다.
___
## 목차
1) **회귀 및 회귀 분석**
2) **최적화 알고리즘 소개 및 정리**
3) **수학적 모형 모델링 및 적용**
4) **단일 회귀 분석**
5) **유전자 알고리즘의 적용**
6) **모수 값 추정 및 과정 분석**
___
## **1) 회귀 및 회귀 분석**
### **회귀(Regression)**
**회귀**란 *독립변수*와 *종속변수* 사이의 상관관계를 나타내는 것을 의미한다. *독립변수*는 한 개 이상이 되어야하고, *종속변수*는 한 개로 고정하고 진행한다. 여기서 독립변수가 1개인 경우를 **단일 회귀**, 여러개인 경우를 **다중 회귀**라고 한다. <br/><br/>
이번에는 그 중에서 **단일 회귀**의 내용을 구체화해보려 한다.

회귀에 대한 이해를 위해 적용할 수 있을만한 예시나 사례를 생각해보았는데, 생각보다 많이 있었다. <br/>

*평수에 따른 주택 가격의 차이, 소득에 따른 행복 지수의 차이, 공부량에 따른 성적별 구간 차이, 운동시간에 따른 근성장량의 차이, 국가별 1인당 GDP에 따른 삶의 만족도 차이 등*

### **회귀 분석(Regression Analysis)**
- 회귀 분석에는 *선형 회귀*라고도 불리는 **단일 회귀 분석**과 **다중 회귀 분석**이 있다. 그 중 **선형 회귀를** 적용해보려 한다.
- 위에서 **단일 회귀 분석**에 관한 내용을 다루겠다고 했는데, **단일 회귀**는 **선형 회귀**라고도 부른다.
- 선형 회귀식은 다음과 같이 표현한다.<br/>
``` y = ax + b ```
<br/>
- 일차식이므로 직선 형태의 그래프가 나타날텐데, 표본공간 내에서 입력받는 데이터들과 어떤 관계를 가지고 있는지 알아보자.
- **a와 x, b를 구해보자**

___
## **2) 최적화 알고리즘 소개 및 정리**
- 최적화 과정을 위해 사용하는 최적화 알고리즘에는 다양한 알고리즘들이 있지만, 그 중 강의 시간에 다뤘던 알고리즘인 *유전자 알고리즘, 모의 담금질 기법*에 대해 다뤄보려 한다.
### **유전자 알고리즘(Genetic Algorithm)**
- 다윈의 진화론으로부터 창안된 해 탐색 알고리즘 중 하나로, 특정 문제를 최적화하는데 매우 유용한 알고리즘이다.
- 적자생존, 즉 생물이 살아가는 환경에 적응 및 진화를 반복해가는 모습을 알고리즘에 모방하여 최적의 해를 찾아내기 위한 방법이라고 할 수 있다.
> **유전자 알고리즘의 수행 과정**
1. 여러 개의 해를 랜덤으로 생성하여 초기 세대로 설정한다.
2. 반복문을 진행하는데, 현재 세대에서 다음 세대까지 해를 생성한다.
3. 가장 우수한 해를 찾을 때 까지 반복하고 우수한 해를 찾으면 반복문을 중지한다.
    <br/>
    
※ **Note!** 이 과정에서 최적해 혹은 최적해의 근사값이 될 수 있으므로 이것을 *후보해(candidate solution)* 로 부른다.

- 후보해를 찾는 연산으로는 **선택(Selection), 교차(Crossover), 돌연변이(Mutation)** 등이 있다.

![사진 1. 유전자 알고리즘 by paperswithcode.com](https://production-media.paperswithcode.com/methods/gadiagram-300x196_jThbitI.png)
<br/>
     *> 사진 1. 유전자 알고리즘 byt paperswithcode.com*
 <br/>


### **모의 담금질 기법(Simulated Annealing)**
- 높은 온도의 액체가 점점 식으면서 결정체로 변하는 과정을 모방한 알고리즘이다.
- 융용 상태에선 물질의 분자가 자유롭게 이동할 수 있는 특성을 모방하여 해를 탐색한다.
- 유전자 알고리즘과 달리 추가적으로 후보해에 대해서 이웃하는 해인 이웃해를 정의해주는데, 최적의 해를 찾을 때까지 이웃해를 옮겨가면서 반복한다.
> **모의 담금질 기법 수행과정**
1. 임의의 후보해를 먼저 선정한다.
2. 초기 T(온도)를 정한다.
3. 반복문에 들어간다.<br/>
   ① 후보해 a의 이웃해 중 하나의 이웃해 a'를 선택한다. <br/>
   ② if a < a'? a'가 a로 이동한다.<br/>
   ③ else? 0 < q < 1인 p가 있는데, 여기서 q < p? (p는 자유롭게 모의 담금질 내에서 해를 탐색할 확률이다) <br/> 
     -1) *q < p* 라면?
     -2) 이웃해 a'를 a로 이동한다.
4. 1보다 작은 상수 k를 T에 곱해 새로운 T를 만든다.
5. 3-4 과정을 조건을 만족할 때 까지 반복 후 후보해 a를 반환하면 끝이다.    

- 모의 담금질 기법으로 해를 탐색하기 위해선, **후보해에 대한 이웃해의 정의**가 필요하다.
- *여행자 문제(TSP, Traveling Salesman Problem)의 이웃해를 정의*하는 3가지는 **삽입(Insertion), 교환(Switching), 반전(Inversion)** 등이 있다.
___
## **3) 수학적 모형 모델링 및 적용**
- 선형 회귀 분석을 위해 내가 선택한 주제는 **운전 시간에 따른 피로도**이다. **독립 변수**를 **운전 시간**으로 두고, **종속 변수**를 **피로도**로 둔다. (※ 그러나, *운전자의 건강 상태나 체력, 컨디션, 전날 수면 시간 등* 독립 변수가 될 수 있거나 독립 변수에 영향을 줄 수 있는건 전부 배제하고, 오직 **운전 시간**으로만 독립 변수를 설정했다.)
![사진2. by hidoc.co.kr](https://src.hidoc.co.kr/image/lib/2020/7/6/1594025524745_0.jpg)

    *> 사진 2. by src.hidoc.co.kr* 
 <br/>

- 모델을 정했으니, **운전 시간 별 피로도**의 *독립변수와 종속변수* 생각을 해보아야 한다. 그 전에, 이 주제를 선정한 이유는 2021년 2월에 면허를 취득하고 운전을 한지 1년여밖에 되지 않았지만, **단거리 및 장거리 운전**을 많이 하면서 느꼈던 **피로도**가 매번 달랐기에 이 부분에 대해 회귀 분석을 진행해보고 싶었다.
- **독립 변수**의 기준으로 거리 단위로 할지 시간 단위로 할지 고민했지만, 교통 체증을 고려해보면 같은 거리라도 교통 체증이 없을 때보다 몇 배가 걸릴 수 있으므로 **시간**으로 결정했다.
- **종속 변수**는 당연히 **피로도**이다. 여기서 **피로도**는 *졸림의 정도 + 몸이 뻐근한 정도*로 정의하고 들어가기로 하자.
- 그래프로 나타내기 위해 구체적인 단위 설정이 필요했다. 그래서 **독립 변수**인 시간 기준은 1시간 단위로 설정하고, 최대 8시간까지 설정했다. 운전 시간은 출발지에서 도착지까지의 총 소요시간으로 결정했다.
- **종속 변수**인 피로도는 0부터 6까지로 설정했다. 그 기준은 다음과 같다.
  
|수치|설명|
|:-:|:---------:|
|0|운전을 하고 있지 않음|
|1|전혀 피곤하지 않음|
|2|하품을 하기 시작함|
|3|하품을 하고, 어깨와 팔이 긴장됨|
|4|하품이 잦아지고, 어깨와 팔이 긴장되고, 온몸이 뻐근하기 시작함|
|5|하품이 계속 나오고 온몸이 뻐근하고 눈이 조금씩 감기기 시작함|
|6|온몸이 녹초가 되고, 눈이 계속 감김|
- 운전 시간에 따른 피로도 데이터값은 나를 포함한 가족, 주변 지인들에게서 구해 그래프에 반영했다.
- 원래 계획은 정말 무작위 수를 뽑아서 그래프에 나타낼지 고민도 했지만, 장시간 운전을 할수록 피곤해지는 조건을 고려했다.
- **5명에게 8개씩** 데이터를 받아 **총 40개**의 데이터를 받았고, 이 데이터가 독립적이고 무작위 표본을 구성해야 하므로 40개의 데이터 중 무작위로 추출한다.
- 추출한 데이터를 표와 그래프 상에 점으로 나타낸다. <br/>
![사진 3. 표본](https://user-images.githubusercontent.com/68879690/174206507-ef6c1754-2bc8-449a-ae10-51d8a1d66d35.PNG)
<br/>
#### > 사진 3. 표본
<br/>


![사진 4. 피로도](https://user-images.githubusercontent.com/68879690/174198426-55c523a2-9ce8-47b0-aaf5-e0a9cb10d72f.PNG)
<br/>
#### > 사진 4. 피로도
<br/>

___

## **4) 단일 회귀 분석(선형 회귀)**
- 선형 회귀 분석을 진행하는 과정에서 직선을 나타내는 방법은 여러가지가 있는데 그 중 **최소제곱법**으로 공간 내에 직선을 만들어 볼 것 이다.<br/>
### **최소제곱법**
![사진 5. 최소제곱법](https://user-images.githubusercontent.com/68879690/174206246-929e95b6-f22e-4259-abf1-e78cd37ae7d5.PNG) <br/>
##### > 사진 5. 최소제곱법
<br/>

- ```y = ax + b```에서 a와 b값을 구하기 위해, 최소제곱법을 사용한다고 밝혔다.
-  그리고 주어진 $X, Y$를 이용해서 $ X $와 $Y$의 합과 평균, $X^2$의 합, $XY$의 합도 구해준다.
- 최소제곱법을 사용하기 위해 위의 최소제곱법 식을 각각 **편미분**해준다.
- ![사진 6. 계수 및 상수 -> y = ax +b](https://user-images.githubusercontent.com/68879690/174211002-5f093e9c-c663-48f7-acd8-908dfe2b8034.PNG) <br/>
    ##### > 사진 6. 계수 및 상수 구하기
<br/>

![사진 7. 표본합본](https://user-images.githubusercontent.com/68879690/174208374-fe7abe9c-1cf9-46d9-bb52-c5fa86482ce8.PNG) <br/>
##### 사진 7. 변수 및 합, 곱 (표본합본)
<br/>

||$X$|$Y$|$X^2$|$XY$|
|:-:|:-:|:-:|:-:|:-:|
|합계|171|135.6|961|721.1|
|평균|4.2|3.4|X|X|

- 위 식을 통해 알 수 있는 것은 **a = 0.615, b = 0.817**으로 정리해서 $Y = aX + b$에 대입을 해주자.
 다시 쓰면 ``` y = aX + B = 0.615x + 0.817```이다. 따라서, 회귀 분석 과정을 통해 식을 구했다.
- 이제 이 식을 **사진 4** 위에 일차원 그래프로 나타낼 수 있다. 
- **빨간색 일차원 직선 그래프**가 ```y = 0.615x + 0.817``` 이다.
$$ Y = aX + B = 0.615X + 0.817$$
![사진 8. 직선 추가](https://user-images.githubusercontent.com/68879690/174214208-0da3ce10-9c8b-49c9-8526-360426a7b1c6.jpg) <br/>
##### > 사진 8. 직선 추가
___
## **5) 유전자 알고리즘의 적용(⊂ 최적화 알고리즘)**


___
## **6) 모수 값 추정 및 과정 분석**

___
## Reference
- **'알기 쉬운 알고리즘'** - *양성봉* 著
- [선형회귀분석 - 엑셀](https://loadtoexcelmaster.tistory.com/entry/%EC%97%91%EC%85%80%EC%97%90%EC%84%9C-%EC%A0%90%EA%B7%B8%EB%9E%98%ED%94%84Dot-Plot-%EB%A7%8C%EB%93%A4%EA%B8%B0)
- [Youtube - Genetic Algorithm](https://youtu.be/yUW8yg4_j6w)
- [선형회귀분석](https://m.blog.naver.com/aporia25/221159322718)