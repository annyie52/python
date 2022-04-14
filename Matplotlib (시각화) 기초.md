# Matplotlib (시각화)

```
import matplotlib.pyplot as plt
```



## Colab 한글 깨짐 방지

실행 후 런타임 다시 시작해야 적용된다.

```
!apt -qq -y install fonts-nanum > /dev/null

import matplotlib.pyplot as plt
import matplotlib.font_manager as fm

fontpath = '/usr/share/fonts/truetype/nanum/NanumBarunGothic.ttf'
font = fm.FontProperties(fname=fontpath, size=10)
fm._rebuild()

# 그래프에 retina display 적용
%config InlineBackend.figure_format = 'retina'

# Colab 의 한글 폰트 설정
plt.rc('font', family='NanumBarunGothic') 
```



## 밑그림 그리기

```
import numpy as np

# 캔버스 그리기 (생략 가능)
plt.figure(figsize=(15, 8))

# 데이터 생성
data = np.arange(1, 100)

# 그리기(plot)
plt.plot(data)

# 보여 주기
plt.show()
```



## 다중 그래프- 한 캔버스에 그래프 동시에 그리기

```
data1 = np.arange(1, 51)
data2 = np.arange(51, 101)

plt.plot(data1)
plt.plot(data2)

plt.show()
```



## 캔버스를 여러 개 그리기

- `plt.figure()`을 활용해서 새로운 `canvas`를 만든다.

```
plt.plot(data1)

plt.figure()
plt.plot(data2)

plt.show()
```



## 하나의 캔버스에 여러 개의 plot 그리기

- `plt.subplot(row, column, index)`

```
plt.subplot(2, 1, 1)
plt.plot(data1)

plt.subplot(2, 1, 2)
plt.plot(data2)

plt.show()
```



- `subplots(행의 개수, 열의 개수)`
  - `figure`
  - `axes`

```
fig, axes = plt.subplots(2, 3)

# set_size_inches 메소드 활용해서 그래프 크기 조절하기
fig.set_size_inches((15, 8))

axes[0, 0].plot(data1)
axes[0, 1].plot(data1 ** 2)
axes[0, 2].plot(data1 ** 3)

axes[1, 0].plot(data % 10)
axes[1, 1].plot(-1 * (data ** 2))
axes[1, 2].plot(data // 20)

plt.tight_layout()
plt.show()
```



## 스타일 옵션 

<img src='https://matplotlib.org/_images/anatomy.png' width='600' />



### 제목

```
plt.plot(data1)
plt.plot(data2)

# 제목 삽입
plt.title("타이틀", fontsize=20)
plt.show()
```



### X, Y축 라벨 설정

```
plt.plot(data1)
plt.plot(data2)

plt.title("라벨")

# x축 label 설정
plt.xlabel("X축")
plt.ylabel("Y축")

plt.show()
```



### X, Y축 tick 설정(rotation)

```
plt.xticks(rotation=45)
plt.yticks(rotation=30)

plt.show()
```



### 범례 설정

```
plt.plot(np.arange(10), np.arange(10) * 2)
plt.plot(np.arange(10), np.arange(10) ** 2)
plt.plot(np.arange(10), np.log(np.arange(10)))

# 여러 개의 범례를 설정할 경우 리스트 형태로 삽입
plt.legend(["10 * 2", "10 ** 2", "log"])

plt.show()
```



### X, Y의 한계점(limit)

```
plt.xlim(0, 5)
plt.ylim(0.5, 10)

plt.show()
```



### 스타일 세부 설정: marker

- '.' point marker
- ',' pixel marker
- 'o' circle marker
- 'v' triangle_down marker
- '^' triangle_up marker
- '<' triangle_left marker
- '>' triangle_right marker
- '1' tri_down marker
- '2' tri_up marker
- '3' tri_left marker
- '4' tri_right marker
- 's ' square marker
- 'p' pentagon marker
- '*' star marker
- 'h' hexagon1 marker
- 'H' hexagon2 marker
- '+' plus marker
- 'x' x marker
- 'D' diamond marker
- 'd' thin_diamond marker
- '|' vline marker
- '_' hline marker

