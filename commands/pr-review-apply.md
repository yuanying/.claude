---
name: pr-review-apply
description: GitHub Pull Requestのレビュー内容を適用する手順書。PRレビューのフィードバックをコードに反映する際に使用。
---

# PRレビュー内容の適用手順

## PR URLのパース

PR URL `https://github.com/OWNER/REPO/pull/NUMBER` から以下を抽出して使用：
- `OWNER`: リポジトリオーナー
- `REPO`: リポジトリ名
- `NUMBER`: PR番号

PR のURLは現在のブランチから取得してください。

## 操作一覧

### 1. PR情報取得

```bash
gh pr view NUMBER --repo OWNER/REPO --json title,body,author,state,baseRefName,headRefName,url
```

### 2. コメント取得

Issue Comments（PR全体へのコメント）:
```bash
gh api repos/OWNER/REPO/issues/NUMBER/comments --jq '.[] | {id, user: .user.login, created_at, body}'
```

Review Comments（コード行へのコメント）:
```bash
gh api repos/OWNER/REPO/pulls/NUMBER/comments --jq '.[] | {id, user: .user.login, path, line, created_at, body, in_reply_to_id}'
```

### 3. コメント内容の確認と適用

- 取得したコメントを確認し、コードに反映すべきフィードバックを特定する
- 各コメントの内容に基づき、ローカルリポジトリでコードを修正する
- 修正後、必要に応じてテストを実行し、動作確認を行う
- 修正が完了したら、変更をコミットし、プッシュする
  - コミットはレビューに関連する元のコミットに対して `--fixup` オプションを使用して行うことを推奨
  - 別のコミットとしてまとめたい場合は通常のコミットを行う

```bash
git add <modified-files>
git commit --fixup=<commit-hash-of-original-change>
```

### 4. PRの更新

- **Important** 更新をプッシュした後、**必ず** PRの説明を更新し、変更に関連する返信を追加する

```bash
gh api repos/OWNER/REPO/pulls/NUMBER/comments/COMMENT_ID/replies \
  --method POST \
  -f body="返信内容"
```

`COMMENT_ID`はコメント取得で得た`id`を使用。
