## USTRA HR 알림(Notif) 서비스 가이드

### package

* [notif 위치](../ustra-hr-system-core/src/main/java/hr/system/core/notif)
    * mail : 메일 발송 서비스 관련 소스
    * mng : 알림관리 소스
    * notif : 알림 서비스 관련 소스
    * talk : 알림톡 서비스 관련 소스

### source

* 엔티티
    * [NotifDto](../ustra-hr-system-core/src/main/java/hr/system/core/notif/notif/dto/NotifDto.java)
        * 알림 발송 서비스의 모든 전달인자(argument)정보를 가지는 엔티티
        * notifCode : 알림코드 [NotifCode](../ustra-hr-system-core/src/main/java/hr/system/core/notif/mng/dto/NotifCode.java) enum 이며 필수값.
        * mailList : 메일발송의 인자정보를 같는 MailDto 엔티티 목록
        * talkList : 알림톡발송의 인자정보를 같는 TalkDto 엔티티 목록
        * Static Factory Methods `of` 를 사용하여 객체 생성한다.
        ```java
        NotifDto notifDto = NotifDto.of(NotifCode, mailList, talkList);
        ```
    * [MailDto](../ustra-hr-system-core/src/main/java/hr/system/core/notif/mail/dto/MailDto.java)
        * 메일발송 엔티티
        * 빌더패턴(builder pattern) `builder` 을 사용하여 객체 생성한다.
        ```java
        MailDto mailDto = MailDto.builder()
            .frMailAd("발신자 이메일")
            .frName("발신자명")
            .toMailAd("수신자 이메일")
            .toName("수신자명")
            .subject("제목")
            .message("내용")
            .build();
        ```
    * [TalkDto](../ustra-hr-system-core/src/main/java/hr/system/core/notif/talk/dto/TalkDto.java)
        * 알림톡발송 엔티티
        * 빌더패턴(builder pattern) `builder` 을 사용하여 객체 생성한다.
        * argumentList 인자는 `Mutable List` 를 사용하여 생성한다.
        ```java
        TalkDto talkDto = TalkDto.builder()
            .phoneNumber("수신자 전화번호")
            .argumentList(new ArrayList<>(Arrays.asList("arg1", "arg2")))
            .build();
        ```

* Service
    * [NotifService](../ustra-hr-system-core/src/main/java/hr/system/core/notif/notif/service/NotifService.java)
        * sendNotif(NotifDto notifDto) : 알림발송 method
        * sendNotif(NotifDto notifDto, FileDataSource fileDataSource) : 첨부파일 포함 알림 발송 method

### 사용방법

* 단건발송, 다건발송 모두 인자를 List로 한다.
* 발송 결과는 [NotifResultDto](../ustra-hr-system-core/src/main/java/hr/system/core/notif/notif/dto/NotifResultDto.java) 를 return 한다.
    * mailResult : 메일 발송 결과 DTO
    * talkResult : 유스트라톡 발송 결과 DTO
    ```java
    public class NotifTest {
        // 알림 서비스
        private final NotifService notifService;
        
        // 알림 단건발송
        public void sendNotifSingle() {
            // 메일 엔티티
            MailDto mailDto = MailDto.builder()
                .frMailAd("발신자 이메일")
                .frName("발신자명")
                .toMailAd("수신자 이메일")
                .toName("수신자명")
                .subject("제목")
                .message("내용")
                .build();
    
            // 알림톡 엔티티
            TalkDto talkDto = TalkDto.builder()
                .phoneNumber("수신자 전화번호")
                .argumentList(new ArrayList<>(Arrays.asList("arg1", "arg2")))
                .build();
            // 알림 발송
            NotifResultDto notifResultDto = notifService.sendNotif(NotifDto.of(NotifCode.HOLI_REQ, List.of(mailDto), List.of(talkDto)));
        }
        
        // 알림 다건 발송
        public void sendNotifMulti() {
            List<MailDto> mailList = new ArrayList<>();
            for (int i=0; i<10; i++) {
                mailList.add(MailDto.builder()
                    .frMailAd("발신자 이메일_" + i)
                    .frName("발신자명_" + i)
                    .toMailAd("수신자 이메일_" + i)
                    .toName("수신자명_" + i)
                    .subject("제목_" + i)
                    .message("내용_" + i)
                    .build());
            }
            
            List<TalkDto> talkList = new ArrayList<>();
            for (int i=0; i<10; i++) {
                mailList.add(TalkDto.builder()
                    .phoneNumber("수신자 전화번호_" + i)
                    .argumentList(new ArrayList<>(Arrays.asList("arg1_" + i, "arg2_" + i)))
                    .build());
            }
            // 알림 발송
            NotifResultDto notifResultDto = notifService.sendNotif(NotifDto.of(NotifCode.HOLI_REQ, mailList, talkList));
        }
    }
    ```