---
today: ๐ 
title: ๋ ์ ์ ์๋ ํ ์ง์ ๊ณผ quadratic curve์ ์ ์  ๊ตฌํ๊ธฐ

categories:
    - Computer graphics
tags:
    - TIL
    
---


__1. (x1, y1), (x2, y2)๋ฅผ ์๋ ํ ์ง์ ์ ์์ ๊ตฌํด๋ณด์.__ 

Parametric form (= Vector form):   
X = x1 + (x2 - x1)*t  
Y = y1 + (y2 - y1)*t
<!-- 
implicit form :  
(X - x1) / (x2 - x1) = (Y - y1) / (y2 - y1)  
=> (x2 - x1)*Y - (y2 - y1)*X + x1*y2 - x2*y1 = 0 -->

__2. parametric form์ quadratic curve์ ์์ ๋์ํ์__  
y(t) = a * x(t)^2 + b * x(t) + c


__3. ๋ง์กฑํ๋ t๊ฐ ์๋์ง ํ์ธํด๋ณด์__  

t์ ๋ํด์ ์์ ์ ๋ฆฌํ์ ๋, solution์ด ์๋์ง ํ์ธ.

- t๊ฐ 2์ฐจ ์์ธ ๊ฒฝ์ฐ(D = b^2 - 4ac):  
    D > 0 : ๋ง์กฑํ๋ t 2๊ฐ  
    D = 0 : ๋ง์กฑํ๋ t 1๊ฐ  
    D < 0 : ๋ง์กฑํ๋ t ์์.  

__4. ๋ง์กฑํ๋ t๋ฅผ ์ง์ ์ ์์ ๋์ ํ์ฌ X, Y ๊ตฌํด์ฃผ๊ธฐ__


__์ฝ๋ ๊ตฌํ__  
ํ๋ผ๋ฏธํฐ: ๊ฒฐ๊ณผ, ์ง์ ์ ์ 1, ์ 2, quadratic function์ a, b, c
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