# Seaborn

- matplotlib보다 많이 쓰이는데, 통계 기반 plot을 손쉽게 그릴 수 있기 때문이다.
- matplotlib에서 groupby 또는 pivot_table을 만들어서 시각화해야 하는 것들을 하지 않고도 쉽게 시각화할 수 있게 도와준다.

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

plt.rcParams["figure.figsize"] =(8, 5)
```



## Countplot

- 항목별 개수를 세어 주는 시각화 방법
- 알아서 컬럼을 구성하고 있는 value들을 구분해서 보여 준다.

```
titanic = sns.load_datas("titanic")
tips = sns.load_datas("tips")
```

```
sns.set(style='darkgrid')

sns.countplot(x="class", data=titanic)
plt.show()
```



`hue`라는 옵션을 활용해서 원하는 카테고리별 비교 시각화가 가능하다.

```
sns.countplot(x="class", data=titanic, hue='who')
plt.show()
```

 

### 가로로 그리기

```
sns.countplot(y='class', hue='who', data=titanic)
plt.show()
```



### 색상 팔레트 지정하기

```
sns.countplot(x='class', hue='who', data=titanic, palette='copper')
plt.show()
```



## Distplot (자주 사용 ★)

- matplotlib의 `hist` 그래프 + `kdeplot`(밀도)을 통합한 그래프
- **분포와 밀도**를 확인할 수 있다.

```
N = 100000
X = np.random.randn(N)
```

```
sns.distplot(x)
plt.show()
```



### 데이터가 Series인 경우

```
x = pd.Series(x, name="x vars")
sns.distplot(x)
plt.show()
```



### 데이터셋에서 그리기

```
sns.distplot(titanic["fare"])
plt.show()
```



### rugplot

- 데이터의 위치를 x축 위에 작은 선분(rug)으로 나타내어서, 데이터의 위치와 분포를 보여 준다.

```
x= np.random.randn(100)
sns.distplot(x, rug=True, hist=False)
plt.show()
```



### kde

- 커널 밀도 추정 그래프

```
sns.distplot(x, rug=True, hist=False, kde=True)
plt.show()
```



### 가로로 표현하기

```
x sns.distplot(x, vertical=True)
plt.show()
```



### 컬러 변경하기

```
sns.distplot(x, color="y")
plt.show()
```



## Heatmap

- 데이터의 크기를 색상의 농도로 표현해 주는 기법

```
x = np.random.rand(10, 12)
sns.heatmap(x, annot=True)
plt.show()
```



### pivot table을 활용하여 히트뱁 그리기

```
pivot = tips.pivot_table(index='day', columns='size', values='tip')
pivot

# annot : 히트 맵 위에 그려질 숫자
sns.heatmap(pivot, cmap="Blues", annot=True)
plt.show()
```



### correlation(상관관계)를 시각화

```
sns.heatmap(titanic.corr(), annot=True, cmap="Reds")
plt.show()
```



## PairPlot

- 각 집합의 조합에 대한 히스토그램과 분포도를 그린다.
- 숫자형 column에 대해서만 그려 준다.

```
sns.pairplot(tips)
plt.show()
```



### hue 옵션으로 특성 구분

```
sns.pairplot(tips, hue='size')
plt.show()
```



### 컬러 팔레트 적용

```
sns.pairplot(tips, hue="size", palette="rainbow")
plt.show()
```



### 사이즈 적용

```
sns.pairplot(tips, hue='size', palette='rainbow', height=5,)
plt.show()
```



## Violinplot

- column에 대한 데이터의 비교 분포도를 확인
  - kde는 density(밀집도), violinplot은 실제값의 빈도
- 곡선으로 되어 있는 뚱뚱한 부분이 실제값의 빈도이다.
- 양쪽 끝 뾰족한 부분은 데이터의 최소와 최대를 나타낸다.

```
sns.violinplot(x=tips["total_bill"])
```



### 비교 분포 확인

x, y 축을 동시에 지정하여 바이올린을 분할, 비교 분포를 확인

```
sns.violinplot(x="day", y="total_bill", data=tips)
plt.show()
```

- 가로 형태 violinplot

```
sns.violinplot(y='day', x='total_bill', data=tips)
plt.show()
```



### hue 옵션으로 비교

- hue 옵션을 부여하면 단일 컬럼에 대한 바이올린 모양의 비교를 할 수 있다.

```
# hue 옵션으로 분할된 바이올린 플롯을 각각 합치고 싶으면 split=True를 주면 된다.

sns.violinplot(x='day', y='total_bill', data=tips, hue='smoker', split=True)
plt.show()
```



## mplot

- 컬럼 간의 선형 관계를 확인하기 좋다.

```
sns.lmplot(x='total_bill', y='tip', height=6, data=tips)
plt.show()
```



### hue 옵션으로 다중 선형 관계 그리기

```
sns.lmplot(x='total_bill', y='tip', hue='smoker', height=5, data=tips)
plt.show()
```



### col 옵션 사용해 보기

`col_wrap` 옵션을 이용해 한 줄(row)에 표기할 컬럼의 개수 명시 가능

```
sns.lmplot(x='total_bill', y='tip', hue='smoker',  data=tips, col='day', col_wrap=2)
plt.show()
```



## Replot

- 두 컬럼 간의 상관관계를 보여 주지만, lmplot처럼 선형관계를 그려 주지는 않는다.

```
sns.relplot(x='total_bill', y='tip', hue='day', data=tips)
plt.show()
```



### col 옵션으로 그래프 분할

```
sns.relplot(x='total_bill', y='tip', hue='day', col='time', data=tips)
plt.show()
```



### 그래프의 row와 column에 표기할 컬럼을 선택

```
sns.relplot(x='total_bill', y='tip', hue='day', row='sex', col='time', data=tips)
plt.show()
```



### 컬러 팔레트 적용

```
sns.relplot(x='total_bill', y='tip', hue='day', row='sex', col='time', data=tips, palette='CMRmap_r')
plt.show()
```





## Jointplot

- scatter, histogram을 동시에 그려 준다.
- 숫자형 데이터만 표현 가능하다.

```
sns.jointplot(x='total_bill', y='tip', height=5, data=tips)
plt.show()
```



### 선형 관계를 표현하는 regression line 그리기

- kind='reg' 옵션 추가

```
sns.jointplot(x="total_bill", y="tip", height=5, data=tips, kind='reg') # hue 사용 불가
plt.show()
```



### hex 밀도 보기

- kind='hex' 옵션을 이용해 육각형(hex) 모양의 밀도 확인

```
sns.jointplot(x="total_bill", y="tip", height=5, data=tips, kind='hex')
plt.show()
```



### 등고선 모양으로 밀집도 확인하기

- kind="kde" 옵션으로 밀집도를 선으로 확인

```
sns.jointplot(x="total_bill", y="tip", height=5, data=tips, kind='kde', color='g')
plt.show()
```

