# 외교부 공지사항 OpenAPI 기술 문서

> 문서 출처: 외교부 공지사항 API 기술 문서 v1.3
> 인터페이스: REST (GET)
> 데이터 포맷: JSON (또는 XML)
> 데이터 갱신주기: 수시

---

## 목차

- [1. API 서비스 명세](#1-api-서비스-명세)
- [2. 보안 / 표준 / 배포 정보](#2-보안--표준--배포-정보)
- [3. 상세기능 목록](#3-상세기능-목록)
- [4. 공지사항 목록 조회 API](#4-공지사항-목록-조회-api-getnoticelist2)
- [5. 요청 메시지 명세](#5-요청-메시지request-명세)
- [6. 응답 메시지 명세](#6-응답-메시지response-명세)
- [7. 요청 / 응답 예제](#7-요청--응답-예제)
- [8. OpenAPI 에러 코드](#8-openapi-에러-코드)
- [9. 빠른 사용 가이드](#9-빠른-사용-가이드)
- [10. PHP 연동 예제](#10-php-연동-예제)
- [11. JavaScript 연동 예제](#11-javascript-연동-예제)

---

## 1. API 서비스 명세

### 1.1 공지사항

#### 1) API 서비스 개요

**API 서비스 정보**

- **API명(국문)**: 공지사항
- **API명(영문)**: `NoticeService2`
- **API 설명**
  - **공지사항 목록 조회**: 출국 전 참고해야 할 공지사항 목록 조회
  - 출국 전 꼭 참고해야 할 **공지사항 목록 및 상세 정보**를 제공하는 공공데이터 API 서비스

---

## 2. 보안 / 표준 / 배포 정보

### 2.1 보안

- **서비스 인증/권한**
  - 서비스 Key: **사용**
  - 인증서(GPKI): 미사용
  - Basic(ID/PW): 미사용
  - 없음: 미사용
- **메시지 레벨 암호화**
  - 전자서명: 없음
  - 암호화: 없음
- **전송 레벨 암호화**
  - SSL: 없음

### 2.2 인터페이스 / 교환 데이터 표준

- **인터페이스 표준**
  - SOAP 1.2: 미사용
  - **REST (GET): 사용**
  - RSS/Atom: 미사용
- **교환 데이터 표준**
  - XML: 선택
  - **JSON: 사용**

### 2.3 API 서비스 배포 정보

- **서비스 URL**: `http://apis.data.go.kr/1262000/NoticeService2`
- **서비스 WADL**: `http://apis.data.go.kr/1262000/NoticeService2`
- **서비스 버전**: 1.0
- **서비스 시작일**: 2020-12-24
- **서비스 배포일**: 2020-12-24
- **서비스 이력**
  - 2020-12-24: 서비스 시작
- **메시지 교환 유형**
  - Request-Response: 사용
- **최대 처리**
  - 평균 응답 시간: 500 ms
  - 초당 최대 트랜잭션: 30 tps
- **최대 메시지 사이즈**: 4000 bytes

---

## 3. 상세기능 목록

| 일련번호 | API명(국문) | 상세기능명(영문) | 상세기능명(국문) |
|---:|---|---|---|
| 1 | 공지사항 | `getNoticeList2` | 공지사항 목록 조회 |

---

## 4. 공지사항 목록 조회 API (`getNoticeList2`)

### 4.1 기능 설명

- **상세기능 유형**: 조회(목록)
- **상세기능명(국문)**: 공지사항 목록 조회
- **상세기능 설명**: 출국 전 참고해야 할 공지사항 목록 조회

### 4.2 Endpoint

- **Call Back URL**
  - `http://apis.data.go.kr/1262000/NoticeService2/getNoticeList2`

- **HTTP Method**
  - `GET`

---

## 5. 요청 메시지(Request) 명세

### 5.1 Query Parameters

> 항목구분: 필수(1), 옵션(0)

| 파라미터(영문) | 항목명(국문) | 크기 | 구분 | 샘플 | 설명 |
|---|---|---:|---:|---|---|
| `serviceKey` | 인증키 | 100 | 1 | (공공데이터포털 발급 키) | 공공데이터포털에서 발급받은 인증키 (**URL Encode 필요**) |
| `returnType` | 데이터표출방식 | 4 | 0 | `JSON` | `XML` 또는 `JSON` |
| `numOfRows` | 한 페이지 결과 수 | 4 | 1 | `10` | 한 페이지 결과 수 |
| `pageNo` | 페이지 번호 | 4 | 1 | `1` | 페이지 번호 |

---

## 6. 응답 메시지(Response) 명세

### 6.1 Response Fields

> 항목구분: 필수(1), 옵션(0)

| 필드(영문) | 항목명(국문) | 크기 | 구분 | 샘플 | 설명 |
|---|---|---:|---:|---|---|
| `resultCode` | 결과코드 | 3 | 1 | `0` | 결과코드 |
| `resultMsg` | 결과메시지 | 50 | 1 | `정상` | 결과메시지 |
| `numOfRows` | 한 페이지 결과 수 | 4 | 1 | `10` | 한 페이지 결과 수 |
| `pageNo` | 페이지 번호 | 4 | 1 | `1` | 페이지 번호 |
| `totalCount` | 전체 결과 수 | 20 | 1 | `1268` | 전체 결과 수 |
| `currentCount` | 현재 결과 수 | 20 | 1 | `10` | 현재 결과 수 |
| `data[].id` | 공지사항ID | 20 | 0 | `ATC0000000007730` | 공지사항ID |
| `data[].title` | 제목 | 2000 | 0 | (문자열) | 제목 |
| `data[].txt_origin_cn` | 텍스트원본내용 | 4000 | 1 | (문자열) | 텍스트 원본 내용(HTML 엔티티/태그 포함 가능) |
| `data[].file_download_url` | 파일경로 | 4000 | 0 | (URL/문자열) | 첨부 파일 다운로드 경로 |
| `data[].written_dt` | 작성일 | 8 | 0 | `2020.03.12` 또는 `2020-08-26` | 작성일 |

> 참고: `txt_origin_cn`에는 `&nbsp;` 같은 HTML 엔티티가 포함될 수 있다.

---

## 7. 요청 / 응답 예제

### 7.1 REST(URI) 예제

```bash
curl "http://apis.data.go.kr/1262000/NoticeService2/getNoticeList2?serviceKey=인증키(URL Encode)&pageNo=1&numOfRows=10&returnType=JSON"
```

### 7.2 JSON 응답 예제

```json
{
  "currentCount": "1",
  "data": [
    {
      "file_download_url": "",
      "id": "ATC0000000007730",
      "title": "중국 베이징 수도공항 국제선 노선 변경 보도 관련 공지",
      "txt_origin_cn": "베이징 수도공항 국제선 노선 변경 보도 관련 공지○ 최근 중국 일부&nbsp;중국민용항공국이 ... 끝.&nbsp;",
      "written_dt": "2020-08-26"
    }
  ],
  "numOfRows": 10,
  "pageNo": 1,
  "resultCode": 0,
  "resultMsg": "정상",
  "totalCount": 225
}
```

---

## 8. OpenAPI 에러 코드

| 에러코드 | 에러메세지 | 설명 |
|---:|---|---|
| 0 | OK | 정상 |
| -1 | 시스템 내부 오류가 발생하였습니다. | 시스템 에러 |
| -2 | 요청하신 파라미터가 적합하지 않습니다. | 잘못된 요청 및 파라미터 에러 |
| -3 | 등록되지 않은 서비스입니다. | 존재하지 않는 서비스 요청 에러 |
| -4 | 등록되지 않은 인증키입니다. | 등록되지 않은 인증키 사용 에러 |
| -9 | 종료된 서비스입니다. | 종료된 서비스 요청 에러 |
| -10 | 트래픽 허용 횟수를 초과하였습니다. | 호출 트래픽 초과 에러 |
| -401 | 유효하지 않은 인증키입니다. | 유효하지 않은 인증키 사용 에러 |
| -999 | UNKNOWN | 알 수 없는 에러 |

---

## 9. 빠른 사용 가이드

1. 공공데이터포털에서 발급받은 `serviceKey`를 준비
2. `serviceKey`는 반드시 **URL Encode** 해서 요청
3. `pageNo`, `numOfRows`로 페이지네이션
4. 응답의 `resultCode`가 `0`인지 확인
5. `data[]` 배열에서 공지사항 목록을 읽고, `txt_origin_cn`의 HTML 엔티티를 필요에 따라 디코딩/정리

---

## 10. PHP 연동 예제

```php
<?php
/**
 * 외교부 공지사항 API 조회 예제
 *
 * @param string $serviceKey 공공데이터포털에서 발급받은 인증키
 * @param int $pageNo 페이지 번호 (기본값: 1)
 * @param int $numOfRows 페이지당 결과 수 (기본값: 10)
 * @return array 공지사항 목록 또는 에러 정보
 */
function get_mofa_notices(string $serviceKey, int $pageNo = 1, int $numOfRows = 10): array {
    // API 엔드포인트
    $endpoint = 'http://apis.data.go.kr/1262000/NoticeService2/getNoticeList2';

    // 요청 파라미터 구성
    $params = [
        'serviceKey' => $serviceKey,  // URL Encode는 http_build_query에서 자동 처리
        'pageNo' => $pageNo,
        'numOfRows' => $numOfRows,
        'returnType' => 'JSON'
    ];

    // URL 생성
    $url = $endpoint . '?' . http_build_query($params);

    // cURL로 API 호출
    $ch = curl_init();
    curl_setopt_array($ch, [
        CURLOPT_URL => $url,
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_TIMEOUT => 30,
        CURLOPT_HTTPHEADER => ['Accept: application/json']
    ]);

    $response = curl_exec($ch);
    $httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
    $error = curl_error($ch);
    curl_close($ch);

    // cURL 에러 처리
    if ($error) {
        return ['resultCode' => -1, 'resultMsg' => 'cURL Error: ' . $error];
    }

    // HTTP 에러 처리
    if ($httpCode !== 200) {
        return ['resultCode' => -1, 'resultMsg' => 'HTTP Error: ' . $httpCode];
    }

    // JSON 파싱
    $data = json_decode($response, true);
    if (json_last_error() !== JSON_ERROR_NONE) {
        return ['resultCode' => -1, 'resultMsg' => 'JSON Parse Error: ' . json_last_error_msg()];
    }

    return $data;
}

/**
 * HTML 엔티티를 정리하는 헬퍼 함수
 *
 * @param string $text HTML 엔티티가 포함된 텍스트
 * @return string 정리된 텍스트
 */
function clean_notice_content(string $text): string {
    // HTML 엔티티 디코딩
    $text = html_entity_decode($text, ENT_QUOTES | ENT_HTML5, 'UTF-8');
    // HTML 태그 제거 (선택사항)
    $text = strip_tags($text);
    // 연속된 공백 정리
    $text = preg_replace('/\s+/', ' ', $text);
    return trim($text);
}

// 사용 예시
$serviceKey = 'YOUR_SERVICE_KEY_HERE';
$result = get_mofa_notices($serviceKey, 1, 10);

if ($result['resultCode'] === 0) {
    echo "총 {$result['totalCount']}개의 공지사항이 있습니다.\n";
    echo "현재 페이지: {$result['pageNo']}, 표시 개수: {$result['currentCount']}\n\n";

    foreach ($result['data'] as $notice) {
        echo "ID: {$notice['id']}\n";
        echo "제목: {$notice['title']}\n";
        echo "작성일: {$notice['written_dt']}\n";
        echo "내용: " . clean_notice_content($notice['txt_origin_cn']) . "\n";
        if (!empty($notice['file_download_url'])) {
            echo "첨부파일: {$notice['file_download_url']}\n";
        }
        echo "---\n";
    }
} else {
    echo "에러 발생: [{$result['resultCode']}] {$result['resultMsg']}\n";
}
```

---

## 11. JavaScript 연동 예제

```javascript
/**
 * 외교부 공지사항 API 조회 함수
 *
 * @param {string} serviceKey - 공공데이터포털에서 발급받은 인증키
 * @param {number} pageNo - 페이지 번호 (기본값: 1)
 * @param {number} numOfRows - 페이지당 결과 수 (기본값: 10)
 * @returns {Promise<object>} 공지사항 목록 또는 에러 정보
 */
async function getMofaNotices(serviceKey, pageNo = 1, numOfRows = 10) {
    const endpoint = 'http://apis.data.go.kr/1262000/NoticeService2/getNoticeList2';

    // 요청 파라미터 구성
    const params = new URLSearchParams({
        serviceKey: serviceKey,
        pageNo: pageNo.toString(),
        numOfRows: numOfRows.toString(),
        returnType: 'JSON'
    });

    const url = `${endpoint}?${params.toString()}`;

    try {
        const response = await fetch(url);

        if (!response.ok) {
            throw new Error(`HTTP Error: ${response.status}`);
        }

        const data = await response.json();
        return data;
    } catch (error) {
        return {
            resultCode: -1,
            resultMsg: error.message
        };
    }
}

/**
 * HTML 엔티티를 정리하는 헬퍼 함수
 *
 * @param {string} text - HTML 엔티티가 포함된 텍스트
 * @returns {string} 정리된 텍스트
 */
function cleanNoticeContent(text) {
    // HTML 엔티티 디코딩
    const textarea = document.createElement('textarea');
    textarea.innerHTML = text;
    let cleaned = textarea.value;

    // HTML 태그 제거
    cleaned = cleaned.replace(/<[^>]*>/g, '');

    // 연속된 공백 정리
    cleaned = cleaned.replace(/\s+/g, ' ').trim();

    return cleaned;
}

// 사용 예시
(async () => {
    const serviceKey = 'YOUR_SERVICE_KEY_HERE';
    const result = await getMofaNotices(serviceKey, 1, 10);

    if (result.resultCode === 0) {
        console.log(`총 ${result.totalCount}개의 공지사항이 있습니다.`);
        console.log(`현재 페이지: ${result.pageNo}, 표시 개수: ${result.currentCount}\n`);

        result.data.forEach((notice, index) => {
            console.log(`[${index + 1}] ${notice.title}`);
            console.log(`    ID: ${notice.id}`);
            console.log(`    작성일: ${notice.written_dt}`);
            console.log(`    내용: ${cleanNoticeContent(notice.txt_origin_cn).substring(0, 100)}...`);
            if (notice.file_download_url) {
                console.log(`    첨부파일: ${notice.file_download_url}`);
            }
            console.log('');
        });
    } else {
        console.error(`에러 발생: [${result.resultCode}] ${result.resultMsg}`);
    }
})();
```

### Vue.js 컴포넌트 예제

```javascript
/**
 * 외교부 공지사항 Vue.js 컴포넌트
 * Options API 사용
 */
function MofaNoticeComponent() {
    return {
        template: `
            <div class="mofa-notices">
                <h3>외교부 공지사항</h3>

                <div v-if="loading" class="text-center py-4">
                    <span class="spinner-border spinner-border-sm"></span>
                    로딩 중...
                </div>

                <div v-else-if="error" class="alert alert-danger">
                    {{ error }}
                </div>

                <template v-else>
                    <p class="text-muted">
                        총 {{ totalCount }}개 중 {{ notices.length }}개 표시
                    </p>

                    <div class="list-group">
                        <div
                            v-for="notice in notices"
                            :key="notice.id"
                            class="list-group-item"
                        >
                            <h5 class="mb-1">{{ notice.title }}</h5>
                            <small class="text-muted">{{ notice.written_dt }}</small>
                            <p class="mb-1 mt-2">{{ cleanContent(notice.txt_origin_cn) }}</p>
                            <a
                                v-if="notice.file_download_url"
                                :href="notice.file_download_url"
                                target="_blank"
                                class="btn btn-sm btn-outline-primary"
                            >
                                첨부파일 다운로드
                            </a>
                        </div>
                    </div>

                    <nav v-if="totalPages > 1" class="mt-3">
                        <ul class="pagination justify-content-center">
                            <li
                                class="page-item"
                                :class="{ disabled: pageNo <= 1 }"
                            >
                                <a
                                    class="page-link"
                                    href="#"
                                    @click.prevent="loadPage(pageNo - 1)"
                                >
                                    이전
                                </a>
                            </li>
                            <li class="page-item active">
                                <span class="page-link">{{ pageNo }} / {{ totalPages }}</span>
                            </li>
                            <li
                                class="page-item"
                                :class="{ disabled: pageNo >= totalPages }"
                            >
                                <a
                                    class="page-link"
                                    href="#"
                                    @click.prevent="loadPage(pageNo + 1)"
                                >
                                    다음
                                </a>
                            </li>
                        </ul>
                    </nav>
                </template>
            </div>
        `,

        props: {
            serviceKey: {
                type: String,
                required: true
            },
            numOfRows: {
                type: Number,
                default: 10
            }
        },

        data() {
            return {
                notices: [],
                loading: false,
                error: null,
                pageNo: 1,
                totalCount: 0
            };
        },

        computed: {
            totalPages() {
                return Math.ceil(this.totalCount / this.numOfRows);
            }
        },

        methods: {
            async loadNotices() {
                this.loading = true;
                this.error = null;

                try {
                    const result = await getMofaNotices(
                        this.serviceKey,
                        this.pageNo,
                        this.numOfRows
                    );

                    if (result.resultCode === 0) {
                        this.notices = result.data;
                        this.totalCount = result.totalCount;
                    } else {
                        this.error = `에러: [${result.resultCode}] ${result.resultMsg}`;
                    }
                } catch (e) {
                    this.error = '데이터를 불러오는 중 오류가 발생했습니다.';
                } finally {
                    this.loading = false;
                }
            },

            loadPage(page) {
                if (page < 1 || page > this.totalPages) return;
                this.pageNo = page;
                this.loadNotices();
            },

            cleanContent(text) {
                return cleanNoticeContent(text).substring(0, 200) + '...';
            }
        },

        mounted() {
            this.loadNotices();
        }
    };
}

// 사용 예시
// ready(() => {
//     Vue.createApp({
//         components: {
//             'mofa-notice': MofaNoticeComponent()
//         }
//     }).mount('#app');
// });
```
