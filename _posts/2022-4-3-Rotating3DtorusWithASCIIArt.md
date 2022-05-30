---

title: 화면에 3D rotating torus를 ASCII 코드로 표현하기
date: 2022-4-3
categories:
    - Computer graphics
tags:
    - 특강

---

__Step1. 무엇이 필요한가?__  
일반적으로 화면에 무엇인가를 그리기 위해서는 각 픽셀에 어떤 색을 채워넣을 것인지를 결정해야한다. 그러나 ASCII art는 픽셀 단위로 그림을 그리는 것이 아니라, character 단위로 그림을 그린다. 따라서 ASCII를 통해서 무엇인가를 그릴 때는 각 character에 어떤 것을 넣을지를 정해주면된다. 보통 ASCII art에서는 다음과 같은 character를 이용한다. 

>  . , - ~ : ; = ! * # $ @   

오른쪽으로 갈 수록 더 뺵빽해진다. 검은 바탕에 흰색으로 쓰인는 것이라고 했을 때, 밝은 부분일 수록 더 빽뺵한 글자를 사용하면 된다. 결국 알아내야 할 것은 3D torus처럼 보이게 하기 위해서 각 자리에 위 12개의 ASCII 코드 중 어떤 것을 넣어야 할지이다.

가만히 있는 torus를 표현하고 싶다면, 프로그래밍 없이 적당히 알맞는 것을 찾아 일일이 넣어주는 것으로 목표를 달성할 수도 있다. 하지만 지금 하고자 하는 것은 3D로 rotating하는 torus를 표현하는 것이다. 이를 일일이 작업을 한다는 것은 매우 수고스러운 일이다. 프로그래밍을 통해서 이런 수고를 획기적으로 줄일 수 있지 않을까? 

__Step2. 모델링__  
torus는 수학적으로 표현하기가 아주 좋다. 3차원 좌표에서, x축과 y축을 basis로 가지는 평면의 원을 y축을 기준으로 360도 회전시키면 torus이다. (우리가 일반적으로 아는 도넛 모양이다🤤) 원의 parametric equation에 y축에 대해서 rotation하는 행렬을 곱해주면 torus의 식을 얻을 수 있다. 그 결과는 다음과 같다.
 
> x =  r1 * cos(⏀) + r2 * cos(θ) * cos(⏀)   
y = r2 * sin(θ)  
z = r1 * sin(⏀) + r2 * cos(θ) * sin(⏀)    
  (0 <= ⏀, θ < 2𝝅 )

r1: 원점에서 기준이는 원의 중점까지의 거리  
r2: 원의 반지름  
θ: 원의 parameter  
⏀: y축 rotation의 parameter

기본적인 torus의 모델은 구했다. 이제 torus를 x축으로 A만큼, z축으로 B만큼 rotate시킨 후의 x, y, z 좌표를 구하면 다음과 같다.
> x =  (r1 + r2 * cos(θ)) * (cos(B) * cos(⏀) + sin(A) * sin(B) * sin(⏀)) - sin(B) * (cos(A) * r2 * sin(θ))  
y =  (r1 + r2 * cos(θ))(sin(B) * cos(⏀) - cos(B) * sin(A) * sin(⏀)) + cos(B) * cos(A) * r2 * sin(θ)  
z = cos(A) * sin(⏀) * (r1 + r2 * cos(θ)) + r2 * sin(θ) * sin(A)  
(0 <= ⏀, θ < 2𝝅 )

r1: 원점에서 기준이는 원의 중점까지의 거리  
r2: 원의 반지름  
θ: 원의 parameter  
⏀: y축 rotation의 parameter
A: x 축 rotate 각도
B: z축 rotate 각도


__Step3. 휘도(Luminance)의 계산__  
물체가 3D로 보인다는 것은 무슨 의미일까? 그건 음영이 표현되어있다는 의미이다. 또 원근법에 따라 보인다는 의미이다. 이 문제에서는 음영만 고려하여 3D를 표현하려고 한다. 프로그래밍적으로 어떻게 음영을 표현할 수 있을까?

우선, 빛이 일정한 방향으로 torus를 향해 들어온다고 가정하자. 그리고 이를 벡터 L이라고 하자. torus의 한 점에서 Normal벡터를 구하고, 벡터 L과 Normal 벡터의 내적이 0보다 크다면, 즉 두 벡터가 이루는 각이 90도 보다 작다면 빛의 방향과 표면의 방향이 같은 쪽을 향하고 있다고 말할 수 있다. 반대로 내적이 0보다 작다면 빛의 방향과 표면의 방향이 서로 마주보고 있다고 말 할 수 있다. 즉, 내적이 0보다 크다면 빛을 받지 않는다는 의미이고, 0보다 작으면 빛을 받는다는 의미이다.

위 설명에서는 빛의 방향이 L이라고 했지만, 관습적으로 프로그래밍에서는 빛의 정반대 방향을 L이라고 한다. 이렇게 두면 내적이 양수일 때 빛을 받는 것, 음수일 때 빛을 받지 않는 것이 된다. 이렇게 생각하는 것이 더 편하기 때문에 이제 이 관습을 따르겠다.

