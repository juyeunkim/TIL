

# @ManytoOne





## 속성

### targetEntity

Entity Class를 정의

### cascade

현 Entity 변경에 대해 관계를 맺은 Entity도 변경 전략을 결정

- ALL

  모든 Cascade 적용

- PERSIST

  엔티티를 영속화 할 때, 이 필드에 보유된 엔티티도 유지

- MERGE

  엔티티 상태를 병합 할 때, 이 필드에 보유된 엔티티도 병합

- REFRESH

  엔티티를 새로 고칠 때, 이 필드에 보유 된 엔티티도 새로 고침

- DETACH

  부모 엔티티가 detach()를 수행하게 되면, 연관된 엔티티도 detach() 상태가 되어 변경사항이 반영되지 않는다

- REMOVE

  엔티티를 삭제 할 때, 이 필드에 보유된 엔티티도 삭제

### fetch

- FetchType
  - LAZY :  데이터를 실제로 요청하는 순간 가져온다
  - EAGER : 관계된 Entity의 정보를 미리 읽어오는 것
