# Camera Model Identification Using Convolutional Neural Networks



## Introduce

+ 10개의 카메라를 찾아내는 방식으로 접근한 딥러닝을 묘사

  





## Dataset

![image-20210727163356896](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210727163356896.png)

+ Dataset-1 과 Dataset-2을 합침
+ 100개의 이미지를 validataion set으로, 나머지를 training으로 



### Dataset-1

----

- Organizers of the IEEE's Signal Processing Camera identification Challenge
  - 10개의 카메라 모델과, 각 모델당 250개의 이미지 (총  2500개)
  - test set은 500x500으로 central crop됨
  - 이미지의 절반은 augmentaton됨 (resize, JPEG compression, Gamma,,)
  - 나머지 이미지의 절반은 원본 사진들 임



### Dataset-2

---

+ Flickr, Yandex.Fotki, Wikipedia Commons, mobile reviews websites에서 500GB이상의 이미지를 스크랩 (10개의 카메라 모델)

+ **Pre-processing**
  + EXIF metadata를 지움
  + 95 보다 적은 quality의 JPEG 압축 이미지는 제외시킨다.
  + 카메라가 생성할 수 있는 이미지 크기에 벗어날 경우 걸러낸다.
+ 총 78807개의 not-manipulated이미지



## Pre-processing

**training**

1. 훈련동안, 960x960 크기의 이미지 패치들을 크롭한다.
2. 변형한다.
   1. D4: 90, 180, 270도로 회전한다.
   2. Gamma: gamma parameter를 [0.8, 1.2]범위 안으로 적용한다.
   3. JPEG 압축: 70~90정도로 압축한다.
   4. Scale: [0.5, 2]범위안으로 scaling한다.
3. 480x480으로 크롭한다.



**testing**

480x480 corner에서 추론

이미지를 센터 크롭하고, D4 tansformaton을 함



## Method

1) end2end 방식으로 훈련된 네트워크를 평가
2) 161층의 레이어 구조를 사용한다.  --> low-lever feature들을 추출한다.
3) 이미 훈련된(ImageNet) 모델로 가중치를 초기화한다.
4) 공격적인 데이터 편집을 한다.



### CrossEntropy Loss

<img src="C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210727171532313.png" alt="image-20210727171532313" style="zoom:80%;" />

### DenseNet161

---

+ 전처리 단계에 중점을 두지 않고, 네트워크 구조 및 훈련 절차를 선택하는데중점을 둠
+ Parameters
  + epochs: 100k
  + optimizer: Adam optimizer
  + learning late(initial): 1e-3

![image-20210727181909605](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210727181909605.png)



## Result

+ score: 0.987976
+ compute weighted accuracy
  + ![image-20210727182013898](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210727182013898.png)

![image-20210727182150273](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210727182150273.png)

![image-20210727182205366](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210727182205366.png)

![image-20210727182255347](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210727182255347.png)

![image-20210727182310100](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210727182310100.png)

![image-20210727182322110](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210727182322110.png)

![image-20210727182330984](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210727182330984.png)

