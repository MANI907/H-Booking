# H-Booking
에어비앤비 따라잡기
-----
<img src ="/Images/c9e64cd1-4307-4f74-8f3d-0bef898e2eff.jpg" width ="700">

# 평가항목
1. Saga
2. CQRS(Command and Query Responsitibility and Segregation)
3. Correlation/Compensation(반드시 구현)
4. Req / Resp
5. Gateway(단일진입점)
6. Deploy / Pipeline
7. Circuit Breaker
8. Autoscale (HPA)
9. Zero-downtime deploy (readiness probe)
10. ConfigMap/Persistence Volume
11. Polyglot
12. Self-healing (liveness probe)
   
----
# Saga
+ Step1<p>
*전반적인 어플리케이션의 구조 및 흐름을 인지한 상태에서 실시한 이벤트 스토밍과정으로, 기초적인 이벤트 도출이나, Aggregation 작업은 `Bounded Context`를 먼저 선정하고 진행*
<img src = '/images/Screen Shot 2022-03-28 at 14.42.26.png'>

+ Step2<p>
*Pub/Sub연결*
<img src = '/images/Screen Shot 2022-03-28 at 15.18.42.png'>

+ Step3<p>
*완성본 대한 기능 검증*
<img src = '/images/Screen Shot 2022-03-28 at 15.30.42.png'>

```
  - 기능요소
    - 호스트가 제공할 숙소를 등록/수정/삭제한다
    - 고객이 숙소를 선택하여 예약을 요청한다
    - 예약 요청 직후 결제가 진행된다
    - 사용자가 결제한다
    - 결제가 완료되면 호스트에게 예약 요청 정보가 전달된다
    - 호스트가 예약을 확정하면 고객에게 예약내역이 전달된다
    - 고객이 예약을 취소할 수 있다
    - 숙박예약이 취소될 경우 취소 내역이 전달된다
    - 숙소에 대한 정보 및 예약 상태 등을 한 화면에서 확인할 수 있다
  - 비기능요소
    - 마이크로 서비스를 넘나드는 시나리오에 대한 트랜잭션 처리
    - 고객 결제가 완료 되지 않은 예약 요청은 'ACID' 트랜잭션 적용(Request/Response 방식 처리)
    - 장애 격리
      - 숙소 등록/수정 기능, 예약 내역 메시지 전송 기능과 별도로 예약은 24시간 처리 가능(Async, Eventual Consistency)
      - 예약시스템이 과중되면 예약을 잠시 받지 않고 잠시 후에 하도록 유도(Circuit breaker, fallback)
  
  ```
  
# CQRS(Command and Query Responsitibility and Segregation)
