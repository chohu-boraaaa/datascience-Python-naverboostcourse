# 공공데이터 상권정보 분석하기
* data : https://www.data.go.kr/dataset/15012005/fileData.do
* 국가중점데이터인 상권정보 살펴보기
* 상가(상권)정보_의료기관_201909 

## 필요한 라이브러리 불러오기
* Python Data Analysis Library - pandas : Python Data Analysis Library
* NumPy - NumPy
* seaborn: statistical data visualization - seaborn 0.9.0 document

```
import pandas as pd
import numpy as np
import seaborn as sns
```

## 시각화를 위한 폰트 설정

```
# ctrl(cmd) + /
import matplotlib.pyplot as plt
# Window 한글 폰트 설정
plt.rc('font', family = 'Malgun Gothic')
# Mac의 한글 폰트 설정
# plt.rc('font', family = 'AppleGothic')
plt.rc('axes', unicode_minus=False)

# 그래프가 노트북 안에 보이게 하기 위해
%matplotlib inline

from IPython.display import set_matplotlib_formats
# 폰트가 선명하게 보이기 위해
set_matplotlib_formats('retina')
```

## 데이터 로드하기
* 판다스에서 데이터 로드할 때는 read_csv 사용
* 데이터를 로드해서 df라는 변수에 담는다.
* 그리고 shape를 통해 데이터의 갯수를 찍는다. 결과는 행, 열 순

```
!move "C:\Users\home\Downloads\소상공인시장진흥공단_상가업소정보_의료기관_201909.csv" .

# read_csv로 불러온 파일을 df라는 변수에 담기
df = pd.read_csv("data\소상공인시장진흥공단_상가업소정보_의료기관_201909.csv", low_memory = False)
df.shape
```

## 데이터 미리보기
* head, tail을 통해 데이터를 미리 볼 수 있다.

```
# Shift + tab 키를 누르면 docstring을 볼 수 있다.
# head로 데이터 미리보기
# 인덱스 0부터
df.head(6)

# tail로 마지막 부분에 있는 데이터 불러오기
df.tail(1)

# sample로 미리보기
df.sample()
```

## 데이터 요약하기

### 요약정보

```
# info로 데이터의 요약 보기
# int 정수형 float 실수형 object 문자형
# 컬럼 별로 숫자가 다른 것은 결측치가 있다는 것
df.info()# info로 데이터의 요약 보기
```

### 컬럼명 보기

```
# 컬럼명만 출력
df.columns
```

### 데이터 타입

```
# 데이터 타입만 출력
df.dtypes
```

## 결측치

```
# True가 결측치
df.isnull().sum()

# 각 컬럼별 결측치 개수
null_count = df.isnull().sum()
null_count

# 위에서 구한 결측치를 .plot.bar를 통해 막대그래프로 표현
# barh로 하면 그래프 수평적으로
null_count.plot.barh(figsize=(5, 7))

# 위에서 계산한 결측치 수를 reset_index를 통해 데이터 프레임으로 만들기
# df_null_count 변수에 결과를 담아 head로 미리보기
df_null_count = null_count.reset_index()
df_null_count.head()
```

## 컬럼명 변경하기

```
# df_null_count 변수에 담겨있는 컬럼의 이름을 "컬럼명", "결측치수"로 변경
df_null_count.columns = ["컬럼명", "결측치수"]
df_null_count.head()
```

## 정렬하기

```
# df_null_count 데이터프레임에 있는 결측치수 컬럼을 sort_values를 통해 정렬
# 결측치가 많은 순으로 상위 10개만 출력
df_null_count.sort_values(by = "결측치수", ascending = False)
df_null_count_top = df_null_count.sort_values(by = "결측치수", ascending = False).head(10)
```

## 특정 컬럼만 불러오기

```
# 지점명 컬럼을 불러오기
# NaN == Not a Number의 약자로 결측치 의미
df["지점명"].head()

# "컬럼명"이라는 컬럼의 값만 가져와서 drop_columns라는 변수에 담기
drop_columns = df_null_count_top["컬럼명"].tolist()
drop_columns

# drop_columns 변수로 해당 컬럼 정보만 데이터프레임에서 가져오기
df[drop_columns].head()
```

## 제거하기

```
# 행 기준으로 drop을 해줘야 하므로 axis = 1 지정
print(df.shape)
df = df.drop(drop_columns, axis = 1)
print(df.shape)

# 제거 결과를 info로 확인
df.info()
```

