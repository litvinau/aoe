# aoe

GitHub Action, который запускает [opencode](https://opencode.ai) для помощи с задачами разработки непосредственно в вашем репозитории.

## Использование

Этот action запускается при комментариях к issues или PR review, содержащих `/oc` или `/opencode`.

### Пример workflow

```yaml
name: opencode

on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]

jobs:
  opencode:
    if: |
      contains(github.event.comment.body, ' /oc') ||
      startsWith(github.event.comment.body, '/oc') ||
      contains(github.event.comment.body, ' /opencode') ||
      startsWith(github.event.comment.body, '/opencode')
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: write
      pull-requests: write
      issues: write
    steps:
      - name: Checkout repository
        uses: actions/checkout@v6
        with:
          persist-credentials: false

      - name: Run opencode
        uses: anomalyco/opencode/github@latest
        env:
          OPENCODE_API_KEY: ${{ secrets.OPENCODE_API_KEY }}
        with:
          model: opencode/minimax-m2.5-free
```

## Настройка

### Требуемые секреты

- `OPENCODE_API_KEY`: Ваш API-ключ opencode

### Параметры

- `model`: Модель для использования (по умолчанию: `opencode/minimax-m2.5-free`)

## Запуск opencode

Добавьте комментарий к issue или PR с `/oc` и вашим запросом:

```
/oc Explain this code
```

или

```
/opencode Write tests for this function
```
