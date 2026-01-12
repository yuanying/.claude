---
name: issue-fix
description: GitHub Issue を修正し、関連する PR を作成する手順書。
---

# GitHub Issue 修正手順

このコマンドは、指定された Issue を修正し、PR を作成して Issue と関連付けます。

## 引数の確認

**$ARGUMENTS**: Issue 番号または Issue URL（必須）

例:
- `/issue-fix 123`
- `/issue-fix #123`
- `/issue-fix https://github.com/owner/repo/issues/123`

引数がない場合は、ユーザーに Issue 番号を確認する。

## 1. Issue の確認

```bash
# Issue の詳細を確認
gh issue view <issue番号>
```

Issue の内容を理解し、修正方針を把握する。

## 2. 実装プランの作成と共有

Issue の内容を分析し、実装プランを作成する。

**プランに含める内容**:
- 修正方針の概要
- 変更対象のファイル
- 実装ステップ

**プランを Issue にコメントとして投稿**:

```bash
gh issue comment <issue番号> --body "$(cat <<'EOF'
## 🔧 実装プラン

### 概要
[修正方針の概要]

### 変更対象ファイル
- `path/to/file1.ts`
- `path/to/file2.ts`

### 実装ステップ
1. [ステップ1]
2. [ステップ2]
3. [ステップ3]

---
🤖 Generated with [Claude Code](https://claude.com/claude-code)
EOF
)"
```

ユーザーにプランを確認してもらい、承認を得てから実装に進む。

## 3. ブランチ管理

- **ユーザーから特に指示がない限り**、常に最新の main branch から新しい branch を作成する
- ブランチ名は Issue 番号を含める（例: `fix/issue-123-description`）

```bash
# Fetch latest changes
git fetch origin

# Create and checkout new branch from origin/main
git checkout -b fix/issue-<issue番号>-<簡潔な説明> origin/main
```

## 4. 実装

Issue の内容に基づいて修正を実装する。

**コミット粒度**:
- 実装全体を通じて、適切な粒度で commit を作成する
- 実装の一環として編集または作成したファイルのみを追加する
- `git add .` や `git add -A` の代わりに具体的なファイルパスを使用する
- コミットメッセージは英語で書き、Subject は 50 文字以内、Body は 72 文字以内に収める
- コミットメッセージの Subject は命令形で書く

```bash
# Add only the files you edited
git add path/to/edited/file1.ts path/to/edited/file2.ts

# Commit with clear message
git commit -m "Fix: description of the change"
```

## 5. 修正リクエストの対処

ユーザーが実装済みの機能に対して修正を要求した場合、fixup commit を使用する:

```bash
# Identify the commit to fix
git log --oneline

# Create a fixup commit
git commit --fixup <コミットハッシュ>
```

## 6. PR 作成前の準備

### fixup コミットの整理

PR 作成前に fixup コミットを整理する:

```bash
# fixup コミットがあるか確認
git log --oneline origin/main..HEAD

# fixup コミットがある場合は整理
git rebase -i --autosquash origin/main
```

### リモートにプッシュ

```bash
git push -u origin <branch-name>
```

## 7. PR の作成

Issue と関連付けた PR を作成する。

**重要**: PR 本文に `Fixes #<issue番号>` を含めることで、PR がマージされた際に Issue が自動的にクローズされる。

```bash
gh pr create --title "<PRタイトル>" --body "$(cat <<'EOF'
## Summary

[変更内容の概要]

## Related Issue

Fixes #<issue番号>

## Changes

- [変更点1]
- [変更点2]

## Test Plan

- [テスト項目1]
- [テスト項目2]

🤖 Generated with [Claude Code](https://claude.com/claude-code)
EOF
)"
```

## 8. 完了確認

```bash
# PR の状態を確認
gh pr view

# CI/チェックの状態を確認
gh pr checks
```

PR の URL をユーザーに表示する。

## ワークフロー概要

1. Issue 番号を確認し、Issue の内容を把握
2. 実装プランを作成し、Issue にコメントとして投稿
3. 最新の main から新しい branch を作成
4. 機能を段階的に実装
5. 論理的なチェックポイントで commit
6. fixup コミットを整理
7. リモートにプッシュ
8. `Fixes #<issue番号>` を含む PR を作成
9. PR の URL を表示
