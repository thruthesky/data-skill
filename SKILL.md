---
name: data-skill
description: 본 스킬은 대한민국 공공데이터포털(https://www.data.go.kr/)에서 제공하는 각종 공공데이터 API 를 사용하기 위한 설명입니다. 본 스킬은 공공데이터 개발 또는 공공 API 개발을 할 때 사용하면 됩니다.
---

# 대한민국 공공데이터포털 API 스킬

## 1. 개요

**대한민국 공공데이터포털(data.go.kr)**은 중앙정부, 지자체, 공공기관이 보유한 데이터를 개방·제공하는 공식 플랫폼이다. 데이터는 파일 다운로드 또는 Open API 형태로 제공된다.

### 주요 특징

- **REST API 기반**: HTTP GET/POST 방식 호출
- **응답 형식**: JSON 또는 XML 선택 가능
- **인증 방식**: 서비스키(serviceKey) 기반
- **무료 제공**: 대부분의 API는 무료로 사용 가능

## 2. API 사용 절차

### 2.1 회원가입 및 인증키 발급

1. [data.go.kr](https://www.data.go.kr) 접속
2. 회원가입 (개인/기업 선택 가능)
3. 마이페이지 → 인증키 발급
4. **일반 인증키**와 **운영 인증키** 구분
   - 일반 인증키: 개발/테스트용 (일일 호출 제한 낮음)
   - 운영 인증키: 실제 서비스용 (신청 후 승인 필요)

### 2.2 API 활용 신청

1. 원하는 데이터 검색 (예: "외교부 공지사항", "기상청 날씨")
2. **활용신청** 버튼 클릭
3. 활용 목적 작성 후 신청
4. 즉시 승인 또는 담당자 승인 대기 (API에 따라 다름)

### 2.3 API 호출

```bash
# 기본 호출 형식
curl "http://apis.data.go.kr/{서비스경로}?serviceKey={인증키}&파라미터=값"

# 예시: 외교부 공지사항 목록 조회
curl "http://apis.data.go.kr/1262000/NoticeService2/getNoticeList2?serviceKey=YOUR_KEY&pageNo=1&numOfRows=10&returnType=JSON"
```

## 3. 공통 요청 파라미터

| 파라미터 | 설명 | 필수 | 비고 |
|----------|------|------|------|
| `serviceKey` | 인증키 | Y | **URL Encode 필수** |
| `returnType` | 응답 형식 | N | `JSON` 또는 `XML` (기본값은 API마다 다름) |
| `pageNo` | 페이지 번호 | N | 기본값: 1 |
| `numOfRows` | 페이지당 결과 수 | N | 기본값: API마다 다름 |

### 3.1 인증키 URL Encode

인증키에 특수문자(`+`, `/`, `=` 등)가 포함되어 있으므로 **반드시 URL Encode** 해야 한다.

```php
// PHP 예시
$serviceKey = urlencode($originalKey);
```

```javascript
// JavaScript 예시
const serviceKey = encodeURIComponent(originalKey);
```

## 4. 공통 응답 구조

### 4.1 JSON 응답 예시

```json
{
  "resultCode": 0,
  "resultMsg": "정상",
  "numOfRows": 10,
  "pageNo": 1,
  "totalCount": 225,
  "currentCount": 10,
  "data": [
    { /* 실제 데이터 항목 */ }
  ]
}
```

### 4.2 에러 코드

| 코드 | 메시지 | 설명 |
|------|--------|------|
| `0` | OK | 정상 |
| `-1` | 시스템 내부 오류 | 서버 에러 |
| `-2` | 파라미터 부적합 | 요청 파라미터 오류 |
| `-3` | 등록되지 않은 서비스 | 존재하지 않는 서비스 |
| `-4` | 등록되지 않은 인증키 | 인증키 미등록 |
| `-9` | 종료된 서비스 | 서비스 종료됨 |
| `-10` | 트래픽 초과 | 일일 호출 횟수 초과 |
| `-401` | 유효하지 않은 인증키 | 인증키 오류 |

## 5. PHP 연동 예시

```php
<?php
/**
 * 공공데이터 API 호출 헬퍼 함수
 *
 * @param string $endpoint API 엔드포인트 URL
 * @param array $params 요청 파라미터
 * @return array 응답 데이터
 */
function call_public_api(string $endpoint, array $params): array {
    // 인증키 URL Encode
    if (isset($params['serviceKey'])) {
        $params['serviceKey'] = urlencode($params['serviceKey']);
    }

    // 쿼리스트링 생성
    $queryString = http_build_query($params);
    $url = $endpoint . '?' . $queryString;

    // API 호출
    $ch = curl_init();
    curl_setopt_array($ch, [
        CURLOPT_URL => $url,
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_TIMEOUT => 30,
    ]);

    $response = curl_exec($ch);
    $httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
    curl_close($ch);

    if ($httpCode !== 200) {
        return ['resultCode' => -1, 'resultMsg' => 'HTTP Error: ' . $httpCode];
    }

    return json_decode($response, true) ?? ['resultCode' => -1, 'resultMsg' => 'JSON Parse Error'];
}

// 사용 예시: 외교부 공지사항 조회
$result = call_public_api(
    'http://apis.data.go.kr/1262000/NoticeService2/getNoticeList2',
    [
        'serviceKey' => 'YOUR_SERVICE_KEY',
        'pageNo' => 1,
        'numOfRows' => 10,
        'returnType' => 'JSON'
    ]
);

if ($result['resultCode'] === 0) {
    foreach ($result['data'] as $notice) {
        echo $notice['title'] . "\n";
    }
}
```

## 6. JavaScript 연동 예시

```javascript
/**
 * 공공데이터 API 호출 함수
 *
 * @param {string} endpoint - API 엔드포인트 URL
 * @param {object} params - 요청 파라미터
 * @returns {Promise<object>} 응답 데이터
 */
async function callPublicApi(endpoint, params) {
    // 인증키 URL Encode
    const encodedParams = new URLSearchParams();
    for (const [key, value] of Object.entries(params)) {
        encodedParams.append(key, value);
    }

    const url = `${endpoint}?${encodedParams.toString()}`;

    try {
        const response = await fetch(url);
        if (!response.ok) {
            throw new Error(`HTTP Error: ${response.status}`);
        }
        return await response.json();
    } catch (error) {
        return { resultCode: -1, resultMsg: error.message };
    }
}

// 사용 예시: 외교부 공지사항 조회
const result = await callPublicApi(
    'http://apis.data.go.kr/1262000/NoticeService2/getNoticeList2',
    {
        serviceKey: 'YOUR_SERVICE_KEY',
        pageNo: 1,
        numOfRows: 10,
        returnType: 'JSON'
    }
);

if (result.resultCode === 0) {
    result.data.forEach(notice => {
        console.log(notice.title);
    });
}
```

## 7. 제공 API 레퍼런스

각 API의 상세 기술 문서는 `references/` 폴더에서 확인한다.

| API | 문서 | 설명 |
|-----|------|------|
| 외교부 공지사항 | [mofa-reminder.md](references/mofa-reminder.md) | 출국 전 참고 공지사항 목록 조회 |

### 7.1 외교부 공지사항 API 빠른 참조

- **서비스 URL**: `http://apis.data.go.kr/1262000/NoticeService2/getNoticeList2`
- **HTTP Method**: GET
- **응답 형식**: JSON/XML
- **상세 문서**: [references/mofa-reminder.md](references/mofa-reminder.md)

```bash
# 테스트 호출
curl "http://apis.data.go.kr/1262000/NoticeService2/getNoticeList2?serviceKey=YOUR_KEY&pageNo=1&numOfRows=5&returnType=JSON"
```

## 8. 개발 시 주의사항

1. **인증키 보안**: 인증키를 소스코드에 직접 하드코딩하지 말고 환경변수나 설정 파일로 관리
2. **URL Encode**: serviceKey는 반드시 URL Encode 처리
3. **호출 제한**: 일일 트래픽 제한 확인 (API마다 다름)
4. **에러 처리**: resultCode 확인 후 적절한 에러 처리
5. **캐싱**: 동일한 데이터를 반복 조회할 경우 캐싱 적용 권장
6. **HTTPS**: 일부 API는 HTTP만 지원하므로 Mixed Content 주의

## 9. 새 API 추가 시 문서화 규칙

새로운 공공데이터 API를 연동할 때 `references/` 폴더에 다음 형식으로 문서를 추가한다:

```markdown
# [API명] OpenAPI 기술 문서

## 1. API 서비스 개요
- API명, 설명, 용도

## 2. 보안 / 인증 정보
- 인증 방식, SSL 지원 여부

## 3. Endpoint
- 서비스 URL, HTTP Method

## 4. 요청 파라미터
- 필수/옵션 파라미터 표

## 5. 응답 필드
- 응답 데이터 구조

## 6. 요청/응답 예제
- 실제 호출 예시와 응답 샘플

## 7. 에러 코드
- API 고유 에러 코드 (있을 경우)
```
