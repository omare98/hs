## USTRA HR SQL 스타일 가이드

### 공통

* 네이밍에는 문자, 숫자, _ 만 사용한다.
* 네이밍의 시작은 문자로 시작하고 _로 끝나지 않는다.
* 탭은 사용하지 않고 공백만 사용한다.
* 쿼리 내 들여쓰기는 4칸 들여쓰기로 한다.
* 테이블, 필드 이름은 모두 대문자이여야 하며 스네이크 케이스를 지향한다. (카멜케이스 X)
* 키워드, 타입, 함수들은 모두 대문자로 표현한다.
* SELECT, FROM, WHERE 와 같은 예약어들은 항상 대문자를 사용한다.
* 필드는 항상 대문자를 사용한다.

```sql
SELECT 
       SUBSTR(COLUMN1, 0, 3) AS COLUMN1
     , F_GET_CODE_NM() AS CODE_NM
  FROM TABLE1 
```

### 주석

* Mybatis 태그 시작전에 간략한 설명의 주석을 추가한다.
* Mybatis 태그내에 항상 query id의 경로를 SQL문 시작전에 명시한다. (/* [sql파일위치].[sql id] */)

```html
<!-- 공통코드 조회 -->
<select id="findCommonCode" resultType="CamelCaseMap">
    /* hr.system.core.code.mapper.CodeMapper.findCommonCode */
</select>
```

### Queries

1. 공통
    * 중앙 정렬 River Indentation을 적용하여 명령어를 우측 정렬, 컬럼 및 조건을 좌측 정렬한다.
   ```sql
   SELECT ...
     FROM ...
     JOIN ...
       ON ...
    WHERE ...
      AND ...
   ```
    * SELECT 명령어 다음에 줄을 바꾼다. 빈 공간에는 힌트절을 적는다. 힌트절을 적지 않거나 조회하는 컬럼의 수가 적을 경우에는 바로 붙여쓰기도 한다.
    * 각 컬럼을 하나의 행으로 구분하고 첫 컬럼을 제외한 나머지 컬럼은 콤마(,)를 앞에 붙인다.
    * 필드는 들여쓰기 (4칸)의 규칙을 무시하고 상위 필드와 동일한 라인에 맞도록 정렬한다,
   ```sql
      SELECT 
             COLUMN1
           , COLUMN2
      ```
    * 모든 경우에 해당되지는 않지만, 아래의 경우는 항상 공백을 사용한다.
        + 등호(=) 전, 후
        + 쉼표(,) 뒤
    * <> 보다 !=를 사용한다.
    * Alias 사용시 항상 AS 를 명시하며 의미있는 명칭 또는 축약어을 부여한다.
        + A, B, C... 비추천
        + TB_PGM_BAS -> PGM_BAS
    ```sql
    SELECT 
           PGM_BAS.PGM_ID
         , PGM_BAS.PGM_NM
      FROM TB_PGM_BAS AS PGM_BAS
     WHERE 1=1 
       AND PGM_BAS.PGM_ID != 'AA'
    ```
    * subquery 일 경우에만 짧은 문장이면 위의 규칙을 어기고 1줄로 표현한다.
    ```sql
    SELECT 
           SAMPLE1.COLUMN1
         , (SELECT 1 FROM DUAL WHERE 1 IS NOT NULL) AS COLUMN2
      FROM SAMPLE1 AS SAMPLE1 
     WHERE 1=1 
    ```
2. 표현식
    * 괄호 앞에는 띄어쓰기를 추가해 준다.
    * 콤마 뒤에는 띄어쓰기를 추가해 준다.
    * 연산자 앞, 뒤는 띄어쓰기를 추가해 준다.
   ```sql
   SELECT (COLUMN1, COLUMN2, COLUMN3)
   
   SELECT (COLUMN1 + COLUMN2 + COLUMN3)
   ```
3. WHERE절
    * WHERE 절은 ```<where>``` 태그 사용 또는 1=1 을 항상 명시한다.
    * AND와 OR는 콤마와 같이 항상 왼쪽에 배치한다.
    * AND와 OR를 섞어서 사용할 때, 순서에 의존하지 말고 괄호로 묶어준다.
   ```sql
   SELECT 
          COLUMN1
        , COLUMN2
     FROM TABLE 
    WHERE 1=1 
      AND (COLUMN1 = 'AA' OR COLUMN2 = 'BB')
   
   또는
   
   SELECT 
          COLUMN1
        , COLUMN2
     FROM TABLE 
   <where>
      AND (COLUMN1 = 'AA' OR COLUMN2 = 'BB')
   </where>
   ```
4. GROUP BY와 ORDER BY절
    * 공통의 절의 요소 배치속성과 동일하며 요소는 필드와 동일한 형식으로 작성한다.
    * 내림차순(DESC)은 생략한다.
   ```sql
   SELECT 
          COLUMN1
        , COLUMN2 
     FROM TABLE 
    GROUP BY COLUMN1
           , COLUMN2
    ORDER BY COLUMN1 
           , COLUMN2 ASC
   ```
5. UPDATE문
    * SET에 포함되는 = 는 동일한 위치에 있도록 정렬한다.
   ```sql
       UPDATE TABLE
          SET COLUMN1 = #{column1}
            , COLUMN2 = #{column2}
            , COLUMN3 = #{column3}
    ```