## 기초 통계값 보기

### 기초 통계 수치

```
# 평균값
df["위도"].mean()

# 중앙값
df["위도"].median()

# 최댓값
df["위도"].max()

# 최솟값
df["위도"].min()

# 갯수
df["위도"].count()
```

### 기초통계값 요약 - describe

describe를 사용하면 데이터 요약 가능

기본적으로 수치형 데이터 요약해서 보여줌

데이터 갯수, 평균, 표준편차, 최솟값, 1사분위수(25%), 2사분위수(50%), 3사분위수(75%), 최댓값 볼 수 있음

```
# 위도를 describe로 요약
df["위도"].describe()

# 2개의 컬럼을 desribe로 요약
df[["위도", "경도"]].describe()

# describe로 문자열 데이터타입의 요약 보기
# top : 가장 많이 등장한 object
# include = "number"로 하면 숫자형 데이터
df.describe(include="object")

# include = "all"로 하면 숫자형 데이터 + 문자형데이터
df.describe(include = "all")
```

### 중복제거한 값 보기
* unique로 중복을 제거한 값을 보고 nunique로 갯수를 세어보기

```
# "상권업종대분류명"
df["상권업종대분류명"].unique()

# 의료라는 데이터 한번 나옴
df["상권업종대분류명"].nunique()

# "상권업종중분류명"
df["상권업종중분류명"].unique()

df["상권업종중분류명"].nunique()

# "상권업종소분류명"
df["상권업종소분류명"].unique()

df["상권업종소분류명"].nunique()

# unique대신 len을 사용할 수도 있음
len(df["상권업종소분류명"].unique())
```

### 그룹화된 요약값 보기 - value_counts
* value_counts를 사용하면 카테고리 형태의 데이터 갯수를 세어볼 수 있다.

```
# 시도코드 세어보기
df["시도명"].head()

# 시도명 세어보기
city = df["시도명"].value_counts()
city

# normalize=True 옵션을 사용하면 비율을 구할 수 있다.
city_normalize = df["시도명"].value_counts(normalize = True)
city_normalize

# Pandas에는 plot기능을 내장하고 있다.
# 위에서 분석한 시도
city.plot.barh()

# 판다스의 plot.pie()를 사용해서 파이그래프 그리기
city.plot.pie(figsize=(7,7))

# seaborn의 countplot으로 그려보기
c = sns.countplot(data=df, y="시도명")

# "상권업종대분류명"으로 갯수 세어보기
df["상권업종대분류명"].value_counts()

# "상권업종중분류명"으로 갯수 세어보기
d = df["상권업종중분류명"].value_counts()
d

# normalize=True를 사용해 비율 구하기
n = df["상권업종중분류명"].value_counts(normalize=True)
n

# 판다스의 plot.bar() 사용해서 막대그래프 그려보기
# rot=0하면 글씨가 똑바로 보임
d.plot.bar(rot=0)

# 판다스의 plot.pie() 사용해서 파이그래프를 그려보기
n.plot.pie()

# "상권업종소분류명"에 대한 그룹화된 값을 카운트
e = df["상권업종소분류명"].value_counts()
e

# "상권업종소분류명"으로 갯수 세어보기
# 판다스의 plot.bar()를 사용해서 막대그래프 그리기
e.plot.barh(figsize=(7,8), grid=True)
```

## 데이터 색인하기
* 특정 데이터만 모아서 따로 보기

```
# "상권업종중분류명"이 "약국/한약방"인 데이터만 가져와서
# df_medical 이라는 변수에 담기
# head()를 통해 미리보기 하기
# .copy를 하면 df_medical 바뀌어도 df(원본)에는 영향 X
df_medical = df[df["상권업종중분류명"] == "약국/한약방"].copy()
df_medical.head(3)

# "상권업종대분류명"에서 "의료"만 가져오기
# df.loc를 사용하면 행,열을 함께 가져올 수 있음
# 이 기능을 통해 " 상권업종중분류명"만 가져오기
# 가져온 결과를 value_counts를 통해 중분류의 갯수 세어보기
df[df["상권업종대분류명"] == "의료"]["상권업종중분류명"]

m = df["상권업종대분류명"] == "의료"
df.loc[m, "상권업종중분류명"].value_counts()

# 위와 똑같은 기능을 수행하는 코드, 아래와 같이 한 줄에 표현할 수 있음
# df.loc[df["상권업종대분류명"] == "의료", "상권업종중분류명"].value_counts()
df.loc[df["상권업종대분류명"] == "의료", "상권업종중분류명"].value_counts()

# 유사의료업만 따로 모아보기
df_medi = df[df["상권업종중분류명"] == "유사의료업"]
df_medi

df[df["상권업종중분류명"] == "유사의료업"].shape

# 상호명을 그룹화해서 갯수 세어보기
# value_counts를 사용해서 상위 10개 출력
df["상호명"].value_counts().head(10)

# 유사의료업만 df_medi 변수에 담기
# df_medi 변수에서 상호명으로 갯수 세어보기
# 가장 많은 상호 상위 10개 출력
df_medi["상호명"].value_counts().head(10)
```

