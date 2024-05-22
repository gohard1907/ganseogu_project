# ganseogu_project
강서구 내 이동노동자 쉼터 입지 분석

## 분석 개요

- **배경**
  
이동노동자 종사자 수가 꾸준히 증가하고 있음에도 불구하고 강서구 내에는 그들을 위한 적절한 휴식 공간이 없는 상황. (지난 2020년 8월 마곡역 근처에 개소하였으나 접근성과 수요를 고려하지 못한 위치 선정으로 2년 만에 철거)
이에 강서구 내에서 활동하는 이동노동자를 위한 쉼터 개설 및 이용률을 높일 수 있는 최적의 위치를 추천해주기 위함

- **분석 목표**

강서구 내 이동노동자 쉼터 입지 분석: 이동노동자들의 복지 향상을 위한 최적의 위치 선정

- **분석 도구**

Python(pandas, sklearn), QGIS, Excel

- **담당 역할**

택배 데이터 군집화, 화곡동 공간 데이터 시각화 및 점수 산출

- **사용 데이터**

강서구 생활인구 수 (서울 열린데이터 광장_서울 생활인구 데이터) 

강서구 공영주차장 주차면 수 (서울 열린데이터 광장_서울시 공영주차장 안내 정보) 

강서구 지하철역수, 버스정류장 수 (서울 열린데이터 광장_서울시 상권분석서비스(집객시설-행정동)) 

서울시 버스정류소 위치정보 (서울 열린데이터 광장) 

강서구 일반 / 휴게음식점 수 (서울 열린데이터 광장_서울시 강서구 일반음식점 / 휴게음식점 인허가 정보) 

강서구 택배 건수* (서울 빅데이터 캠퍼스_서울시 CJ 대한통운 택배 유형별 월 데이터) 

서울시 집객시설 공간데이터 (서울 빅데이터 캠퍼스) 

강서구 배달 건수 (데이터스(롯데카드)_우리 동네 배달/외식 지도)

**택배 데이터는 반출 정책상 비율로 환산하여 사용*


## 분석 내용

1. 이동노동자를 직업에 따라 **택배, 배달, 대리, 방문**(판매, 설치 등)으로 나눠 **군집화**를 통한 최적의 행정동 선택
---

2. 사용한 군집화 기법
- K-means
- 계층 군집화
- GMM(가우시안 혼합 모델)
- K-medoids

---

3. **실루엣 점수 계산**을 통해 가장 정확도가 높은 군집화 기법 채택
![Untitled (6)](https://github.com/gohard1907/ganseogu_project/assets/33924444/0381bf7a-adf0-4070-b820-718caa042539)

**배달은 계층 군집화, GMM 군집화 결과 모두 고려*

**방문은 K-means, GMM 군집화 결과 동일*

---

4. 군집화를 통해 형성된 군집 및 변수별 평균 점수 확인
- 4-1. 택배 (K-means)

 ![Untitled (7)](https://github.com/gohard1907/ganseogu_project/assets/33924444/ba0f5df8-63eb-44db-9f22-a517de00a3ab)
   
→ 특성 변수라고 할 수 있는 **택배 건수가 가장 높은** 두 번째 군집 선택

: 가양1동, 공항동, 등촌3동, 발산1동, 방화1동, 염창동, 우장산동

- 4-2. 배달 (계층 군집화)
    
![Untitled (12)](https://github.com/gohard1907/ganseogu_project/assets/33924444/75b25202-873d-4b95-b108-de25eed7fdd8)
    

→ 특성 변수라고 할 수 있는 **배달 건수가 가장 높은** 두 번째 군집 선택

: 공항동, 등촌3동, 발산1동, 방화1동, 염창동, 우장산동, 화곡1동

- 4-2. 배달 (GMM)
    
![Untitled (13)](https://github.com/gohard1907/ganseogu_project/assets/33924444/56e176a0-e802-4043-aaec-4a2964e30410)

→ 특성 변수라고 할 수 있는 **배달 건수가 가장 높은** 두 번째 군집 선택

: 가양1동

- 4-3. 대리 (계층 군집화)
    
![Untitled (10)](https://github.com/gohard1907/ganseogu_project/assets/33924444/c2b10e14-c8e0-4bf4-884e-f6f7085243bf)
    

→ **전체적으로 값이 높은** 세 번째 군집 선택

: 화곡1동

- 4-4. 방문(K-means)
    
![Untitled (11)](https://github.com/gohard1907/ganseogu_project/assets/33924444/d5171488-9791-42bc-a9f4-8e1f04984ecd)
    

→ **전체적으로 값이 높은** 두 번째 군집 선택

: 화곡1동, 가양1동, 방화2동

---

5. 각 직업별 군집화를 통해 가장 공통된 행정동 선택

**→ 가양1동(3회), 화곡1동(3회)**

---

6. QGIS를 통해 가양1동, 화곡1동을 150m*150m 단위의 블록으로 분할

---

7. 강서구의 음식점, 집객시설, 지하철역, 버스정류장, 공영주차장 공간 데이터를 활용하여 각 블록별 시설 개수 파악 후 아래 기준에 따라 점수 부여, **총 점수가 가장 높은 블록 선정**

![Untitled (14)](https://github.com/gohard1907/ganseogu_project/assets/33924444/aeb4cb12-58ca-4ecc-a70b-80bba68db9b2)

**지하철역, 공용 주차장은 희소한 변수이므로 해당 시설이 위치한 블록에는 5점, 그 인접 블록에는 2.5점 부여*

## 분석 결과

- 가양1동 : 음식점 점수와 지하철 점수가 높은 **상업지역**이 선정됨

![Untitled (15)](https://github.com/gohard1907/ganseogu_project/assets/33924444/ca790aab-d45a-489b-a905-19bc4b187f63)

![image01](https://github.com/gohard1907/ganseogu_project/assets/33924444/19c4b1cc-0bf9-47cb-a92b-3a11a89fef0d)

![화면 캡처 2024-05-22 133251](https://github.com/gohard1907/ganseogu_project/assets/33924444/77a7c398-7198-4e67-ab58-f7b02b693206)


---

- 화곡1동 : 주차장 점수와 버스정류장 점수가 높은 **주거지역**이 선정됨

![Untitled (16)](https://github.com/gohard1907/ganseogu_project/assets/33924444/6777faf5-505d-4f97-8c92-5a4a02523604)

![image05](https://github.com/gohard1907/ganseogu_project/assets/33924444/85ffa68d-5d72-48c3-b40d-ba3aa71f17bc)

![화면 캡처 2024-05-22 133319](https://github.com/gohard1907/ganseogu_project/assets/33924444/93d3bfb8-82f4-4d76-ac3e-a035da621e50)


## **활용방안 및 기대효과**

- 쉼터의 최적 위치 선정을 통해 이동노동자들의 편의가 향상될 것으로 예상
- 쉼터 이용률의 증가는 이동노동자들의 업무 효율성 향상과 건강한 노동 환경 조성에 기여할 것이며, 이를 통해 과로, 졸음운전 등의 산업 재해 예방 가능
- 전반적인 사회적 관심과 복지 향상에도 긍정적인 영향을 미칠 것으로 기대