6. WITH절
    * WITH문은 쿼리의 상단에 배치한다.
    * WITH문의 이름은 명확하고 간결하게 생성하며 `W_` 를 접두어로 사용한다.
    * WITH 절 다음에 나오는 이름은 새로운 줄에 쓰지 않고 붙여쓴다.
    * WITH 절 내의 쿼리는 새로운 줄에서 4칸 들여쓰기를 적용하여 진행한다,
    * 중복되는 기능의 WITH문은 <sql> 태그를 활용한다.
    ```html
    WITH W_ROLE_ID_DATA AS (
        SELECT ROLE_ID 
          FROM TB_ROLE_BAS 
         WHERE CO_ID = 'IT'
    )
    
    SELECT ROLE_ID
      FROM W_ROLE_ID_DATA 
    
    또는
    
    <sql id="W_ROLE_ID_DATA">
        WITH W_ROLE_ID_DATA AS (
            SELECT ROLE_ID 
              FROM TB_ROLE_BAS 
             WHERE CO_ID = 'IT'
        )
    </sql>
    
    <include refid="W_ROLE_ID_DATA" />
    SELECT ROLE_ID
      FROM W_ROLE_ID_DATA
    ```
    * WITH 절이 여러개가 등록될 경우, 공통의 섹션 요소 정렬 과 같이 정렬한다.
    ```sql
    WITH W_ROLE_ID_DATA1 AS (
        SELECT ROLE_ID 
          FROM TB_ROLE_BAS 
         WHERE CO_ID = 'IT'
    )
       , W_ROLE_ID_DATA2 AS (
        SELECT ROLE_ID 
          FROM TB_ROLE_BAS 
         WHERE CO_ID = 'IT'
    )   
    ```
7. JOIN
    * JOIN은 FROM 절의 첫 테이블과 동일한 인덴트를 갖는다.
    * 별칭에는 항상 AS 를 붙인다.
    * 조인을 사용할 땐, 어떤 조인인지 명시하도록 한다,
    * 단, 조인은 아래와 같이 간단히 명시한다,
        * INNER JOIN -> JOIN
        * LEFT OUTER JOIN -> LEFT JOIN
    * ON절은 JOIN과 동일한 라인에 맞도록 정렬한다,
   ```sql
   SELECT 
          T1.COLUMN1
        , T2.COLUMN1
        , T3.COLUMN1
     FROM TABLE1 AS T1
     JOIN TABLE2 AS T2
       ON T1.KEY = T2.KEY
     LEFT JOIN TABLE3 AS T3
       ON T1.KEY = T3.KEY
   ```
8. Subquery
    * 여는 괄호는 해당 줄의 가장 마지막에 위치하며 괄호 안의 쿼리는 개행 후 4칸 들여쓰기 한다.
    * 닫는 괄호는 Subquery의 SELECT와 같은위치에 맞춘다.
   ```sql
   WITH WITH_DATA AS (
   ....SELECT 
              COLUMN1
            , COLUMN2
            , COLUMN3
            , COLUMN4
         FROM TABLE1
        WHERE 1=1
          AND COLUMN1 IN (
   ........SELECT COLUMN1
             FROM TABLE2
            WHERE 1=1
              AND COLUMN1 IN (1, 2, 3, 4, 5)
   ........)
   )
   
   SELECT (
   ....SELECT 1 
         FROM DUAL 
        WHERE 1=1 
          AND 1 IS NOT NULL
   ....) AS X
     FROM (
   ....SELECT 1 
         FROM DUAL 
        WHERE 1=1 
          AND 1 IS NOT NULL
   ....) AS Y
    WHERE (
   ....SELECT TRUE 
         FROM DUAL
        WHERE 1=1 
          AND 1 IS NOT NULL
   ....)
   ```
9. CASE문
    * WHEN은 CASE문 다음 줄에 표현하며 4칸 들여쓰기를 적용한다.
    * THEN은 WHEN 다음에 한칸 공백후 표현하며 만약 조건이 길 경우 다음 줄에 4칸 들여쓰기를 적용한다.
    * ELSE는 WHEN과 동일한 위치에 정렬한다.
    * END는 CASE와 동일한 위치에 정렬한다.
   ```sql
   CASE 
   ....WHEN COLUMN1 = 'Y' THEN 'YES'
   ....WHEN COLUMN2 IS NOT NULL AND COLUMN2 = 'Y'
   ........THEN 'NO'
   ....ELSE 'EMPTY'
   END
   ```
    * 만약 case 문이 짧다면 위의 규칙을 생략한다.
   ```sql
   SELECT 
          COLUMN1
        , COLUMN2
        , COLUMN3
        , CASE WHEN COLUMN4 < 0 THEN 'Y' ELSE 'N' END AS COLUMN4_VALUE
        , CASE 
          ....WHEN COLUMN5 = '1' THEN 'A'
          ....WHEN COLUMN5 = '2' THEN 'B'
          ....WHEN COLUMN5 = '3' THEN 'C'
          ....WHEN COLUMN5 IS NOT NULL AND COLUMN5 = ''
          ........THEN 'X'
          ....ELSE NULL
          END AS COLUMN5_VALUE
   ```
    * CASE문을 단순화 할 수 있으면 단순화 하도록 한다.
   ```sql
   CASE COLUMN1 
   ....WHEN 1 THEN 'A' 
   ....WHEN 2 THEN 'B'
   ....WHEN 3 THEN 'C'
   ....WHEN 4 THEN 'D'
   END AS COLUMN1_SIMPLE
   ```
10. Window Function
    * 각 절마다 하나씩 여러줄로 분할하여 정렬한다.
    ```sql
    SELECT 
           RANK() 
           OVER (
           ....PARTITION BY COLUMN1, COLUMN2, COLUMN3 
           ....ORDER BY COLUMN1, COLUMN2 ASC
           ) AS RANK_NUM
      FROM TABLE
    ```