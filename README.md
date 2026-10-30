# GitHub Operation Rules

このRepositoryは、`mhinata2010-wq` 配下で進める開発・デザイン・検証プロジェクトのGitHub運用ルールを管理する **Single Source of Truth** です。

各Repositoryで判断が分かれた場合は、原則としてこのRepositoryの規定を優先します。

## 基本方針

1. `main` は原則として、そのRepositoryで公開・共有できる安定版とする
2. 大きな変更は `main` に直接入れず、Feature Branch + Pull Requestで行う
3. Branch名だけで「何を変更するBranchか」が分かる名前にする
4. Pull RequestのDescriptionを空欄にしない
5. Repositoryを横断した実装・データ移管を認める。ただしSource / Destination / Reasonを記録する
6. Prototype・Design Reference・ExperimentとProductionを混在させない
7. API Key、Password、Secret、Token、個人情報などをGitHubへCommitしない
8. 既存Repositoryを変更するときは、Default Branch / Current Branch / Open PR / Open Issue / Directory Structureを確認する
9. 公開用Repositoryでは、第三者の商用ソースコード・専用アセットをそのまま複製しない
10. `main` に入る変更は「現在のProductionの正本を前進させる変更」であること

## Repositoryの役割

Repositoryごとに役割を明確にします。

- **Production Repository**: 公開版・実運用版を管理
- **Design / Reference Repository**: UI模倣、Visual Experiment、Reference実装を管理
- **Rules Repository**: GitHub運用・開発ルールを管理
- **Prototype / PoC Repository**: 短期検証を管理
- **Archive Repository**: 今後本番で使わないが履歴として保持する資産を管理

ProductionのSource of TruthとReference Dataを同じRepositoryで長期間混在させないことを原則とします。

## Repository横断を認める

Repositoryを横断したデータ移行・機能移管・設計参照を正式に認めます。

例:

```text
building-schedule
  └─ Production

design-deta
  └─ UI Reference / Design Experiment

rules-github
  └─ GitHub Governance
```

ただし、横断するときは必ず以下を残します。

- Source Repository
- Source Branch / PR / Commit
- Destination Repository
- Destination Path
- 移管理由
- ProductionかReferenceか
- 元データを残すか削除するか

詳細: [`docs/CROSS_REPOSITORY_RULES.md`](docs/CROSS_REPOSITORY_RULES.md)

## Branch Naming Rule

Branch名は **Prefix + 具体的な変更内容** とします。

```text
<prefix>/<feature-or-change-name>
```

推奨Prefix:

| Prefix | 用途 |
|---|---|
| `feature/` | 新機能 |
| `fix/` | Bug修正 |
| `refactor/` | 挙動を変えない構造改善 |
| `docs/` | ドキュメント |
| `design/` | UI/UX変更 |
| `experiment/` | PoC・実験 |
| `release/` | 公開・Release準備 |
| `chore/` | 設定・保守 |
| `security/` | Security対応 |
| `migration/` | データ・構成移行 |

良い例:

```text
feature/gantt-horizontal-scroll
feature/receipt-task-link
feature/network-schedule-auto-sync
fix/task-dialog-cancel
fix/mobile-nav-overlap
refactor/task-single-source-of-truth
design/operational-status-dashboard
experiment/codebase-dashboard-reference
migration/codebase-ui-to-design-repo
release/public-final-v1
docs/github-governance-cross-repo-branch-rules
```

禁止・非推奨:

```text
feature/update
feature/new
fix/ui
fix/test
branch1
work
latest
final-final
hinata-test
```

**Branch名を見ただけで、そのBranchで追加・変更した機能が推測できない名前は禁止**します。

詳細: [`docs/BRANCH_NAMING.md`](docs/BRANCH_NAMING.md)

## main Branch Rule

`main` は以下を満たす状態を原則とします。

