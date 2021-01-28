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

