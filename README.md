# Gesture Recognition

## 介紹
一個使用python結合opencv及google mediapipe函式庫所做出來的手勢辨識專案

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c3/Python-logo-notext.svg/1200px-Python-logo-notext.svg.png" width="100" height="100"><img src="https://github.com/yichengtsai/opencv_gesture/blob/main/photo/opencv_image.png" width="130" height="130"><img src="https://miro.medium.com/v2/resize:fit:1400/0*uMb2M-O9fLtRKmOo.png" width="300" height="100">

## 版本

Python >= 3.8

## 安裝

```
python -m pip install --upgrade pip
pip install opencv-python
pip install mediapipe
```
## 程式碼運作過程介紹

#### 透過mediapipe函式庫繪製手指的骨架結構及關節點
```
mpDraw.draw_landmarks(img, handLms, mpHands.HAND_CONNECTIONS, handLmsStyle, handConStyle)
```

#### 將mediapipe偵測到的手部關節點轉換成實際像素座標
- 利用for迴圈依序將座標存入lm變數中
- 將lm變數中的x、y乘以圖像寬度及高度，轉換成實際像素座標
- 將每個座標存入finger_points變數中
```
for i, lm in enumerate(handLms.landmark):
  xPos = int(lm.x * imgWidth)
  yPos = int(lm.y * imgHeight)
  finger_points.append((xPos,yPos))
```

#### 計算手部關節之間的向量
- `hand_[0] = (x0, y0)`:第一個關節的座標
- `hand_[2] = (x2, y2)`:第二個關節的座標<br/>

公式:<br/>
<img src="https://github.com/yichengtsai/opencv_gesture/blob/main/photo/formula_1.png" width="240" height="50">
```
angle_ = vector_2d_angle(
  ((int(hand_[0][0])- int(hand_[2][0])),(int(hand_[0][1])-int(hand_[2][1]))),
  ((int(hand_[3][0])- int(hand_[4][0])),(int(hand_[3][1])- int(hand_[4][1])))
)
```

#### 計算向量之間的角度<br/>
公式:<br/>
<img src="https://github.com/yichengtsai/opencv_gesture/blob/main/photo/formula_2.png" width="200" height="80"/><br/>
```
angle_= math.degrees(math.acos((v1_x*v2_x+v1_y*v2_y)/(((v1_x**2+v1_y**2)**0.5)*((v2_x**2+v2_y**2)**0.5))))
```

#### 取得手指角度
```
f1 = finger_angle[0]   # 大拇指角度
f2 = finger_angle[1]   # 食指角度
f3 = finger_angle[2]   # 中指角度
f4 = finger_angle[3]   # 無名指角度
f5 = finger_angle[4]   # 小拇指角度
```

#### 判斷每根手指的彎曲角度是否大於特定值
```
if f1>=50 and f2>50 and f3>50 and f4>50 and f5>50:
    return '0'
elif f1>=50 and f2<50 and f3>=30 and f4>=30 and f5>=30:
    return '1'
elif f1>=50 and f2<50 and f3<50 and f4>=50 and f5>=50:
    return '2'
elif f1>=50 and f2<50 and f3<50 and f4<50 and f5>=50:
    return '3'
elif f1>=50 and f2<50 and f3<50 and f4<50 and f5<50:
    return '4'
elif f1<60 and f2<30 and f3<30 and f4<30 and f5<30:
    return '5'
elif f1<60 and f2>=50 and f3>=50 and f4>=50 and f5<50:
    return '6'
elif f1<60 and f2<50 and f3>=30 and f4>=30 and f5>=30:
    return '7'
else:
    return '8'
```







