---
layout: single
title:  "Pendulum-korean ver"
---

# vpython을 이용한 이중진자 시뮬레이션

## 필요한 모듈들 불러오기

```python
import vpython
import math 
```
## 장면 세팅
##### 세팅을 바꾸셔도 상관 없습니다 :)

```python
scene.center = vector(0,-1,0)
scene.width = 400
scene.height = 700
scene.background=color.white
```

## 시뮬레이션에 필요한 물체들 만들기
##### 공 두개와 공들을 잇는 실린더가 있습니다
```python
ball = sphere(pos=vector(0,0,0), radius=0.15, color=color.blue, \
  opacity=0.8)
base = box(pos=vector(0,-2.5,-1), size=vector(2,0.1,2))
wall = box(pos=vector(0,-1,-1), size=vector(0.1,3,0.1))
bar = cylinder(pos=vector(0,0,-1), radius=0.05,\
  axis=vector(0,0,1), color=color.yellow)
line = curve(pos=bar.pos, color=color.gray(0.5))
line.append(ball.pos)
ball1 = sphere(pos=vector(0,-1,0),\
  radius=0.15, color=color.blue, opacity=0.8)
line1 = curve(pos=bar.pos, color=color.gray(0.5))
line1.append(ball.pos)
attach_trail(ball1, color=vec(0,1,1)) #Attaching a trail
```

## 기본값
##### alpha 와 alpha1 는 각각 각 theta1 theta2의 각가속도 입니다. omega 와 omega1 는 각속도 입니다.
```python
r = 1.0
g = 9.81  

theta0 = 90   
theta1 = theta0*pi / 180   
theta2=  theta0*pi / 180  

m=1.0

alpha=0.0
omega=0.0

alpha1=0.0
omega1=0.0

t = 0.0
dt=0.0001
```
## 시뮬레이션
```python
while True:
    while t<3*k:
        rate(10000)
    

        alpha = (-g*(2*m+m)*sin(theta1)-m*g*sin(theta1-2*theta2)-\
          2*sin(theta1-theta2)*m*(r*omega1**2+r*cos(theta1-theta2)\
           *omega**2))\
          /(r*(2*m+m-m*cos(2*theta1-2*theta2))) #ball 1의 각가속도
        omega += alpha*dt #ball 1 의 각속도
        theta1 += omega*dt # ball 1의 각

        alpha1 = (2*sin(theta1-theta2)*((m+m)*r*omega**2+g*(m+m)*\
          cos(theta1)+r*m*cos(theta1-theta2)*omega1**2))/(r*(2*m+m\
          -m*cos(2*theta1-2*theta2))) #ball 2의 각가속도
        omega1 += alpha1*dt #ball 2 의 각속도
        theta2 += omega1*dt #ball 2의 각
        #I used the euler's method
    
        line.modify(0, bar.pos + vector(0,0,1))
        line.modify(1, ball.pos)
        ball.pos = vector(r*sin(theta1), -r*cos(theta1), 0)

        line1.modify(0, ball.pos )
        line1.modify(1, ball1.pos)
        ball1.pos = vector(r*sin(theta1)+r*sin(theta2), \
          -r*cos(theta1)-r*cos(theta2), 0)
        
        t += dt
    attach_trail(ball1, color=vec(0,random.random(), \
      random.random()))
    k+=1
```

[완성](https://glowscript.org/#/user/king.jihu/folder/진자/program/이중진자완성)
