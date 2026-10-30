# Cross Repository Rules

## 目的

複数Repositoryにまたがるデータ・機能・デザイン資産を、安全に移管・参照できるようにするための規定です。

## Repository横断は許可する

以下のケースではRepository横断を認めます。

- ProductionからDesign Referenceを分離する
- PrototypeからProductionへ採用機能を移す
- 共通ルールやテンプレートを専用Repositoryへ移す
- 大きく役割が異なる機能を別Repositoryへ切り出す
- 旧Repositoryから新Repositoryへ移行する

## 必須記録

横断時は最低限以下をPR Descriptionに記録します。

```md
## Cross Repository
- Source Repository: owner/repo
- Source Branch: feature/...
- Source PR: #123
- Destination Repository: owner/repo
- Destination Path: path/to/file
- Reason: なぜ移すのか
- Classification: Production / Prototype / Reference / Archive
- Source Handling: keep / close / delete / archive
```

## 原則

1. コピー元とコピー先を明示する
2. ProductionのSource of Truthを必ず1つ決める
3. 同じ機能を複数Repositoryで別々に進化させない
4. 移管後に旧PRが不要なら、移管先を記載してCloseする
5. Design ReferenceをProductionへ戻す場合は、そのままコピーせずProduction要件に合わせて再評価する
6. Repository横断後もGit履歴から経緯を追える状態にする

## Source of Truth

同じデータや機能が複数Repositoryに存在する場合、必ずどれが正本かを決めます。

```text
Production Source of Truth
Reference Copy
Archive Copy
```

曖昧な状態は禁止します。

## 例

```text
Source:
building-schedule/feature/codebase-client-ui

Destination:
design-deta/codebase-client-ui/

Reason:
Productionからデザイン参考実装を分離するため

Classification:
Reference

Source Handling:
旧PRをCloseし、Production mainには含めない
```

## 禁止

- 出所不明のコードを別Repositoryへコピーする
- PRやCommitの記録を残さずに機能を移す
- ProductionとPrototypeの両方を正本として扱う
- Secretや顧客機密を横断移管する
- 第三者のライセンス条件を無視して資産を移す
