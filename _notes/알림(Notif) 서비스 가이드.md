## USTRA HR 알림(Notif) 서비스 가이드

### 1. 알림

#### package

* notif 위치 : /ustra-hr-system-core/src/main/java/hr/system/core/notif

#### source

* 알림코드
    * NotifCode
    * 공통코드 (SY21) 을 enum 으로 정의 

* 엔티티
    * NotifDto
        * 알림 발송 서비스의 모든 전달인자(argument)정보를 가지는 엔티티
        * toEmplNo : 알림 대상자의 사번 
            * 알림서비스 대상자의 사번만 입력받고 나머지 정보는 공통에서 처리함
            * 이메일 주소, 전화번호 그리고 사원에 관한 부가정보등.
        * paramMap : 알림의 인자정보 파라미터 Map

        ```java
        import java.util.ArrayList;
        import java.util.HashMap;
        
        HashMap<String, String> paramMap = new HashMap<>();
        paramMap.put("content", "내용 입니다.");
      
        NotifDto notifDto = NotifDto.builder()
            .toEmplNo(emplNo)
            .paramMap(paramMap)
            .build();
        ```

* Service
    * NotifService
        * sendNotif(NotifCode notifCode, NotifDto notifDto) : 단건 알림발송 method
        * sendNotif(NotifCode notifCode, NotifDto notifDto, String fileId) : 단건 알림발송 with 첨부파일 ID method
        * sendNotif(NotifCode notifCode, List<NotifDto> notifList) : 다건 알림발송 method
        * sendNotif(NotifCode notifCode, List<NotifDto> notifList, String fileId) : 다건 알림발송 with 첨부파일 ID method

#### 사용방법

* 단건발송, 다건발송 모두 notifCode 는 필수값이다.
    * 발송 결과는 NotifResultDto 를 return 한다.
        * mailResultList : List of MailResultDto
            * 메일결과 DTO 목록 
                * receiveEmailAddr : 수신자 이메일
                * receiveName : 수신자명
                * resultCode : 결과 코드
                * resultMessage : 결과 메시지
        * talkResultList : List of TalkResultDto
            * 알림톡결과 DTO 목록
                * receivePhoneNumber : 수신자 전화번호
                * resultCode : 결과 코드
                * resultMessage : 결과 메시지

        ```java
        public class NotifTest {
            // 알림 서비스
            private final NotifService notifService;
        
            // 알림 단건발송
            public void sendNotifSingle() {
                // 메일 엔티티
                NotifCode notifCode = "알림코드";
                // 알림 파라미터 Map
                HashMap<String, String> paramMap = new HashMap<>();
                NotifResultDto notifResultDto = notifService.sendNotif(notifCode, NotifDto.builder()
                    .toEmplNo("알림 대상 사원번호")
                    .paramMap(paramMap)
                    .build()
                );
            }

            // 알림 단건발송 with 첨부파일 ID
            public void sendNotifSingleWithFileId() {
                // 메일 엔티티
                NotifCode notifCode = "알림코드";
                // 첨부파일 ID
                String fileId = "첨부파일 ID";
                // 알림 파라미터 Map
                HashMap<String, String> paramMap = new HashMap<>();
                NotifResultDto notifResultDto = notifService.sendNotif(
                    notifCode, 
                    NotifDto.builder()
                        .toEmplNo("알림 대상 사원번호")
                        .paramMap(paramMap)
                        .build(), 
                    fileId
                );
            }
        
            // 알림 다건 발송
            public void sendNotifMulti() {
                NotifCode notifCode = "알림코드";
                List<NotifDto> notifList = new ArrayList<>();
                for (int i=0; i<10; i++) {
                    // 알림 파라미터 Map
                    HashMap<String, String> paramMap = new HashMap<>();
                    notifList.add(NotifDto.builder()
                        .toEmplNo("알림 대상 사원번호_" + i)
                        .paramMap(paramMap)
                        .build()
                    );
                }
                // 알림 발송
                NotifResultDto notifResultDto = notifService.sendNotif(notifCode, notifList);
            }

            // 알림 다건 발송 with 첨부파일 ID
            public void sendNotifMultiWithFileId() {
                NotifCode notifCode = "알림코드";
                // 첨부파일 ID
                String fileId = "첨부파일 ID";
                for (int i=0; i<10; i++) {
                    // 알림 파라미터 Map
                    HashMap<String, String> paramMap = new HashMap<>();
                    notifList.add(NotifDto.builder()
                        .toEmplNo("알림 대상 사원번호_" + i)
                        .paramMap(paramMap)
                        .build()
                    );
                }
                // 알림 발송
                NotifResultDto notifResultDto = notifService.sendNotif(notifCode, notifList, fileId);
            }
        }
        ```

### 2. 메일

#### package

* mail 위치 : /ustra-hr-system-core/src/main/java/hr/system/core/mail

#### source

* 양식코드
    * formCode : 메일양식의 코드 (TB_MAIL_BASE_FORM 테이블의 FORM_CODE)

* 엔티티
    * MailDto
        * 메일 발송 서비스의 모든 전달인자(argument)정보를 가지는 엔티티
        * toEmailAddr : 수신자 이메일 주소 (필수)
        * toName : 수신자 이름 (필수)
        * fromEmailAddr : 발신자 이메일 주소 (시스템에서 정의)
        * fromName : 발신자 이름 (시스템에서 정의)
        * mailTitle : 메일 제목 (기본적으로 메일양식에서 정의한 메일제목을 사용, 재정의시 재정의한 메일제목으로 발송)
        * paramMap : 메일의 인자정보를 파라미터 Map (HashMap<String, String>)
        * isTest : 테스트 여부 (기본 : false)
        
        ```java
        import java.util.HashMap;
        
        HashMap<String, String> paramMap = new HashMap<>();
        paramMap.put("content", "내용 입니다.");
      
        MailDto mailDto = MailDto.builder()
            .toEmailAddr("수신자 이메일 주소")
            .toName("수신자 이름")
            .paramMap(paramMap)
            .build();
        ```

* Service
    * MailService
        * sendMail(String formCode, MailDto mailDto) : 단건 메일발송 method
        * sendMail(String formCode, MailDto mailDto, String fileId) : 단건 발송 with 첨부파일 ID method
        * sendMail(String formCode, List<MailDto> mailList) : 다건 메일발송 method
        * sendMail(String formCode, List<MailDto> mailList, String fileId) : 다건 메일발송 with 첨부파일 ID method

### 3. 알림톡

#### package

* talk 위치 : /ustra-hr-system-core/src/main/java/hr/system/core/talk

#### source

* 알림톡 구분
    * msgDiv : 메일양식의 코드 (TB_MSG_MNG 테이블의 MSG_DIV)

* 엔티티
    * TalkDto
        * 알림톡 발송 서비스의 모든 전달인자(argument)정보를 가지는 엔티티
        * phoneNumber : 수신자 휴대전화번호 (필수)
        * paramMap : 알림톡의 인자정보를 파라미터 Map (HashMap<String, String>)
        * isTest : 테스트 여부 (기본 : false)
        
        ```java
        import java.util.ArrayList;
        
        HashMap<String, String> paramMap = new HashMap<>();
        paramMap.put("content", "내용 입니다.");
        
        TalkDto talkDto = TalkDto.builder()
            .phoneNumber("수신자 휴대전화번호")
            .paramMap(paramMap)
            .build();
        ```

* Service
    * TalkService
        * sendTalk(String msgDivCode, TalkDto talkDto) : 단건 알림톡발송 method
        * sendTalk(String msgDivCode, List<TalkDto> talkList) : 다건 알림톡발송 method

### 4.테스트 방법

* 메일
    * MailService
        * application-api.yml 의 send-service 값에 따라 AWS(자바 메일), SCP(RestApi) 방식의 메일서비스를 이용한다.
        * 현재 local, dev 에서는 MailService class 의 TEST_MAIL 에 정의된 주소로 메일을 발송하여 테스트 할 수 있다.  
* 알림톡
    * TalkService
        * application-msg.yml 의 test-mode 값을 'Y' 로 설정하여 테스트 할 수 있다.
        * 현재 local, dev 에서는 TalkService class 의 TEST_PHONE_NUMBER 에 정의된 전화번호로 알림톡을 발송하여 테스트 할 수 있다.