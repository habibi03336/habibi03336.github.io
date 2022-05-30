---
today: 😈 
title: 두 점을 잇는 한 직선과 quadratic curve의 접점 구하기

categories:
    - Computer graphics
tags:
    - TIL
    
---


__1. (x1, y1), (x2, y2)를 잇는 한 직선의 식을 구해보자.__ 

Parametric form (= Vector form):   
X = x1 + (x2 - x1)*t  
Y = y1 + (y2 - y1)*t
<!-- 
implicit form :  
(X - x1) / (x2 - x1) = (Y - y1) / (y2 - y1)  
=> (x2 - x1)*Y - (y2 - y1)*X + x1*y2 - x2*y1 = 0 -->

__2. parametric form을 quadratic curve의 식에 대입하자__  
y(t) = a * x(t)^2 + b * x(t) + c


__3. 만족하는 t가 있는지 확인해보자__  

t에 대해서 식을 정리했을 때, solution이 있는지 확인.

- t가 2차 식인 경우(D = b^2 - 4ac):  
    D > 0 : 만족하는 t 2개  
    D = 0 : 만족하는 t 1개  
    D < 0 : 만족하는 t 없음.  

__4. 만족하는 t를 직선의 식에 대입 하여 X, Y 구해주기__


__코드 구현__  
파라미터: 결과, 직선의 점1, 점2, quadratic function의 a, b, c
```c++
void getInterCoords(float *result, float *coord1, float *coord2, float a, float b, float c){
    float tA = a * ((coord1[0] - coord2[0]) * (coord1[0] - coord2[0])) + FLT_EPSILON;
    float tB = 2 * a * coord1[0] * (coord2[0] - coord1[0]) + b * (coord2[0] - coord1[0]) + coord1[1] - coord2[1];
    float tC = coord1[0] * (b + a * coord1[0]) + c - coord1[1];
    
    float det = tB*tB - 4*tA*tC;
    if (det >=0){
        float t_plus = (-tB + sqrt(det))/(2.f*tA);
        float t_minus = (-tB - sqrt(det))/(2.f*tA);

        result[0] = coord1[0] + (coord2[0] - coord1[0])*t_minus;
        result[1] = coord1[1] + (coord2[1] - coord1[1])*t_minus;
        result[2] = coord1[0] + (coord2[0] - coord1[0])*t_plus;
        result[3] = coord1[1] + (coord2[1] - coord1[1])*t_plus;
    }
}
```