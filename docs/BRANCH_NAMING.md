# Branch Naming Rules

## 目的

Branch名を見ただけで、そのBranchで何を追加・変更しているか分かる状態を作ります。

## 基本形式

```text
<prefix>/<specific-change-name>
```

## Prefix

| Prefix | 意味 | 例 |
|---|---|---|
| `feature/` | 新機能 | `feature/network-schedule-auto-sync` |
| `fix/` | Bug修正 | `fix/task-dialog-cancel` |
| `refactor/` | 構造改善 | `refactor/task-single-source-of-truth` |
| `design/` | UI/UX変更 | `design/mobile-today-view` |
| `docs/` | 文書・規定 | `docs/github-governance-rules` |
| `experiment/` | PoC・実験 | `experiment/codebase-dashboard-reference` |
| `migration/` | Repository/データ移行 | `migration/codebase-ui-to-design-repo` |
| `release/` | Release準備 | `release/public-final-v1` |
| `security/` | Security対応 | `security/remove-exposed-token` |
| `chore/` | 設定・保守 | `chore/update-pages-workflow` |

## 命名原則

Branch名には可能な限り次の情報を含めます。

1. **対象**: `gantt`, `network`, `receipt`, `mobile-nav`, `task-dialog`
2. **変更内容**: `add`, `sync`, `scroll`, `cancel`, `export` などが推測できる単語
3. **目的**: 必要なら `public`, `reference`, `realtime`, `mobile` など

ただし冗長すぎる名前は避け、3〜6語程度を目安にします。

## 良い例

```text
feature/gantt-horizontal-scroll
feature/receipt-task-link
feature/network-schedule-auto-sync
feature/mobile-progress-report
fix/task-dialog-cancel
fix/gantt-bar-overlap
fix/github-pages-cache
design/operational-status-dashboard
design/mobile-bottom-navigation
refactor/task-single-source-of-truth
migration/reference-ui-to-design-repo
release/public-final-v1
```

## 悪い例

```text
feature/update
feature/new
feature/test
fix/ui
fix/bug
work
branch2
latest
final
final2
hinata-test
```

## 1 Branch = 1 Main Purpose

1つのBranchに無関係な機能を詰め込みません。

悪い例:

```text
feature/gantt-receipt-profile-ai-dashboard
```

良い分割:

```text
feature/gantt-horizontal-scroll
feature/receipt-task-link
feature/profile-edit
feature/ai-daily-report
```

## Repository横断時

Repository間で移管するときは `migration/` を推奨します。

```text
migration/codebase-ui-to-design-repo
migration/legacy-schedule-to-building-schedule
migration/shared-rules-to-rules-github
```

## Branchを作る前の確認

- 最新 `main` から切るべきか
- 既存Branchと目的が重複していないか
- 同じ機能のOpen PRがないか
- Repositoryの役割に合っているか
- Branch名だけで変更内容を説明できるか

最後の質問に「できない」と答える場合、そのBranch名は作り直します。
