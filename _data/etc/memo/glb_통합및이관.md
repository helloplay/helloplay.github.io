 > **<font size = "6">kor / jpn 데이터를 glb 로 이관시켜야한다..</font>**

<br><br>

## **전제**
---
- 모든 데이터를 옮기지 않아도 된다고 한다.
- 국가별로 기획 테이블 내, row 식별자인 index가 아예 다르게 사용되는 경우가 있다.
    - 위와 더불어, ID가 특정 국가에는 아예 없는 것도 있고, 서로 ID가 바뀌어 있는 것도 있다,,,, ( 일본에선 id:3 스킨이 ,, 글로벌에선 id:4 로 사용되고,, 뭐 이런거,,) 
        - 이러한 케이스는 기획에서 필터해서 준다고 한다,, >> 검증 할 방법이 없다,, ㅠㅠ

<br><br>

### **이관 방법**
___
- <span style="color:green"> GameDB + WorldDB </span>
    - 기존 UserId + @샤딩 범위 (jpn) 로 신규 UserID를 만들어 치환
    - KOR/JPN 의 GameDB/WorldD을 UserID 기반의 샤딩(기존 GameDB방식) 으로 통합할 예정, 데이터 베이스 통합은 아니고 분리하여 사용만한다. ( 기존 Identity 로 동작하는 다른 데이터들이 문제없이 이관될 수 없다고 판단함)
    - GameDB는 애초에 샤딩을 가정하고 만들어서 서버 로직에 문제되는 부분이 없을텐데, **_WorldDB 쪽은 샤딩되어 문제되는 것들을 따로 추출_** 하여 확인이 필요하다.
    - 많다
- <span style="color:green"> AuthDB </span>
    - 많다
- <span style="color:green"> GuildDB </span>
    - 많다
- <span style="color:red"> RankDB </span>
    - 이관하지 않음
    - 해당 랭크 데이터를 기준으로 보상을 주는 컨텐츠는 동작이 안된다고 봐야함
- <span style="color:red"> PvpDB </span>
    - 이관하지 않음
    - 초기 데이터 필요없는 것으로 확인 ( 해당 컨텐츠 플레이 시, 삽입이 진행 )
    - 랭킹전 보상(일일/주간 등)의 유무 정보도 포함된다,, 받은 보상은 한번 더 받을 수 있다라는 가정이 필요
- <span style="color:red"> AuctionDB </span>
    - 이관하지 않음
    - 이관 요청 시, 경매 정보를 유저가 수동으로 비우고 이관 요청할 수 있도록 예외 처리 추가 필요
- <span style="color:red"> LogDB </span>
    - 이관하지 않음
    - 이관 이전의 로그 조회 시, DB에서 별도 수동 조회가 필요
    - 이관 이전 데이터 조회를 위해, **이관 작업하는 날에, 이전 데이터 조회용 DB를 만들어 놓는 게 좋을 듯 하다. ( 직전 라이브 DB를 복원하는 방식.. kor/jpn 둘다 )**


<br><br>

### **검증 방법**
---
- ? 

<br><br>

### **공통 이슈**
---
- DB 마이그레이션 과정
    - DB 트랜잭션 범위가 너무 넓어질 듯 하다,, 스텝별로 트랜잭션 처리하고 스텝 순으로 DB 마이그레이션이 될 수 있도록 처리가 필요해보임. 양이 좀 되어 시간이 걸리니 진행 과정을 클라에서 표현해줘야 할 수도 있어 보인다.

    - 특히 인벤토리 같이 로우가 많은 넘들은 **userId 변환**, **데이터삽입** 시, **테이블 혹은 페이지락** 반드시 유의해야 한다.

- 문자열 Collation 확인

- 검증은 어떻게 하지?
    - 이관되는 데이터의 케바케가 너무 방대해서 테스트 케이스를 모두 만드는건 불가능
    - foreginkey 없는 상태로 해당 작업을 진행하려고 하니, 무결성 체크가 불가능하다,,  
    그렇다고 지금 추가할 수도 없는 노릇이고,,




<br><br>

### **DB 별, 인덱스 중복 정리**
---
### 1. AuthDB
- _User::UserId
    - GameDB 샤딩에도 연관되어 있어서, 게임 서버의 GameDB 샤딩 룰도 변경이 필요함
    - UserId 변경해 주면서,, 다른 테이블에 참조된 UserId 들도 다 변경이 필요,,ㅠㅠ
