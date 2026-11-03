# NAVER SA + GFA 자동화 보고서 v3

**GitHub Pages 배포 + 브라우저 자동 저장 + 비공개 GitHub 저장소 백업**

## 1. 바로 배포 (GitHub Pages)

1. GitHub에서 **새 저장소**를 만듭니다. 예: `naver-ads-report-site`.
2. 이 ZIP을 풀고 **`index.html` 파일을 저장소 루트에 업로드**합니다. (`sample_csv/`과 이 README는 선택사항)
3. 저장소의 **Settings → Pages → Build and deployment → Deploy from a branch**를 선택합니다.
4. `main` / `/(root)` → **Save**를 선택합니다.
5. 배포 후 주소 `https://계정명.github.io/naver-ads-report-site/`에서 접속합니다.

> 사이트 저장소는 디자인/코드 전용입니다. 원본 광고 성과 데이터는 절대 사이트 저장소에 직접 넣지 마세요. GitHub Pages 페이지는 링크를 아는 사람이 열 수 있다고 가정하고, 실제 데이터는 별도 비공개 저장소에 보관하십시오.

## 2. 로우파일 업로드 및 로컬 자동 저장

1. 페이지 오른쪽 위에서 **로우파일 업로드**를 누릅니다.
2. 필요에 따라 다음을 올립니다. SA 일별 캠페인 / GFA 일별 캠페인 / SA 키워드 / GFA 소재.
3. `.csv` / `.tsv` / `.xlsx`를 지원합니다. 데이터는 현재 브라우저 **IndexedDB**에 즉시 저장됩니다. 새로고침해도 유지됩니다.
4. 같은 유형의 파일을 다시 올리면 해당 유형의 데이터가 **교체**됩니다. 기간별 누적 추가(append)는 지원하지 않습니다.
5. **광고주 추가**에서 이름(한글 가능)과 영문 저장 ID를 만든 뒤 광고주를 바꾸면 서로 분리된 데이터를 관리할 수 있습니다.

예제 파일: `sample_csv/` (가상 데이터, 4개 파일). 처음에는 예제 데이터를 통해 작동 테스트를 권장합니다.

필수 열: `날짜, 광고비, 노출수, 클릭수, 구매전환수, 전환매출액`; 캠페인 파일에는 `캠페인명`, 키워드 파일에는 `키워드`, 소재 파일에는 `소재명`을 추가하세요. `.xlsx`는 표 형태 시트의 일반적인 ZIP/XML 구조를 브라우저에서 읽으며 암호화된 Excel, 구형 `.xls`, 피벗 구조는 지원하지 않습니다. 헤더는 35행 내에 있어야 합니다. 네이버 원본에 실제 존재하는 전환 지표와 매출 지표가 다른 경우 헤더 매핑을 조정해야 합니다.

**이전 기간 비교가 비어 있으면** 비교 기간에 대한 데이터가 없는 것입니다. 전월·전주 비교를 위해 과거 날짜까지 포함한 파일을 올리세요.

## 3. GitHub 원본 백업 (선택, 실제 계정 필요)

GitHub Pages에서 공개 사이트 저장소와 **별도로 비공개(private) 데이터 저장소**를 하나 만듭니다. 예: `naver-ads-private-data`.

1. GitHub **Settings → Developer settings → Personal access tokens → Fine-grained tokens**에서 새 토큰을 발급합니다.
2. Repository access를 **Only select repositories**로 제한하고, `naver-ads-private-data`만 지정합니다.
3. Repository permissions에서 **Contents: Read and write**만 부여합니다. 토큰 기한을 짧게 설정하고 절대 코드·채팅·저장소에 붙여 넣지 마세요.
4. 대시보드의 **GitHub 연결 및 백업**에서 GitHub 계정명, 비공개 저장소명, 토큰을 입력하고 **연결 확인**을 누릅니다. 브랜치는 비워 두면 저장소 기본 브랜치를 사용합니다.
5. **현재 광고주 백업**을 눌러 백업하거나, '업로드 직후 자동 백업'을 체크해 두면 이후 파일 업로드 때마다 자동 백업됩니다.
6. 다른 PC에서 같은 보고서 페이지를 열고 동일한 광고주 저장 ID를 생성한 다음 GitHub에 연결해 **GitHub 데이터 불러오기**를 누르면 그 광고주의 데이터가 복원됩니다.

