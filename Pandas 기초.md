#  Pandas

- 파이썬에서 데이터를 시각적으로 간편하게 다룰 수 있도록 만들어진 라이브러리
- 2차원 배열 데이터에 대한 집계, 시각화 편집 등을 쉽게 할 수 있게 해 준다.
- 정형 데이터(규칙에 의해서 정확하게 데이터가 존재하는 식의 형식, 정해진 형식)를 다룬다.

```
import pandas as pd
```



## Series

- 한 종류의 데이터를 모아 놓은 것
- Series가 여러 개 모이면 `DataFrame`이 된다.

```
pd.Series([1, 2, 3, 4])
```



## DataFrame

- `Series`가 여러 개 모여서 하나의 데이터 시트를 표현하면 `DataFrame`이 된다.



**방법 1:** 2차원 배열을 활용해서 만들기

```
data = [["홍길동", 24, 186],
        ["김철수", 25, 190]]
        
pd.DataFrame(data, columns=["이름", "나이", "키"])
```

**방법 2:** `dict`로 만들기

```
data = {
    "이름" : ["홍길동","김철수","박영희"],
    "나이" : [24, 25, 28],
    "키" : [186, 190, 163.3]
}

pd.DataFrame(data)
```

```
data = [
  {
   "이름": "홍길동",
   "나이": 24,
   "키"  : 186
  },

  {
   "이름": "김철수",
   "나이": 25,
   "키"  : 190
  }
]

pd.DataFrame(data)
```



## CSV 파일 불러오기

- 데이터가 쉼표로 구분되어 있다.
- `CSV`를 사용하면 순수하게 데이터만 가져올 수 있다. 엑셀보다 속도가 훨씬 빠르다.

```
df = pd.read_csv("korean.csv")
```

- 영어권이 아닌 데이터를 불러올 때 한글이나 기타 특수 기호들이 깨지는 경우
  - `read_csv("파일 경로", encoding='utf-8')`
  - `read_csv("파일 경로", encoding='euc-kr')`
  - `read_csv("파일 경로", encoding='ms949')`



##  데이터 프레임의 기본 정보 확인

- `df.columns` : 열 정보 확인
- `df.rename(inplace=True)` : 열 이름 바꾸기
- `df.info`: 데이터의 기본 정보 확인
- `df.describe`: 숫자 데이터 등 각종 수치 확인 (통계 정보)
- `df.shape`: 행과 열 갯수 확인
- `df.head(n)`: 상위 n개 데이터를 보여 주기
- `df.tail(n)`: 하위 n개 데이터를 보여 주기



## 데이터 정렬하기

- 인덱스를 이용한 정렬: `sort_index()`, 내림차순의 경우 `ascending=False` 옵션을 넣어 주기
- 값을 이용한 정렬: `sort_values(by="컬럼 이름")`
  -  두 개 이상의 컬럼을 사용할 때는 앞쪽에 배치된 컬럼을 먼저 고려해서 정렬한다. `by=["컬럼 1", "컬럼 2"]`



## loc 선택

location의 약자

- `loc[행 선택, 열 선택]`
  - 행 선택: 행의 인덱스를 활용하거나, `True/False`로 이루어진 조건 색인
  - 열 선택: 컬럼의 이름(들)



## iloc 선택

- 포지션 선택
- `iloc[행 인덱스 선택, 열 인덱스 선택]`
- 배열의 순번 인덱스 개념을 사용



## 조건 색인 (Boolean Indexing)

- 특정한 조건을 이용해서 그 조건을 만족하는 (`True`) 데이터만 추출해 내는 과정

1. 조건 마스크 생성
2. 생성된 조건 마스크를 행 선택 위치에 넣어 주기

```
# 키가 175 이상인 사람의 이름, 그룹, 소속사, 성별, 키
column_names = ['이름', '그룹', '소속사', '성별', '키']
mask = df['키'] >= 175

df.loc[mask, column_names]
```



- `isin`을 활용한 인덱싱

```
group_list = ["아스트로", "블랙핑크"]
group_mask = df['그룹'].isin(group_list)
df.loc[group_mask]
```