- 公開して問題ない
- 主要機能が壊れていない
- Reference用の不要なPrototypeが混在していない
- Secretが含まれていない
- PR Descriptionが存在する
- 変更理由が履歴から追える

Production Repositoryでは、`main` を公開専用の安定版として扱います。

Prototypeや大幅なUI実験を行う場合は、別Branchまたは別Repositoryを使用します。

## Pull Request Rule

Pull RequestはDescription必須です。

`No description provided.` の状態は禁止します。

最低限、以下を記載します。

```md
## Summary
このPRで何を変更したか

## Changes
- 変更内容1
- 変更内容2

## Testing
- 実施した動作確認
- テスト結果

## Notes
- 注意事項
- 未対応事項
- レビュアーに確認してほしい点

## Related
- 関連Issue / PR / Repository
```

Repository横断の変更ではさらに以下を記載します。

```md
## Cross Repository
- Source: owner/repository#PR
- Destination: owner/repository/path
- Reason: 移管理由
```

詳細: [`docs/PULL_REQUEST_RULES.md`](docs/PULL_REQUEST_RULES.md)

## Commit Rule

Commit messageは変更内容が追えるものにします。

推奨:

```text
feat: add gantt horizontal scrolling
fix: close task dialog on cancel
refactor: centralize task state
style: update mobile navigation spacing
docs: define cross repository rules
chore: update pages workflow
```

避ける:

```text
update
fix
change
aaa
test
final
```

## Design / Reference Data Rule

外部サイトを参考にしたUI、模倣検証、デザインパターン研究はProduction Repositoryと分離できます。

例:

```text
design-deta/
  codebase-client-ui/
  dashboard-reference/
  mobile-navigation-study/
```

ただし、第三者のライセンス違反につながるソースコード・画像・フォント・有償アセットそのものは保存しません。

Reference Repositoryには、可能な限り以下を記載します。

- Reference URL
- 参考にした要素
- 自分たちで再実装した範囲
- Productionで採用する要素
- Productionでは採用しない要素

## Security Rule

以下はCommit禁止です。

- API Key
- Password
- Secret
- Access Token
- Private Key
- Cookie
- `.env` の実値
- 個人情報
- 顧客の機密情報

必要に応じて以下を使用します。

```text
.env
.env.local
.gitignore
.env.example
```

`.env.example` には値を入れず、必要なKey名だけ記載します。

## Repository作成時の最低要件

新規RepositoryではREADMEに最低限以下を記載します。

- 解決する課題
- 想定ユーザー
- 提供価値
- 主要機能
- 技術スタック
- Architecture
- Setup
- 現在の開発状況
- Roadmap
- Production / Prototype / Reference のどれか

公開範囲の指定がない新規Repositoryは、機密性がある場合はPrivateを優先します。

## 開発フロー

```text
Issue / Requirement
        ↓
Repositoryを決定
        ↓
mainの最新状態を確認
        ↓
具体的なBranch名を作成
        ↓
実装
        ↓
Test / Review
        ↓
Description付きPR
        ↓
Merge
        ↓
main = 最新の安定版
```

Repositoryを横断する場合:

```text
Source Repository
        ↓
移管対象を確定
        ↓
Destination RepositoryでBranch作成
        ↓
データ移管
        ↓
Source / DestinationをPRに記録
        ↓
DestinationへMerge
        ↓
Source側の旧PR/Branchを整理
```

## 現在の代表的なRepository Policy

```text
building-schedule
  main = Production / Public Source of Truth

design-deta
  main = Design Reference / UI Experiment

rules-github
  main = GitHub Governance Source of Truth
```

この対応関係は今後Repositoryが増えた場合も、READMEまたは本Repositoryに追記します。

## Rule変更方法

この規定自体も `main` に直接変更せず、原則として以下で更新します。

```text
docs/<what-rule-changes>
```

例:

```text
docs/add-release-tag-policy
docs/update-cross-repository-rules
```

変更後はDescription付きPRを作成し、規定変更理由を履歴に残します。