데이터 경로 구조:

```
report-data/
  advertiser-id/
    snapshot.json          # 정규화된 성과 데이터 / 파일 정보
    raw/
      SA.bin  # 원본 파일 바이트, 파일명은 snapshot.json에 기록
      GFA.bin
      KEYWORD.bin
      CREATIVE.bin
```

GitHub 백업은 업로드 직후 전체 데이터 스냅샷과 원본 로우파일들을 **하나의 커밋으로 저장**합니다. 중복 집계가 일어나지 않도록 캠페인 파일이 있으면 매체별 합계는 캠페인 파일을 우선합니다. 토큰이 연결되지 않았어도 브라우저 저장은 자동으로 작동합니다.

### 저장 범위와 제한

- 브라우저 저장은 동일 브라우저/동일 웹사이트 원본(origin)에 귀속됩니다. 시크릿 모드·브라우저 데이터 삭제·다른 PC로 이동하면 사라질 수 있습니다. 따라서 중요한 파일은 GitHub 백업을 병행하세요.
- GitHub 연결은 **현재 탭에서만** 유지되며, 새로고침·종료하면 다시 토큰을 입력해야 합니다. 브라우저에 PAT를 영구 저장하지 않습니다.
- 개인정보·광고주 데이터를 공개 저장소에 업로드하지 않도록 GitHub 연결 시 `private=true`를 확인합니다.
- 현재 **파일당 20MB**, 정규화 데이터 JSON **20MB**, 로컬 단일 파일 **30MB** 제한이 있습니다. 너무 큰 파일은 필요한 기간을 분리해 관리하세요.
- `GitHub 데이터 불러오기`는 현재 광고주 로컬 데이터를 **덮어씁니다**. 이전 업로드 버전이 필요하면 GitHub의 커밋 기록을 이용하세요.
- 클라이언트 웹페이지에 PAT를 직접 입력하는 방식은 **내부 관리자용**입니다. 광고주에게 계정별 권한을 주거나 로그인 없이 실데이터를 안전하게 공유하려면 서버/인증/DB(예: 별도 API + 접근제어)가 추가로 필요합니다.
- HTML/JS 소스에 **토큰을 직접 삽입하지 마세요**. GitHub Pages 공개 사이트의 코드는 누구나 열어볼 수 있습니다.

## 4. 화면에 포함된 분석

전체 광고비·매출·ROAS·클릭·구매전환·CPC·CTR·CVR, 조회/비교 기간, SA와 GFA 비교, 캠페인 TOP, 우수·저조 키워드, GFA 우수·저조 소재, 설정 가능한 최소 광고비/전환수/목표 ROAS, 자동 분석 인사이트, PDF 인쇄, 요약 복사.

**주의:** 기본 목표 ROAS 350% / 최소 광고비 5만원 / 최소 전환 3건은 시연 기본값이며 실제 광고주의 KPI에 맞게 변경하세요. SA 검색어 보고서와 등록 키워드 보고서는 별개이며, 혼합 업로드 시 지표가 달라질 수 있습니다. SA/GFA의 전환 창, VAT, 구매전환 기준을 통일해야 합니다. 비교 기간에 날짜 수가 다를 수 있으므로 총비용·매출 비교를 해석할 때 기간 커버리지를 확인하세요.

## 5. 검증

`sample_csv/`의 4개 파일을 올리고 새로고침 → 저장 데이터가 그대로 표시되는지 확인합니다. 광고주 추가 후 두 프로필에 다른 파일을 올려 혼합되지 않는지 확인합니다. 비공개 GitHub 저장소에 연결하고 백업 후 GitHub 저장소에서 `report-data/...` 커밋과 파일이 생성되는지 확인합니다.

GitHub 공식 문서:
- Pages: https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site
- Contents API: https://docs.github.com/en/rest/repos/contents
- Git Data: https://docs.github.com/en/rest/git
- PAT: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens
