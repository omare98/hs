## USTRA HR 네이밍 가이드

1. 공통규칙
    * 대소문자가 구분되어야하며 길이에 제한은 없다.
    * 예약어는 사용하지 않는다.
        * ex) **class, import...**
    * 숫자로 사직하지 않는다.
    * 특수문자는 `_(언더바)` 와 `$`만 허용한다.
    * 버전관리
        * 이미 만들어진 기능에 커스터 마이징이 필요한 화면일 경우 `V02, V03`의 접미사를 붙여 명명한다.
            * 새로 만들어지는 소스는 기존의 방식으로 작성한다.
                * ex) **PsmstrRestController**(O) **PsmstrV01RestController**(X)
            * 특히 남양넥스모 개발에 사용된 `_NYN` 의 소스를 사용할경우 `_NYN`을 `V02`로 수정한다.
            * ex) **MblApprDoc100NYNRestController -> MblApprDoc100V02RestController**
2. 클래스 (Class)
    * 클래스명은 `파스칼(Pascal) 케이스`로 작성한다.
    * 의미있는 약어의 조합으로 명명한다.
    * 버전관리에 대한 명명규칙을 따른다.
3. 엔티티 (DTO)
    * `TB_` 접두사를 제외한 테이블명 + Dto 로 한다.
    * `파스칼(Pascal) 케이스`로 작성한다.
        * ex) **TB_NOTIF_MNG -> NotifMngDto**
4. 메소드 (Method)
    * 메서드 이름은 `카멜(Camel) 케이스`로 작성한다.
    * Controller & Service
        * 속성에 접근하는 메소드명의 접두사는 `get / set`을 사용한다.
            * ex) **getCoCode()**
        * 데이터를 조회하는 메소드명의 접두사는 `find`를 사용한다.
            * ex) **findProduct()**
        * 해당 객체가 복수인지 단일인지 구분하는 메소드명의 접미사는 `List`를 사용한다.
            * ex) **findProductList()**
        * B를 기준으로 A를 하겠다는 메소드명의 전치사는 `By`를 사용한다.
            * ex) **findProductByCoCode()**
            * ex) **modifyProductByCoCode()**
        * 데이터를 등록/수정/삭제하는 메소드명의 접두사는 `save`를 사용한다.
            * ex) **saveProduct()**
        * 데이터를 입력하는 메소드명의 접두사는 `add`를 사용한다.
            * ex) **addProduct()**
        * 데이터를 수정하는 메소드명의 접두사는 `modify`를 사용한다.
            * ex) **modifyProduct()**
        * 데이터를 삭제하는 메소드명의 접두사는 `remove`를 사용한다.
            * ex) **removeProduct()**
        * 데이터를 초기화 하는 메소드명의 접두사는 `init`을 사용한다.
            * ex) **initData()**
        * 반환값이 boolean 인 메소드는 `is`를 접두사로 사용한다.
            * ex) **isData()**
        * 데이터가 있는지 확인하는 메소드명의 접두사는 `has`를 사용한다.
            * ex) **hasData()**
        * 새로운 객체를 만든뒤 해당 객체로 변환해주는 메소드명의 접두사는 `create`를 사용한다.
            * ex) **createUser()**
    * Mapper
        * 데이터를 조회하는 메소드명의 접두사는 `select`를 사용한다.
            * ex) **selectProduct()**
            * ex) **selectProductCount()**
        * 해당 객체가 복수인지 단일인지 구분하는 메소드명의 접미사는 `List`를 사용한다.
            * ex) **selectProductList()**
        * B를 기준으로 A를 하겠다는 메소드명의 전치사는 `By`를 사용한다.
            * ex) **selectProductByCoCode()**
            * ex) **updateProductByCoCode()**
        * 데이터를 입력하는 메소드명의 접두사는 `insert`를 사용한다.
            * ex) **insertProduct()**
        * 데이터를 수정하는 메소드명의 접두사는 `update`를 사용한다.
            * ex) **updateProduct()**
        * 데이터를 삭제하는 메소드명의 접두사는 `delete`를 사용한다.
            * ex) **deleteProduct()**
        * 데이터를 입력 그리고 수정하는 메소드명의 접두사는 `save`를 사용한다.
            * ex) **saveProduct()**

5. 상수 (final)
    * `대문자의 스네이크(Snake) 케이스`로 작성한다.
    * ex) **private static final USER_NAME = "AAA";**
6. 변수
    * `소문자 또는 카멜(Camel) 케이스`로 작성한다.
7. Database Object
    * 의미있는 약어롤 조합하여 `대문자의 스네이크(Snake) 케이스`로 작성한다.
    * Table : `TB_` 의 접두사를 사용한다.
    * Function : `FN_` 의 접두사를 사용한다.
    * Procedure : `STP_ ` 의 접두사를 사용한다.
    * View : `V_` 의 접두사를 사용한다.
8. Thymeleaf html
    * 모두 소문자로 작성한다.
    * 해당 업무화면의 기능을 처리하는 클래스명 기준으로 작성한다.
    * 파일 위치는 `[depth1]` > `[depth2]` > `html 파일` 에 위치한다.
        * ex) `인사` > `인사관리` > `인사정보관리.html`
    * 소문자로 작성하며 뒤에 버전은 대문자로 작성한다.
        * ex) **UserMngV02RestClass -> usermngV02.html**
    * 팝업은 케밥(kebab) 케이스로 작성하며 pop 접미사를 사용한다.
        * ex) **user-mng-pop.html**
9. URL
    * Rest Controller 의 RequestMapping URL은 프로그램에 등록된 프로그램 URL 과 같다.
    * Thymeleaf html 파일 위치 경로와 같다.
        * ex) 프로그램 URL :  **/emp/empmngV02/view -> /emp/empmngV02**
    * `_NYN` 이 들어간 URL은 `V02` 의 버전으로 수정하며 관련 소스도 같이 수정한다.
        * ex) /emp/psmstinfo/emppsmstinfolst_NYN -> /emp/psmstinfo/emppsmstinfolstV02
        * EmppsmstinfolstNYNRestController -> EmppsmstinfolstV02RestController
        * EmppsmstinfolstNYNService -> EmppsmstinfolstV02Service
        * EmppsmstinfolstNYNMapper -> EmppsmstinfolstV02Mapper