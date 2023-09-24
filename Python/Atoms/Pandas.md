
## Series
- ==**1차원 배열 형태의 자료구조**==로, 인덱스(index)와 값(value)로 이루어져 있다.
- Series는 일련의 데이터를 ==**레이블링된 인덱스와 함께 저장하고 처리**==하는데 사용된다.
###  example
```python
s = pd.Series([1, 2, 3, 4])  
print(s)  
  
# 인덱스에 레이블링 하기  
s1 = pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd'])  
print(s1)  
  
# 슬라이싱  
s2 = pd.Series([99, 100, 98, 91, 92])  
s2_subset = s2[1:4]  
print(s2_subset)  
  
# 평균  
s2_mean = s2.mean()  
print(s2_mean)

""" 결과 """
# 1
0    1
1    2
2    3
3    4
dtype: int64

# 2
a    1
b    2
c    3
d    4
dtype: int64

# 3
1    100
2     98
3     91
dtype: int64

# 4
96.0
```

### 설명
- `# 1` 
	- 기본적으로 index 컬럼이 함께 생성되는 것을 알 수 있음.
- `# 2`
	- index에 지정한 값으로 레이블링.
	- 원소 개수하고 안맞으면 바로 에러남.
- `# 3`
	- Python의 기본적인 index slicing 방식도 지원한다.
- `# 4`
	- Series 전체 값들에 대한 평균값.

## DataFrame
- ==**2차원 테이블 형태의 자료구조**==로, 여러 개의 Series 객체가 모여서 열(column)과 행(row)으로 구성되어 있다.
- DataFrame은 엑셀 스프레드 시트나 SQL 테이블과 유사한 형태로, 다양한 유형의 데이터를 관리하고 조작하는데 사용된다.

### example
```python
def square(n) -> int:  
  
    return n * n  
  

df = pd.DataFrame(  
    [[4, 7, 10],  
     [5, 8, 11],  
     [6, 9, 12]],  
    index=[1, 2, 3],  
    columns=['a', 'b', 'c'])  
  
print(df)  
print(df.apply(square))  
print(df.apply(lambda x: x * x))  # lambda 스타일  
  
df2 = pd.melt(df)  
print(df2)  
  
df2 = (pd.melt(df)  
       .rename(columns={'variable': 'var', 'value': 'val'}))  
print(df2)  
  
df2 = (pd.melt(df)  
       .rename(columns={'variable': 'var', 'value': 'val'})  
       .query('val > 10'))  
print(df2)  
  
df2 = (pd.melt(df)  
       .rename(columns={'variable': 'var', 'value': 'val'})  
       .query('val > 10')  
       .sort_values('val', ascending=False))  
print(df2)  
  
# iloc : index를 기준으로 데이터 슬라이싱  
df3 = df.iloc[0:2]  
print(df3)  
  
df3 = df.loc[1:2]  
print(df3)  
  
# 이렇게 하면 0번, 2번 열에 대한 값만 가져온다.  
df3 = df.iloc[:, [0, 2]]  
print(df3)  
  
# DataFrame에 len()을 사용하면 행의 개수가 리턴됨.  
print("df의 행의 개수 : ", len(df))

""" 결과 """

# DataFrame 기본 구조
   a  b   c
1  4  7  10
2  5  8  11
3  6  9  12

# DataFrame 모든 원소들을 자신을 제곱한 수로 변경.
    a   b    c
1  16  49  100
2  25  64  121
3  36  81  144

# 같은건데, 코드 작성 방식을 람다(lambda) 방식으로 변경함.
    a   b    c
1  16  49  100
2  25  64  121
3  36  81  144

# melt() 사용 예제. 2차원을 1차원으로 변경해줌.
  variable  value
0        a      4
1        a      5
2        a      6
3        b      7
4        b      8
5        b      9
6        c     10
7        c     11
8        c     12

# melt()를 사용하면 default로 "variable", "value"로 컬럼 이름을 지어준다.
# 기본 이름을 "var", "val"로 수정한것임.
  var  val
0   a    4
1   a    5
2   a    6
3   b    7
4   b    8
5   b    9
6   c   10
7   c   11
8   c   12

# query()를 사용해서 val 값이 10 초과인 값만 추출.
  var  val
7   c   11
8   c   12

# sort_values()를 이용해서 내림차순 정렬
  var  val
8   c   12
7   c   11

# iloc : index를 기준으로 데이터 슬라이싱.
   a  b   c
1  4  7  10
2  5  8  11

# loc : index의 label값 기준으로 데이터를 슬라이싱한다. (사람이 이해하기는 좀 더 편한 방식)
   a  b   c
1  4  7  10
2  5  8  11

# df.iloc[:, [0, 2]] : [행, 열] 순서로 입력한다. 이렇게 입력하면, 
# 1. 행은 전체 (:)
# 2. 열은 0번, 2번 열만.
   a   c
1  4  10
2  5  11
3  6  12

# len(df)를 찍어보면, 행의 개수가 나오는 것이기 때문에 3이 출력된다.
df의 행의 개수 :  3
```


