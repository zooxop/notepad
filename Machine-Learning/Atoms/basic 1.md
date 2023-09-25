
## 예시

- 예) 돈이 사람들을 행복하게 만드는가? 에 대한 일부 국가별 GDP & 삶의 만족도 도표
- 데이터의 trend가 보이는 도표.

```python
import matplotlib.pyplot as plt  
import numpy as np  
import pandas as pd  
from sklearn.linear_model import LinearRegression  
  
# 데이터셋을 다운로드한다.  
data_root = "https://github.com/ageron/data/raw/main/"  
lifesat = pd.read_csv(data_root + "lifesat/lifesat.csv")  
X = lifesat[["GDP per capita (USD)"]].values  
y = lifesat[["Life satisfaction"]].values  
  
# Visualize the data  
lifesat.plot(kind='scatter', grid=True, x="GDP per capita (USD)", y="Life satisfaction")  
plt.axis([23_500, 62_500, 4, 9])  
plt.show()  
  
# 선형회귀 모델 생성  
model = LinearRegression()  
  
# 모델을 훈련시킨다.  
model.fit(X, y)  
  
X_new = [[37_655.2]]  
print(model.predict(X_new))
```


- `LinearRegresssion()`을 통해서 생성된 선형 모델 `model`의 `.fit`에 대해
	- X와 y는 모두 2차원 배열을 넣어준다.
	- 