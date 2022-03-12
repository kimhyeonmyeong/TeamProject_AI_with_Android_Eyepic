## TeamProject_AI_with_Andorid_Eyepic_(Computer Vision)

사용자별로 선호될 수 있는 의류 이미지들에 대해 추천해주는 시스템을 구축하고자 했습니다. (Computer Vision, Data Science)

팀원: 김현명, 김시은, 한도훈  

지도교수님: 박소영 교수

## 프로젝트 개요

AI기반의 이미지 검색을 통해서 사용자가 업로드한 의류 이미지와 유사한 의류 이미지를 검색 및 추천해주는 시스템을 구축하고자 하였습니다.

위검색 시스템은 의류 이미지의 형태 및 속성에 집중한 스타일기반의 검색 시스템으로 사용자에게 편리하고 다양한 온라인 쇼핑 경험을 제공하는 것을 목표로 하고 있습니다.

 이미지 검색시 사용자가 원하는 객체를 인식하는 객체 인식모델과 해당 객체의 이미지 특징들을 추출하는 CNN모델을 Custom 만들고 개선시켜서 사용하기 위해 여러 논문들을 참고했고 이를 기반으로 여러 핵심 전략들을 세웠습니다.  
 최종적으로 Crawling 기반의 의류이미지 데이터 수집, 객체탐지 모델, 전이학습기반의 의류 특징 추출 모델까지 진행했습니다.


## 진행 기간
2020-03-02 ~ 2020-08-01


## 프로젝트 구성

 Eyepic의 프로젝트는 의류 이미지 Data 수집  – 객체 탐지 모델 – 전이 학습 모델로 구성됩니다.


## 의류 분류 기준 정립

