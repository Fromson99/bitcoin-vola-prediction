# GNN(Masked Language Model)을 활용한 분산투자 (Done)
---

## 목차 (Table of Contents)

1. [프로젝트 소개 (Introduction)](#프로젝트-소개-introduction)  
2. [분석 핵심 (Point)](#프로젝트-핵심-Point)  
3. [프로젝트 구조 (Project-Structure)](#프로젝트-구조-project-structure)  


---

## 프로젝트 핵심 (Point)

- **목적**: 동적인 시장에서 빠르게 테마를 만들고 해당 테마를 기반으로 분산투자 
- **현재 서비스의 한계**:
  - 테마는 투자자의 의사결정을 도와줄 뿐만 아니라 직관적으로 분산투자하기 용이하다. 하지만 현재 테마는 사람이 수동으로 만들기에 시장상황을 즉각적으로 반응하기 어려울 수 있다.
    
- **가설**:
  - GNN을 통해 시장상황을 모델링하고 임베딩 값을 통해 군집화를 한다면 좋은 군집이 될 수 있지 않을까?

- **검정 방법**
  - 테마로 만든 군집을 통해 분산투자를 진행해 잘 분산되었는지를 확인한다. 


## 프로젝트 구조 (Project structure)


1. **네트워크 구성**  
   - (1) **노드 특성 선정**: GNN 모델의 가정에 부합하도록 노드(feature)들을 정의  
   - (2) **엣지 구성**: 노드 간 스필오버(spillover) 값을 이용해 엣지(weight) 계산

2. **T-GNN 학습**  
   - 구성된 네트워크를 T-GNN 모델에 입력하여 임베딩(Embedding)을 학습

3. **KNN 클러스터링**  
   - 학습된 네트워크 임베딩 값을 기반으로 KNN 클러스터링 수행

4. **새로운 네트워크 구성**  
   - 클러스터링된 결과(자산 군집 및 테마)를 활용하여 새로운 네트워크를 형성

5. **포트폴리오 구성**  
   - 하나의 자산을 기준으로, **다른 테마**이면서 **네트워크 상에서 거리가 가장 먼** 자산들을 선택  
   - 동일가중 포트폴리오(equal-weight)로 편성

6. **성능 확인**  
   - 최종적으로 구성된 포트폴리오나 네트워크 모델의 성능을 평가
![Performance](./images/Performance.png)




```mermaid
flowchart LR
    A([데이터 제공<br>및 전처리]) --> B([네트워크 구성 <bar> 노드 특성+Spillover 엣지])
    B --> C([T-GNN<br>학습])
    C --> D([임베딩 -><br>KNN 클러스터링])
    D --> E([클러스터링<br>결과 바탕<br>새로운 네트워크])
    E --> F([자산 선정<br>+ 거리가 먼<br>자산 선택<br>-> 동일가중 포트폴리오])
    F --> G([모델 및 포트폴리오<br>성능 확인])