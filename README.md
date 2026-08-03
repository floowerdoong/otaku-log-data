# otaku-log-data

스슥(오타쿠 로그) 앱이 읽어가는 공개 데이터. 앱을 업데이트하거나 심사를 받지 않고도
바꿀 수 있는 값만 둔다.

## titles.json / titles_manifest.json

자동완성 사전. 앱이 하루 한 번 매니페스트만 받아 `version`이 올라갔을 때만
`titles.json`을 내려받는다.

**직접 고치지 말 것** — `scripts/titles_admin.py`(조회·편집) → `[저장]` →
`[배포+푸시]`가 `publish_titles.py`를 호출해 여기로 복사하고 `version`을 +1 한다.

## app_manifest.json

앱 업데이트 안내. v1.1.1부터 앱이 켜질 때 한 번 읽는다.

```json
{
  "min_version": "1.0.0",
  "latest_version": "1.1.1",
  "store_url": "https://apps.apple.com/kr/app/id6793930864"
}
```

| 필드 | 뜻 |
|---|---|
| `min_version` | 이 버전 **미만**이면 닫을 수 없는 안내가 뜬다 |
| `latest_version` | 이 버전 **미만**이면 닫을 수 있는 안내가 뜬다 |
| `store_url` | 안내에서 여는 주소. 없으면 앱에 박힌 값으로 폴백 |

**`min_version`은 함부로 올리지 않는다.** 올리는 순간 그 아래 버전 사용자는 앱을
전혀 쓸 수 없다. 데이터가 깨질 위험이 있을 때만 쓰고, 기본은 `latest_version`이다.

**`latest_version`은 스토어에 실제로 올라간 뒤에 올린다.** 심사 통과 전에 올리면
사용자가 앱스토어에 갔는데 새 버전이 없는 상황이 된다.

값을 못 읽거나 형식이 깨지면 앱은 **아무것도 막지 않는다** — 이 파일이 사라져도
앱은 정상 동작한다.

### 사전 배포와 파일을 나눈 이유

`titles_manifest.json`은 `publish_titles.py`가 통째로 덮어쓴다. 같은 파일에 뒀다면
사전을 배포할 때마다 업데이트 게이트가 날아갔을 것이다.
