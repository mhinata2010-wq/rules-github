# Pull Request Rules

## Description必須

Pull Requestは必ずDescriptionを記載します。

`No description provided.` の状態は禁止です。

## 標準テンプレート

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

## Repository横断時

```md
## Cross Repository
- Source Repository: owner/repo
- Source Branch: feature/...
- Source PR: #123
- Destination Repository: owner/repo
- Destination Path: path/to/file
- Reason: 移管理由
```

## Merge前チェック

- [ ] PR Descriptionが空欄ではない
- [ ] 変更目的が1文で説明できる
- [ ] 変更ファイルが意図した範囲だけである
- [ ] Secret / Token / 個人情報が含まれていない
- [ ] Testing結果を記載した
- [ ] Known Issueがあれば記載した
- [ ] 関連Issue / PR / Repositoryを記載した
- [ ] Productionのmainへ入れてよい変更か確認した

## 大きすぎるPR

以下の場合はPR分割を検討します。

- 複数の無関係な機能が含まれる
- Review時に変更目的を1つに説明できない
- PrototypeとProduction変更が混ざっている
- 数十Commitが長期間積み重なり、差分の意図が追いづらい

## Superseded PR

新しいPRで置き換えた場合、旧PRを放置せずDescriptionに以下を残してCloseします。

```md
## Notes
- Superseded by #123
- 実装は新PRへ移行
- このPRはmergeしない
```

## Merge Method

原則として、小〜中規模のFeature PRはSquash Mergeを推奨します。

理由:

- `main` の履歴を機能単位で読みやすくする
- 試行錯誤CommitをProduction履歴へ大量に残さない
- Revert単位を明確にする

複数Commitの履歴自体に意味がある場合はMerge / Rebaseも選択可能です。
