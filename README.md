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
## 程式運作流程
計算手部關節之間的向量
- `hand_[0] = (x0, y0)`:：第一個關節的座標
- `hand_[2] = (x2, y2)`:：第二個關節的座標
```
angle_ = vector_2d_angle(
  ((int(hand_[0][0])- int(hand_[2][0])),(int(hand_[0][1])-int(hand_[2][1]))),
  ((int(hand_[3][0])- int(hand_[4][0])),(int(hand_[3][1])- int(hand_[4][1])))
)
```
計算向量之間的角度<br/>
公式:<br/>
<img src="https://github.com/yichengtsai/opencv_gesture/blob/main/photo/%E5%85%AC%E5%BC%8F.png" width="250" height="100"/><br/>
```
angle_= math.degrees(math.acos((v1_x*v2_x+v1_y*v2_y)/(((v1_x**2+v1_y**2)**0.5)*((v2_x**2+v2_y**2)**0.5))))
```









