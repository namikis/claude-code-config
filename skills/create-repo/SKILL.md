---
name: create-repo
description: GitHubリポジトリを新規作成するスキル。ローカルリポジトリを~/Desktop/program/配下に作成し、GitHubにリモートリポジトリを作成して紐づける。「リポジトリを作って」「新しいrepoを作成」「プロジェクトを作って」「create repo」などの指示で使用。DO NOT TRIGGER when: 既存リポジトリのクローンや、リポジトリの削除・設定変更の場合。
argument-hint: "[リポジトリ名]"
allowed-tools: Bash(git *), Bash(gh repo create *), Bash(ls *), Bash(mkdir *)
---

# リポジトリ作成スキル

引数またはユーザー指示からリポジトリ名を取得し、ローカル+リモートリポジトリを作成する。

## 前提条件

- `gh` CLI がインストール済みで認証済みであること
- `git` が利用可能であること

## ワークフロー

### Step 1: リポジトリ名の確認

引数からリポジトリ名を取得する。引数がない場合はユーザーに確認する。

### Step 2: ローカルリポジトリ作成

```bash
mkdir ~/Desktop/program/<repo-name>
cd ~/Desktop/program/<repo-name>
git init
```

既にディレクトリが存在する場合はユーザーに確認してから進める。

### Step 3: 初期ファイル作成

README.mdを作成してinitial commitを行う。

```bash
echo "# <repo-name>" > README.md
git add README.md
git commit -m "Initial commit"
```

### Step 4: リモートリポジトリ作成・紐づけ

`gh` CLIでGitHubにリモートリポジトリを作成し、紐づける。デフォルトはprivate。

```bash
gh repo create <repo-name> --private --source=. --remote=origin --push
```

### Step 5: 完了確認

`git remote -v` でリモートが正しく紐づいていることを確認し、結果をユーザーに報告する。

## 完了条件

- ~/Desktop/program/<repo-name> にローカルリポジトリが存在する
- GitHubにリモートリポジトリが作成されている
- originとして紐づけられ、initial commitがpush済み
