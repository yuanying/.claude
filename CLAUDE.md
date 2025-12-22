# CLAUDE.md

## Conversation Guidelines

- 常に日本語で会話する

## Development Philosophy

### Document Strategy

- **Important** ドキュメントを作成する際は、特別に必要でない限りソースコードをドキュメントに含めないこと

### Test-Driven Development (TDD)

- 原則としてテスト駆動開発（TDD）で進める
- 期待される入出力に基づき、まずテストを作成する
- 実装コードは書かず、テストのみを用意する
- テストを実行し、失敗を確認する
- その後、テストをパスさせる実装を進める
- 実装中はテストを変更せず、コードを修正し続ける
- すべてのテストが通過するまで繰り返す

### Branch Strategy

- **Important** 何らかの作業を行う際は特に理由がない限り、最新の `main` ブランチから新しいブランチを切って作業を行うこと。
- すでに存在するブランチで作業を行っていた場合は、そのまま続行すること。

```bash
# 現在のブランチを確認
git branch
# main ブランチにいた場合は以下を実行
git fetch origin
git checkout -b feature/your-feature-name origin/main
```

### Git Strategy

- ユーザーからの指示で機能を追加・修正する場合は、意味のある単位でコミットを行うこと。
- 一通りの機能が完成した段階で、ユーザーの指摘に基づき修正を行う場合は、それが大きな機能変更でない場合 `--fixup` オプションを用いてコミットすること。

```bash
git commit --fixup=<commit-hash>
```

### Pull Request (PR) Strategy

- 作業が完了したら、`main` ブランチに対してプルリクエストを作成すること。
- PR 前にfixup コミットが残っている場合は、`git rebase -i --autosquash` を用いて整理すること。