벡터 L은 광원의 방향을 어떻게 설정하느냐에 따라 달라지는 것이다. 그렇기 때문에 문제가 될 것이 없다. 진짜로 알아내야 하는 것은 normal 벡터이다. 가장 먼저 생각할 수 있는 방법은 식의 미분을 통해 바로 normal을 구하는 방법이다. torus위의 두 벡터를 구해서 outer product를 하는 방식으로 normal을 구할수 있을 것이다.

하지만 보다 효율적인 방법이 있다. normal vector는 상대적인 값이다. 따라서 원의 노말부터 시작하여 rotate해주는 만큼 normal vector도 같이 rotate를 해준 것으로 해석하면, 아주 간단하게 normal을 구할 수 있다. 원의 normal은 (cos(θ), sin(θ), 0)이고, 이를 시작으로 y축, x축, z축의 각각 ⏀, A, B만큼의 rotate를 반영해주면 normal을 바로 얻을 수 있다.

normal과 L을 내적해주면 방향이 같으면 양수, 다르면 음수가 된다. 또한 방향이 같을 수록 값이 커지고, 다를 수록 더 작은 음수가 된다. 
L이 (0, 1/sqrt(2), -1/(sqrt(2))라고 하면 L과 normal의 내적은 -1과 1 사이의 값을 가지게 된다.  

__Step4. 마무리__  
x, y, z의 각 좌표를 얻었고, 각 좌표에 대한 휘도 값을 구할 수 있게 되었다. 이제 이를 잘 조합하면 어떤 ASCII코드를 나타내야 하는지 알 수 있다. z축과 수직으로 x,y 평면을 바라본다고 하면, 카메라에서 가장 가까운 z의 값을 가진 점만 보이게 된다. 따라서 같은 x,y 좌표에 대해서 가장 가까운 z값을 가지는 좌표를 찾은 후, 그 좌표의 휘도에 따라 적절한 ASCII코드를 넣어주면 목표했던 rotating하는 3d torus를 표현할 수 있다. 



__Step5. 구현__
```c++

#include <stdc++.h>
#include <unistd.h>

int main() {
    float A = 0;
    float B = 0;
    float theta, pi;
    int r1 = 10;
    int r2 = 5;
    int k;
    float z[1760];
    char chars[1760];
    float L[] = {0.f, 1.f, -1.f};
    printf("\x1b[2J");
    for(;;)
    {
        memset(chars, 32, 1760);
        memset(z, 0, 4 * 1760);
        for(int i = 0; i< 1761; i++){
            z[i] = 64.f;
        }
        for(pi = 0; pi < 6.24; pi += 0.03)
        {
            for(theta = 0; theta < 6.24; theta += 0.03)
            {
                float sinA = sin(A);
                float sinB = sin(B);
                float sinPi = sin(pi);
                float sinTheta = sin(theta);
                float cosA = cos(A);
                float cosB = cos(B);
                float cosTheta = cos(theta);
                float cosPi = cos(pi);
                
                float subCal1 = r1 + r2 * cosTheta;
                float subCal2 = r2 * sinTheta;
                float subCal3 = sinPi * subCal1 + subCal2;
                float subCal4 = cosA * subCal2 - sinA * subCal3;
                
                int x = 40 + 0.7 * (cosB * cosPi * subCal1 - sinB * subCal4);
                // 좌표상 y의 방향과 화면상 y의 방향이 반대이기 때문에 -1을 곱해주어야 한다.
                int y = 12 - 0.7 * (sinB * cosPi * subCal1 + cosB * subCal4);
                float d = sinA * subCal2 + cosA * subCal3;
                
                int coord = y * 80 + x;
                if (x > 0 && 80 > x && y > 0 && 22 > y && d < z[coord])
                {
                    float normal[3];
                    float norSubCal1 = cosTheta * cosPi;
                    float norSubCal2 = sinTheta * cosA - sinA * cosTheta * sinPi;
                    normal[0] = cosB * norSubCal1 - sinB * norSubCal2;
                    normal[1] = sinB * norSubCal1 + cosB * norSubCal2;
                    normal[2] = sinA * sinTheta + (cosA * sinPi * cosTheta);
                    int luminance = (L[1] * normal[1] + L[2] * normal[2]) * 8;
                    
                    chars[coord] = ".,-~:;=!*#$@"[luminance > 0 ? luminance : 0];
                    z[coord] = d;
                }
            }
        }
        printf("\x1b[H");
        for(k = 0; k < 1761; k++)
        {
            putchar(k % 80 ? chars[k] : 10);
        }
        A += 0.03;
        B += 0.02;
        usleep(30000);
    }
    return 0;
}
```
y축의 좌표와, 화면 상의 좌표가 반대가 되야한다는 점을 알지 못해서 삽질을 했다.(y축이 클수록 먼저 출력이 되어야함) 이곳 저곳 살펴보다가 겨우 찾았다. 실제 구현을 하다보면 생각 외의 난관을 만나는 경우가 많은 것 같다. 코드를 좀 더 다듬을 수 있을 것 같긴 한데 이번 거는 이정도에서 만족하려고 한다. 수고했다!







* 이 글은 연세대학교 프로그래밍 동아리 PoolC의 C++과 함께하는 즐거운 3D 세미나(by 정기웅님)를 바탕으로 작성되었습니다.
