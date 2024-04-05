## USTRA HR 공통 함수 및 컴포넌트

### Java

1. [CommonUtils.java](../ustra-hr-system-core/src/main/java/hr/system/core/util/CommonUtils.java)
    * 공통 유틸클래스 CommonUtils 추가
        * resNoMasking() : 주민번호 마스킹
        * emailMasking() : 이메일 마스킹
        * phoneNumberMasking() : 휴대전화번호 마스킹
        * nameMasking() : 이름 마스킹
        * acctNoMasking() : 계좌번호 마스킹
        * creditCardNoMasking() : 신용카드번호 마스킹
        * escapeHtml() : html escape 처리
        * unescapeHtml() : html unescape 처리
2. [SecurityUtils.java](../ustra-hr-system-core/src/main/java/hr/system/core/util/SecurityUtils.java)
    * 보안 유틸클래스 SecurityUtils 추가
        *

### Javascript

1. [common-utils.js](../ustra-hr-system-thymeleaf/src/main/resources/static/assets/js/common-utils.js)
    * 공통 유틸함수 CommonUtils 추가
        * getConfigValue() : 설정값 조회
        * getConfigValueList() : 설정값 목록 조회
        * getConfigValueSY001() : 전자결재(SY001) 설정값 조회
        * getConfigValueAS001List() : 발령요소 설정값 목록 조회
        * getCommonCodeList() : 공통코드 목록 조회
        ```javascript
        /**
         * 파라미터의 옵션에 따라 자동으로 select, multi, radio, checkbox 에 공통코드 바인딩
         */
        CommonUtils.getCommonCodeList([
            { 
                patternCode: 'AP02', 
                returnKey: 'ap02List', 
                bindList: [
                    {
                        bindId: 'select-commonCode', 
                        bindType: CommonUtils.BIND_TYPE.SELECT, 
                        selectedVal: 'CE',
                        placeholder: '전체'
                    },
                    {
                        bindId: 'radio-commonCode', 
                        bindType: CommonUtils.BIND_TYPE.RADIO, 
                        selectedVal: 'CE'
                    }
                ]
            }, 
            {
                patternCode: 'AIRF', 
                returnKey: 'airfList', 
                bindList: [
                    {
                        bindId: 'select-multi',
                        bindType: CommonUtils.BIND_TYPE.MULTI, 
                        selectedVals: ['A', 'B', 'C'],
                        placeholder: '전체'
                    }
                ]
            },
            {
                patternCode: 'AP03', 
                returnKey: 'ap03List', 
                bindList: [
                    {
                        bindId: 'checkbox-commonCode', 
                        bindType: CommonUtils.BIND_TYPE.CHECKBOX, 
                        isCheckAll: false
                    }
                ]
            }
        ]).then(result => {
            // result array 내용
            [
                {ap02List : [{code: '', codeName: ''}, {...}]},    // AP02 의 코드 정보 
                {airfList : [...]},    // AIRF 의 코드 정보
                {ap03List : [...]}     // AP03 의 코드 정보
            ]
        });
        ```
        * ajax : json 형식의 데이터 서버요청
            * get() : GET 방식
            * post() : POST 방식
            * put() : PUT 방식
            * delete() : DELETE 방식
              ```javascript
              /**
                                                      * @param {string} url                      URL
                                                      * @param {object} paramObj                 파라미터 Object
                                                      * @param {Function||string} callback       callback 함수
                                                      * @param {boolean} async                   비동기 여부
              */
              CommonUtils.ajax.post('url', {paramObject}, (result) => {
              alert(result.notifResult);
              }, true);
              ```
        * toCamelCase() : 카멜 케이스로 변환
        * toPascalCase() : 파스칼 케이스로 변환
        * toSnakeCase() : 스네이크 케이스로 변환
        * toKebabCase() : 케밥케이스로 변환

### Components

1. [input.html](../ustra-hr-system-thymeleaf/src/main/resources/templates/components/input.html)

* class 명에 따른 입력제어 추가 (addClass 에 입력)
    * num-only : 숫자만 입력받음
    * num-comma : 숫자 + 3자리 콤마
    * alpha-only : 영문만
    * han-only : 한글만
    * alpha-han-only : 영문, 한글만
  ```html
  <th:block th:replace="components/input :: input(
      id='numOnly',
      name='numOnly',
      required=true,
      textAlign='left',
      addClass='input num-only',
      maxLength='3',
      title='숫자만 입력'
      )">
  </th:block>
  ```



     