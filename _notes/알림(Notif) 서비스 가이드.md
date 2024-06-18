## USTRA HR 알림(Notif) 서비스 가이드

### package

* notif

### source

* 엔티티
    * NotifDto
        * 알림 발송 서비스의 모든 전달인자(argument)정보를 가지는 엔티티
        * emplNo : 알림 대상자의 사번 
          * 알림서비스 대상자의 사번만 입력받고 나머지 정보는 공통에서 처리함
          * 이메일 주소, 전화번호 그리고 사원에 관한 부가정보등.
        * mailDto : 메일발송의 인자정보를 같는 MailDto 엔티티 목록
        * talkDto : 알림톡발송의 인자정보를 같는 TalkDto 엔티티 목록

        ```java
        NotifDto notifDto = NotifDto.builder()
            .emplNo(emplNo)
            .mailDto(MailDto.class)
            .talkDto(TalkDto.class)
            .build();
        ```

    * MailDto
        * 메일발송 엔티티
        * paramMap : HashMap<String, String> 에 메일 양식에 설정된 항목들의 값을 key, value 로 넣어준다.

        ```java
        HashMap<String, String> params = new HashMap<>();
        params.put("content", "내용 입니다.");
        MailDto mailDto = MailDto.builder()
            .paramMap(paramMap)
            .build();
        ```

    * TalkDto
        * 알림톡발송 엔티티
        * argumentList 인자는 `Mutable List` 를 사용하여 생성한다.

        ```java
        TalkDto talkDto = TalkDto.builder()
            .argumentList(new ArrayList<>(Arrays.asList("arg1", "arg2")))
            .build();
        ```

* Service
    * NotifService
        * sendNotif(NotifCode notifCode, NotifDto notifDto) : 단건 알림발송 method
        * sendNotif(NotifCode notifCode, List<NotifDto> notifList) : 다건 알림발송 method

### 사용방법

* 단건발송, 다건발송 모두 notifCode 는 필수값이다.
  * 발송 결과는 NotifResultDto 를 return 한다.
      * mailResult : 메일 발송 결과 DTO
      * talkResult : 유스트라톡 발송 결과 DTO
      
      ```java
      public class NotifTest {
          // 알림 서비스
          private final NotifService notifService;
        
          // 알림 단건발송
          public void sendNotifSingle() {
              // 메일 엔티티
              NotifCode notifCode = "알림코드";
              NotifResultDto notifResultDto = notifService.sendNotif(notifCode, NotifDto.builder()
                  .emplNo("사원번호")
                  .mailDto("메일 DTO")
                  .talkDto("알림톡 DTO")
                  .build()
              );
          }
        
          // 알림 다건 발송
          public void sendNotifMulti() {
              NotifCode notifCode = "알림코드";
              List<NotifDto> notifList = new ArrayList<>();
              for (int i=0; i<10; i++) {
                  notifList.add(NotifDto.builder()
                      .emplNo("사원번호_" + i)
                      .mailDto("메일 DTO_" + i)
                      .talkDto("알림톡 DTO_" + i)
                      .build()
                  );
              }
              // 알림 발송
              NotifResultDto notifResultDto = notifService.sendNotif(notifCode, notifList);
          }
      }
      ```