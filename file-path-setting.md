# 파일 경로 설정

```
# !move ~/Dow 치고 Tab키 누르면 경로명 자동완성
# 제일 끝에 한 칸 띄우고 .을 꼭 적어주기
!move "C:\Users\home\Downloads\도로교통공단_사고유형별 교통사고 통계_20191231.csv" .

# 파일 확인하기
%ls

import pandas as pd

# ()안에서 Shift+tab 키를 누르면 도움말을 볼 수 있다.
# csv파일을 불러올 때 한글파일 인코딩을 하지 않으면 오류가 나기 때문에 encoding="cp949"를 함께 적기!
pd.read_csv("data\도로교통공단_사고유형별 교통사고 통계_20191231.csv", encoding = "cp949")
```