### 여러 조건으로 색인하기

```
# "상권업종소분류명"이 "약국"인 것과
# "시도명"이 "서울특별시"인 데이터 가져오기
df_seoul_drug = df[(df["상권업종소분류명"] == "약국") & (df["시도명"] == "서울특별시")]
print(df_seoul_drug.shape)
df_seoul_drug.head(3)
```

### 구별로 보기

```
# 위에서 색인한 데이터로 "시군구명"으로 그룹화해서 갯수 세어보기
# 구별로 약국이 몇개가 있는지 확인해 보기
c = df_seoul_drug["시군구명"].value_counts()
c.head()

# normalize = True를 통해 비율 구하기
n = df_seoul_drug["시군구명"].value_counts(normalize=True)
n.head()

# 위에서 구한 결과를 판다스의 plot.bar()를 활용해 막대그래프로 그리기
c.plot.bar(rot=60)

# "상권업종소분류명"이 "종합병원"인 것과
# "시도명"이 "서울특별시"인 데이터만 가져오기
# 결과를 df_seoul_hospital 에 할당해서 재사용
df_seoul_hospital = df[(df["상권업종소분류명"] == "종합병원") & (df["시도명"] == "서울특별시")].copy()
df_seoul_hospital

# "시군구명"으로 그룹화해서 구별로 종합병원의 수 세어보기
df_seoul_hospital["시군구명"].value_counts()
```

### 텍스트 데이터 색인하기

```
# 색인하기 전에 상호명 중에 종합병원이 아닌 데이터 찾기
df_seoul_hospital.loc[~df_seoul_hospital["상호명"].str.contains("종합병원"), "상호명"].unique()

# 상호명에서 특정 단어가 들어가는 데이터만 가져오기 - 꽃배달
df_seoul_hospital[df_seoul_hospital["상호명"].str.contains("꽃배달")]

# 특정 단어가 들어가는 데이터만 가져오기 - 의료기
df_seoul_hospital[df_seoul_hospital["상호명"].str.contains("의료기")]

# "꽃배달|의료기|장례식장|상담소|어린이집"은 종합병원과 무관하기 때문에
# 전처리를 위해 해당 텍스트를 한 번에 검색
# 제거할 데이터의 인덱스만 drop_row에 담고 list 형태로 변환
drop_row = df_seoul_hospital[df_seoul_hospital["상호명"].str.contains("꽃배달|의료기|장례식장|상담소|어린이집")].index
drop_row = drop_row.tolist()
drop_row

# 의원으로 끝나는 데이터도 종합병원으로 볼 수 없기 때문에 인덱스 찾아서
# drop_row2에 담아주고 list 형태로 변환
drop_row2 = df_seoul_hospital[df_seoul_hospital["상호명"].str.endswith("의원")].index
drop_row2 = drop_row2.tolist()
drop_row2

# 삭제할 행을 drop_row에 넣어주기
drop_row = drop_row + drop_row2
len(drop_row)

# 해당 셀을 삭제하고 삭제 전과 후의 행의 갯수 비교
# axis = 0 : 행 기준
print(df_seoul_hospital.shape)
df_seoul_hospital = df_seoul_hospital.drop(drop_row, axis=0)
print(df_seoul_hospital.shape)

# 시군구명에 따라 종합병원의 숫자를 countplot
df_seoul_hospital["시군구명"].value_counts().plot.bar()

plt.figure(figsize=(15, 4))
sns.countplot(data=df_seoul_hospital, x="시군구명", order=df_seoul_hospital["시군구명"].value_counts().index)

df_seoul_hospital["상호명"].unique()
```