![image](https://user-images.githubusercontent.com/44837403/136703324-4a1e7f2a-392d-4b9a-8744-7ea1f506fedf.png)

의류 분류체계를 상위 Level과 하위 Level로 분류하여 구성하였습니다. 이미지 분류를 CNN모델 하나로만 분류할 경우, 분류 기준도 많아지고 그에 따라 학습 Feature Map이 신뢰성있게 학습하기 힘들어집니다.  

 그래서 현도시 팀에서는 전체 시스템에서 의류 분류 기준을 상위 Level과 하위 Level로 그룹화하고 상위 Level에서 먼저 분류 및 객체 검출 모델을 돌리고 그 외 하위 Level은 이미지 분류 추출 모델에서 분류기준으로 삼아 사용하기로 하였습니다.
 

### 의류 데이터 수집 (for 객체 탐지 모델)

위와 같은 분류체계에 맞게 AI모델을 학습하기 위해서 custom한 특성에 맞추어서 데이터를 수집해야 합니다.  
고로, 셀레니움을 활용한 Data Crawling 방식(한도훈 팀원)을 통해서 직접 데이터를 수집하였습니다.

![image](https://user-images.githubusercontent.com/44837403/136703812-bf5d9d26-7124-4495-a8ea-6ee9f47710f4.png)
(형태)

![image](https://user-images.githubusercontent.com/44837403/136703817-d8d50beb-b054-4c8a-9bbd-986fd2751a26.png)
(색상)

![image](https://user-images.githubusercontent.com/44837403/136703898-544c34af-6097-430b-92ee-60277cbb6a42.png)

위 같은 기준으로 각 분류기준당 100개, 총 4000개의 데이터를 수집하였습니다.

## 객체 탐지 모델 개발

이미지들이 입력될 때, 불필요한 배경화면을 줄이고 사용자가 원하는 item만을 검출하기 위한 모델입니다.

![image](https://user-images.githubusercontent.com/44837403/136703946-abee5952-6fb1-41ae-958d-cf11d75a8860.png)

위와 같이 검출이 됩니다.  

## 전이 학습 모델 개발

### 네트워크 모델 설정

이미지 분류를 위한 모델은 여러 모델이 있습니다.  
초창기에는 AlexNet이 만들어졌고 그 뒤를 이어 VGG 모델, Google Model 등등 여러 모델들이 나왔습니다. 이와 관련해서는 이병우 외 저자의 논문(1)의 AlexNet과 VGG모델의 정확도 차이에 대한 연구에서도 확인할 수 있습니다.  
현도시팀에서는 정확도와 계산 성능에서 우수한 모델을 고르고자 했고 이 중 지명근 외 저자의 논문(2)에서 확인할 수 있는 Resnet 모델을 사용하고자 하였습니다. 그 이유는 ResNet이 이미지 분류 모델 중 사람의 능력으로 분류하는 인식 오류보다 낮아지는 모델 중 하나이기 때문입니다.  
또한 ResNet, GoogleLeNet, SENet을 학습시켜봤을 때 오차율 및 계산시간을 테스트한 경우에서 ResNet이 우수한 case를 참고한것에서 볼 수 있습니다. (블로그 참조)

![image](https://user-images.githubusercontent.com/44837403/136704054-3d742c19-42d6-45ee-bcc2-2e88f368789f.png)
ILSVRC (이미지 분류 경진대회) 우승 모델 년도별 에러율
![image](https://user-images.githubusercontent.com/44837403/136704067-b18060fb-0b3f-4173-b242-ac17b62545e6.png)
GoogLeNet, ResNet, SENet 성능 비교

논문 및 문헌 참고  
딥러닝 기반 이미지 특징 추출 모델을 이용한 유사 디자인 검출에 대한 연구(1) _(논문)  
딥 러닝을 이용한 영상 인식 기술 동향(2) _(논문)  
https://blog.nerdfactory.ai/2019/02/26/image-search-ai-based-on-augmented-learning.html  
ResNet, GoogLeNet, SENet 성능 비교  
https://bskyvision.com/425  
ILSVRC 우승 이미지 알고리즘 모델들의 분류 에러율  

### 의류 데이터 수집(for 전이학습 모델)
![image](https://user-images.githubusercontent.com/44837403/136704381-549716e0-7679-4ae9-b60a-b98f78597811.png)

앞서 객체 탐지 모델에서 형태(원피스, 아우터, 상의, 하의) 와 색상(10가지)를 구분해줍니다.  
이제 전이학습 모델에서는 더 세부적인 의류 형태 속성을 구분해야 합니다.  
위 구분기준을 만들기 위해 팀원들 모두 사용자가 의류를 볼 때 어떠한 기준으로 구분하는지를 곰곰히 생각했습니다.  
유명 쇼핑몰사이트도 많이 검색해보게 되었고 회의도 많이 이루어진 끝에 위의 기준을 선정할 수 있었습니다.  

![image](https://user-images.githubusercontent.com/44837403/136704523-eb1df341-e9a2-4882-bdb8-6e66b8d0b924.png)  
![image](https://user-images.githubusercontent.com/44837403/136704529-2344fd39-3749-4441-b276-6fabcaf68b87.png)

위에서 정한 세부 분류기준에 따라 데이터를 수집해 줍니다.(Data Crawling)  
Data Class 마다 데이터 갯수는 200~300개 정도입니다.
이 때 train용. val 용 데이터의 비율은 7:3으로 정했으니 Train에는 200개가량 Val에는 100개 가량의 데이터가 배치됩니다.  


### 전이 학습을 사용하는 이유  

전이학습은 특정 분야에서 학습된 신경망의 일부를 유사하거나 전혀 새로운 분야에서 재사용하는 방식입니다.  
이 방식을 통해서 분류 기준에 비해 학습데이터 수가 적을때도 효과적이고 학습 속도 역시 빠릅니다.  
또한 정확도 역시 전이 모델을 사용하지 않을때에 비해 높습니다.  
위 3가지 이유로 전이학습 모델을 사용하였습니다.

### 전이 학습 모델 결과 확인    
![image](https://user-images.githubusercontent.com/44837403/136704839-7fd28238-9d87-4f86-9bb1-19682b1d4257.png)  
epoch 12) 학습 정확도는 train 72%, val 63%, 총 63%로 나왔습니다.  