## 결측치 (Null, NaN)

- NaN값 위치 확인: `isna()` (전체 데이터 프레임에서 NaN값에 해당하는지 여부를 출력), `isnull()` (보통 한 컬럼)

- NaN이 아닌 것만 색인: `notnull()`



## 데이터프레임 복사

- `copy()`
- `[:]`



## 데이터의 추가

- row 추가하기
  - `dict` 형식의 데이터를 만들어 준 다음, `append()` 함수를 활용하여 데이터를 추가
  - 반드시 `ignore_index=True`로 설정해야 한다.

```
new_data = {
    "이름": "카리나",
    "그룹": "에스파",
    "소속사": "SM",
    "성별": "여자",
    "생년월일": "1988-01-29",
    "키": 162,
    "혈액형": "AB",
    "브랜드평판지수": 123456789
}

df = df.append(new_data, ignore_index=True)
df
```

- column 추가하기

```
df['국적'] = '대한민국'
df.loc[df['이름'] == '카리나','국적'] = 'Korea'
```



## 통계값 다루기

통계값은 항상 `data type`이 정수(`int`) 실수(`float`)



- `min(최소값)`
- `max(최대값)`
- `sum(합계)`
- `mean(평균)`
- `var(분산)`
- `std(표준편차)`
- `count(개수)`
- `median(중앙값)`
- `mode(최빈값)`



## 결측값과 중복값 처리하기

- 결측값 채우기: `fillna()`, 옵션에 `inplace=True`를 넣어 줄 수 있다.
- 결측치 있는 행 제거: `dropna()` 
  - axis = 0 행 삭제
  - axis = 1 열 삭제
  - how="any": NaN값이 하나라도 있으면 제거
  - how="all": 모두 NaN이어야지 제거
- 중복치 제거: `drop_duplicates()`
  - keep="first" -> 첫 번째 데이터만 남기고 제거
  - keep="last" -> 마지막 데이터만 남기고 제거
- 행/열 삭제: `drop`
  - axis=0 행 삭제
  - axis=1 열 삭제



## 데이터 프레임 집계

### 피벗 테이블

- 데이터 프레임의 데이터를 인덱스로 사용하기 위한 테이블

```
import numpy as np
pd.pivot_table(
    df,
    index="그룹",
    columns="혈액형",
    values="브랜드평판지수",
    aggfunc=[np.sum, np.mean]
    )
```



### 그룹핑

- 데이터를 집계해서 가공하는데 목적이 있다.

```
df.groupby("혈액형")[["키"]].mean()
```



## 두 개의 데이터 프레임 합치기

### 행 기준

```
df1_concat = pd.concat([df1, df1_copy], axis=0)
df1_concat
df1_concat.reset_index(drop=True)
```



### 컬럼 기준

```
pd.concat([df1, df2], axis=1)
```



## 데이터 프레임 병합

- 특정한 고유키 값을 기준으로 병합

  

**`pd.merge(left, right, on='기준 컬럼', how='left')`**

 - how 옵션: 'left', 'right', 'inner', 'outer'

```
pd.merge(
    df1, df3,
    left_on='이름',
    right_on='성함'
)
```



## 타입 변환

- `dtypes` : 데이터 타입 확인
- `astype` : 데이터 타입 변환



### 날짜 변환

```
df['생년월일'] = pd.to_datetime(df['생년월일'])
df['생년월일']
```

```
# 요일 추출. 
df['생년월일'].dt.dayofweek
df['생년월일'].dt.year
df['생년월일'].dt.month
df['생년월일'].dt.day
df['생년월일'].dt.hour
```



## Apply 함수

Series, DataFrame에 **구체적인 로직**을 적용하고 싶을 때 활용

- 단순 집계 뿐이 아닌 각 데이터에 프로그래밍적인 효과

```
def male_or_female(x):

  return 1 if x == '남자' else 0


df['성별_NEW'] = df['성별'].apply(male_or_female)
df.head()
```

```
df['성별_lambda'] = df['성별'].apply(lambda x : 1 if x == '남자' else 0 )
```



