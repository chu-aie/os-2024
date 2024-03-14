# 스케줄링: 개요

## 워크로드에 대한 가정
가장 먼저 프로세스에 대해서 몇 가지 가정을 할 것이다. 일련의 프로세스들이 실행하는 상황을 워크로드(Workload)라고 부르기로 한다. 워크로드를 결정하는 일은 정책 개발에 매우 중요한 부분이다. 워크로드에 대해 더 잘 알수록 그에 맞게 정책을 정교하게 손볼 수 있다.
 
우리는 시스템에서 실행 중인 프로세스 혹은 작업에 대해 다음과 같은 가정을 한다.
 

- 모든 작업은 같은 시간 동안 실행된다.
- 모든 작업은 동시에 도착한다.
- 각 작업은 시작되면 완료될 때까지 실행된다.
- 모든 작업은 CPU만 사용한다. (즉, 입출력을 수행하지 않는다.)
- 각 작업의 실행 시간은 사전에 알려져 있다.

이 가정들은 굉장히 비현실적이다. 특히, 각 작업의 실행 시간이 미리 알려져 있다는 가정은 더욱 그렇다. 이 가정은 스케줄러가 모든 것을 다 알 수 있다는 것을 의미한다. 

## 스케줄링 평가 항목

## 선입선출

## 최단 작업 우선

## 최소 잔여시간 우선

## 새로운 평가 기준 : 응답 시간

## 라운드 로빈

## 입출력 연산의 고려

## 만병통치약은 없다 (No More Oracle)

## 요약