---
description: "実装の詳細とガイドライン。"
---
$ARGUMENTS

上記の機能を以下のワークフローに従って実装します：

### 1. ブランチ管理

**ユーザーから特に指示がない限り**、常に最新の main branch から新しい branch を作成します：

```bash
# Fetch latest changes
git fetch origin

# Create and checkout new branch from origin/main
git checkout -b <機能ブランチ名> origin/main
```

### 2. コミット粒度

実装全体を通じて、適切な粒度で commit を作成します。

**重要: git add でファイルを staging する際の注意点:**
- 実装の一環として編集または作成したファイルのみを追加する
- 無関係なファイル（ビルド成果物、一時ファイル、設定ファイルなど）の追加を避ける
- `git add .` や `git add -A` の代わりに具体的なファイルパスを使用して、誤って不要なファイルを含めないようにする

```bash
# Add only the files you edited
git add path/to/edited/file1.ts path/to/edited/file2.ts
```

### 3. 修正リクエストの対処

ユーザーが実装済みの機能に対して修正を要求した場合、fixup commit を使用します：

```bash
# Identify the commit to fix
git log --oneline

# Create a fixup commit
git commit --fixup <コミットハッシュ>
```

**fixup commit を使用すべき場合:**
- ユーザーが以前に commit したコードへの変更を要求した場合
- 以前の commit の問題やバグを修正する場合
- フィードバックに基づいて調整を行う場合
- 実装の詳細を改良する場合

## ワークフロー概要

各機能実装リクエストに対して：

1. 現在の git status を確認する
2. 最新の main から新しい branch を作成する（特に指示がない限り）
3. 機能を段階的に実装する
4. 論理的なチェックポイントで、明確なメッセージと共に commit する
5. 修正リクエストに対しては、`git commit --fixup <commit>` を使用する
6. すべての commit が機能的で、適切に説明されていることを確認する
