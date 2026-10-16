# infra-ci

여러 저장소가 함께 쓰는 GitLab CI 템플릿. 직접 배포하는 것은 없고, 다른 저장소가 **태그로 고정해서** `include` 한다 (GitLab_컨벤션 9절).

```yaml
include:
  - project: tetra/infra/infra-ci
    ref: v0.1.0
    file: templates/flyway.yml
```

| 템플릿 | 쓰는 곳 | 하는 일 |
|---|---|---|
| `templates/flyway.yml` | `tetra/app/db-schema` | MR: 마이그레이션 파일 검사 · main: 공용 `tetra_platform` + 테넌트 `t_*` 스키마에 Flyway info → migrate(수동) |

## 규칙

- 템플릿을 바꾸면 **새 태그**(`vX.Y.Z`)를 붙이고, 쓰는 저장소가 `ref`를 올려서 받아 간다. 이미 붙인 태그는 옮기지 않는다.
- 이미지는 태그를 고정한다(`latest` 금지). 내려받는 파일은 해시를 검증한다.
- 쓰기 권한이 필요한 job은 `tags: [protected]`(보호 Runner — 보호 브랜치 · 태그에서만), 나머지는 `tags: [general]`.
- 비밀값은 템플릿 · 저장소에 넣지 않는다. 쓰는 쪽 프로젝트의 CI/CD 변수(Protected · Masked)나 Secrets Manager.