```
plt.plot(np.arange(10), np.arange(10) * 2, marker='o', markersize=5)
plt.plot(np.arange(10), np.arange(10) * 2 - 10, marker='H', markersize=10)
plt.plot(np.arange(10), np.arange(10) * 2 - 20, marker='D', markersize=15)
plt.plot(np.arange(10), np.arange(10) * 2 - 30, marker='*', markersize=20)
```



### 스타일 세부 설정: line

- '-' solid line style (기본)
- '--' dashed line style
- '-.' dash-dot line style
- ':' dotted line style

```
plt.plot(np.arange(10), np.arange(10) * 2,      marker='o', linestyle='')   # 선이 안 보임
plt.plot(np.arange(10), np.arange(10) * 2 - 10, marker='H', linestyle='-')  # 실선
plt.plot(np.arange(10), np.arange(10) * 2 - 20, marker='D', linestyle='--') # 짧은 실선
plt.plot(np.arange(10), np.arange(10) * 2 - 30, marker='*', linestyle='-.') # 선과 점을 엮어서
plt.plot(np.arange(10), np.arange(10) * 2 - 40, marker='v', linestyle=':') #점선
```



### 스타일 세부 설정: color

- 'b' blue
- 'g' green
- 'r' red
- 'c' cyan
- 'm' magenta
- 'y' yellow
- 'k' black
- 'w' white

```
plt.plot(np.arange(10), np.arange(10) * 2,      marker='o', linestyle='', color='b')
plt.plot(np.arange(10), np.arange(10) * 2 - 10, marker='H', linestyle='-', color='r') 
plt.plot(np.arange(10), np.arange(10) * 2 - 20, marker='D', linestyle='--', color='c')
plt.plot(np.arange(10), np.arange(10) * 2 - 30, marker='*', linestyle='-.', color='y')
```



### 투명도 설정

- `alpha` 옵션을 0 ~ 1사이로 조절. 값이 작을 수록 투명해 진다.

```
plt.plot(np.arange(10), np.arange(10) * 2 - 30, marker='*', color='y', alpha=0.5)
```



## Matplotlib 그래프 종류

### Lineplot

- 시간의 흐름에 따른 데이터의 변화를 보기 좋다
- time 뿐만 아니고, Sequence도 포함

```
np.random.seed(42)
x = np.arange(50)
y = np.random.rand(50)

plt.plot(x, y)

plt.xlabel("x value")
plt.ylabel("y value")
plt.title("line graph")

plt.grid()
plt.show()
```



### Scatterplot

- 산점도 그래프
- 데이터 포인트의 분포를 확인할 때 사용하면 좋다.

```
plt.scatter(x, y)

plt.xlabel("x value")
plt.ylabel("y value")

plt.title("scatter plot")

plt.show()
```



#### colors, area 설정

- colors은 임의 값을 color 값으로 변환
- area는 점의 넓이를 나타낸다. 값이 커지면 area로 커진다.

```
colors = np.arange(50)
area = x * y * 1000

plt.scatter(x, y,
            s=area, # 점의 크기 설정(s)
            c=colors) # 점의 색상 설정(c)
plt.show()
```



#### cmap과 alpha 설정

- cmap에 컬러를 지정하면, 컬러 값을 모두 같게 가져갈 수 있다.

```
plt.scatter(x, y, s=area, cmap='blue', alpha=0.1)
plt.title("alpha = 0.1")
```



### Barplot, Barhplot

- 카테고리(항목)별 집계 또는 조사된 데이터를 표시할 때 보기 좋다.



#### Barplot

```
x = ["수학", '과학', "영어", "컴퓨터", "미술", "체육"]
y = [ 66,     80,     85,     50,       80,     90   ]

# plt.bar(x, y)
plt.bar(x, y, align='center', alpha=0.7, color='red')

plt.xticks(x) # x축에 표시할 값들

plt.ylabel("점수")
plt.title("성적")
plt.show()
```



#### Barhplot

```
x = ["수학", '과학', "영어", "컴퓨터", "미술", "체육"]
y = [ 66,     80,     85,     50,       80,     90   ]

# plt.barh(x, y)
plt.barh(x, y, align='center', alpha=0.7, color='green')

# plt.yticks(x) # x축에 표시할 내용이 y축으로 갔기 때문에 tick을 x에 들어갈 값으로 설정

plt.ylabel("점수")
plt.title("성적")
plt.show()
```



#### 비교 그래프 그리기

```
x_label = ["수학", '과학', "영어", "컴퓨터", "미술", "체육"]
x = np.arange(len(x_label))

y_1 = [ 66,     80,     85,     50,       80,     90   ]
y_2 = [ 85,     75,     91,     43,       77,     70   ]

# 넓이 지정
width = 0.35

# subplots 생성
fig, axes = plt.subplots

axes.bar(x - width/2, y_1, width, align='center', alpha=0.5)
axes.bar(x + width/2, y_2, width, align='center', alpha=0.8)

# xticks 설정
plt.xticks(x)
axes.set_xticklabels(x_label)

plt.ylabel("점수")
plt.xlabel("과목")

plt.legend(["학생 A","학생 B"])

plt.show()
```



### Areaplot

```
np.random.seed(42)
x = np.arange(1, 21)
y = np.random.randint(low=5, high=10, size=20)

plt.fill_between(x, y, color='green', alpha=0.6)
plt.show()
```



### 히스토그램

```
N = 100000 # 데이터의 개수

# 중요한 변수
bins = 30 # 범위

x = np.random.randn(N)

plt.hist(x, bins=bins)
plt.show()
```



#### 밀집도 표현

```
N = 100000
bins = 30

np.random.seed(42)
x = np.random.randn(N)

fig, axes = plt.subplots(1, 2, tight_layout=True)
fig.set_size_inches(9, 3)

# density=True 값을 통하여 Y축에 Density를 표기할 수 있다.
axes[0].hist(x, bins=bins, density=True, cumulative=True) # cumulative : 개수를 누적
axes[1].hist(x, bins=bins)

plt.show()
```



### 파이 차트

**pie chart 옵션**

- explode: 파이에서 툭 튀어져 나온 비율
- autopct: 퍼센트 자동으로 표기
- shadow: 그림자 표시
- startangle: 파이를 그리기 시작할 각도

texts, autotexts 인자를 파이차트를 그리면 리턴 받습니다.

- texts : label에 대한 텍스트 효과
- autotexts : 파이 위에 그려지는 텍스트 효과를 다를 때 사용

```
labels =  ["A",    "B",  "C", "D", "E", "F"]
sizes  =  [20.4,   15.8, 10.5,   9, 7.6, 36.7]
explode = (0.3,    0, 0, 0, 0, 0)

# patches 범례 기호
patches, texts, autotexts = plt.pie(
    sizes,
    explode=explode,
    labels=labels,
    autopct="%1.1f%%",
    shadow=True,
    startangle=90
)

# label 텍스트 스타일링
for t in texts:
  t.set_fontsize(12)
  t.set_color('blue')

# pie 위의 텍스트에 대한 스타일링
for t in autotexts:
  t.set_fontsize(12)
  t.set_color('white')

plt.title("파이차트 그려 보기!")
plt.show()
```



### Box Plot

- 데이터의 최대, 최소, 1Q, 3Q 지점 등 통계적인 부분을 손쉽게 시각화한다.
- 이상치도 빠르게 판단이 가능하다.

```
np.random.seed(42)
# 기본 데이터 ( 정상 범위 내 데이터 )
spread = np.random.rand(50) * 100 # 최소 0, 최대 100
center = np.ones(25) * 50

# high : 최대, low : 최소
flier_high = np.random.rand(10) * 100 + 100 # 최소 100 ~ 최대 200
flier_low  = np.random.rand(10) * -100 # 최소 -100 ~ 최대 0

# 위에서 만든 4가지 데이터를 모두 합치기
data = np.concatenate([spread, center, flier_high, flier_low])
data
```

```
plt.figure(figsize=(15, 8))
# 음수 표현 설정
plt.rcParams["axes.unicode_minus"] = False

plt.boxplot(data)
plt.show()
```



- `IQR`= (3Q-1Q) * 1.5



#### 축 변경하기

```
plt.boxplot(data, vert=False)
plt.show()
```



#### outlier marker, 심볼, 컬러 변경

```
outlier_marker = {'marker': 'D', 'markerfacecolor': 'r'}

plt.boxplot(data, flierprops=outlier_marker) # flierprops : outlier 표시용 점
plt.show()
```

