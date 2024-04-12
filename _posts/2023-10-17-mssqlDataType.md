---
title : MSSQL의 데이터 타입
date : 2023-10-14 10:31
categories : [db, mssql]
tags : [mssql]
---

안녕하세요 개발구리🐸 입니다

MsSQL의 데이터 타입에 대해 알아보겠습니다.

<table>
    <tr>
        <th>데이터 형식</th>
        <th>유형</th>
        <th>자료형</th>
        <th>설명</th>
    </tr>
    <tr>
        <td rowspan="9">정확한 수치</td>
        <td rowspan="4">정수</td>
        <td>TINYINT</td>
        <td>1바이트</td>
    </tr>
    <tr>
        <td>SMALLINT</td>
        <td>2바이트</td>
    </tr>
    <tr>
        <td>INT</td>
        <td>4바이트</td>
    </tr>
    <tr>
        <td>BIGINT</td>
        <td>8바이트</td>
    </tr>
    <tr>
        <td>정수 (BOOLEAN)</td>
        <td>BIT</td>
        <td>1,0 또는 NULL ( BOOLEAN과 대응)</td>
    </tr>
    <tr>
        <td rowspan="2">고정 소수점</td>
        <td>DECIMAL</td>
        <td rowspan="2">
            전체 자릿수와 소수 자릿수가 고정된 데이터 형식입니다.
            <br>
            금융 관련 데이터와 같이 정확한 수를 취급해야하는 경우 사용될 수 있습니다
            <br>
            NUMERIC과 DECIMAL은 기능적으로 동일합니다
            <br>
            인수를 받아 사용됩니다.
            <br>
            사용 예) decimal(10,8) => 총 10개의 자리수를 가지고 소수점 이하로 8자리를 가지는 숫자
        </td>
    </tr>
    <tr>
        <td>NUMERIC</td>
    </tr>
    <tr>
        <td rowspan="2">통화</td>
        <td>MONEY</td>
        <td rowspan="2">통화를 저장하는데 사용됩니다</td>
    </tr>
    <tr>
        <td>SMALLMONEY</td>
    </tr>
    <tr>
        <td rowspan="2">근사치</td>
        <td rowspan="2">부등 소수점</td>
        <td>FLOAT</td>
        <td>4바이트 혹은 8바이트</td>
    </tr>
    <tr>
        <td>REAL</td>
        <td>4바이트</td>
    </tr>
    <tr>
        <td rowspan="6" colspan="2">날짜 및 시간</td>
        <td>DATE</td>
        <td>날짜<br>YYYY/MM/DD</td>
    </tr>
    <tr>
        <td>TIME</td>
        <td>소수 자릿수를 포함한 24시간제 기준 시간<br>hh:mm:ss</td>
    </tr>
    <tr>
        <td>SMALLDATETIME</td>
        <td>초단위를 반올림하는 24시간제 시간과 날짜
        <br>
        예시)<br> 
        2023-10-17 15:25:29 => 2023-10-17 15:25:00
        <br>
        2023-10-17 15:25:30 => 2023-10-17 15:26:00
        </td>
    </tr>
    <tr>
        <td>DATETIME</td>
        <td>소수 자릿수를 포함한 24시간제 시간과 날짜<br>YYYY-MM-DD hh:mm:ss</td>
    </tr>
    <tr>
        <td>DATETIME2</td>
        <td>
        DATETIME보다 높은 정밀도의 데이터 타입
        <br>
        필요에 따라 DATETIME보다 많은 공간 필요
        </td>
    </tr>
    <tr>
        <td>DATETIMEOFFSET</td>
        <td>DATETIME2에 UTC 기준 시간대를 추가
        <br>
        예시)<br>
        2023-10-17 15:31:27 +09:00
        </td>
    </tr>
    <tr>
        <td colspan="2" rowspan="3">문자열</td>
        <td>CHAR</td>
        <td></td>
    </tr>
    <tr>
        <td>VARCHAR</td>
        <td></td>
    </tr>
    <tr>
        <td>TEXT</td>
        <td></td>
    </tr>

</table>

- MySQL에서는 `TINYINT` 타입이 불리언을 저장하는 데이터타입이지만 MSSQL에서는 `BIT`가 불리언을 저장하는 타입이기 때문에 혼동되기 쉽습니다.