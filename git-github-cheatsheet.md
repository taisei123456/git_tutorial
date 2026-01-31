# 初心者向け Git & GitHub コマンド集

Gitの基本操作とリモート（GitHub）の知識をまとめたチートシートです。

---

## 目次
1. [Gitの基本概念](#1-gitの基本概念)
2. [ローカルでの基本コマンド](#2-ローカルでの基本コマンド)
3. [ブランチの操作](#3-ブランチの操作)
4. [リモート（GitHub）の基礎](#4-リモートgithubの基礎)
5. [よく使うGitHubの用語](#5-よく使うgithubの用語)
6. [実践フロー例](#6-実践フロー例)

---

## 1. Gitの基本概念

### 3つのエリア
| エリア | 説明 |
|--------|------|
| **作業ディレクトリ** | 普段ファイルを編集している場所 |
| **ステージング（インデックス）** | コミットする変更を一時的に置く場所 |
| **リポジトリ（.git）** | コミット履歴が保存される場所 |

### 基本的な流れ
```
編集 → add（ステージに追加）→ commit（履歴に記録）
```

---

## 2. ローカルでの基本コマンド

### 初期設定（初回のみ）
```bash
# ユーザー名とメールを設定（GitHubに表示される）
git config --global user.name "あなたの名前"
git config --global user.email "your-email@example.com"

# 設定確認
git config --list
```

### リポジトリの作成・状態確認
```bash
# 現在のフォルダをGitリポジトリにする
git init

# 現在の状態を確認（変更ファイル一覧）
git status

# 変更内容の差分を表示
git diff
git diff --staged   # ステージ済みの差分
```

### 変更を記録する（add → commit）
```bash
# 特定のファイルをステージに追加
git add ファイル名

# すべての変更をステージに追加
git add .

# ステージした内容をコミット（履歴に記録）
git commit -m "コミットメッセージ"

# add と commit をまとめて（変更済みファイルのみ）
git commit -am "コミットメッセージ"
```

### 履歴の確認
```bash
# コミット履歴を表示
git log

# 1行で簡潔に表示
git log --oneline

# グラフ付きで表示
git log --oneline --graph
```

### 変更の取り消し
```bash
# ステージングを取り消す（ファイルの変更は残る）
git restore --staged ファイル名
# または git reset HEAD ファイル名

# ファイルの変更を破棄（元に戻す）
git restore ファイル名
# または git checkout -- ファイル名
```

---

## 3. ブランチの操作

ブランチ = 履歴の「枝」。別の作業を並行して進められます。

```bash
# ブランチ一覧を表示（* が現在のブランチ）
git branch

# 新しいブランチを作成
git branch ブランチ名

# ブランチを切り替え
git switch ブランチ名
# または git checkout ブランチ名

# ブランチを作成してすぐ切り替え
git switch -c ブランチ名
# または git checkout -b ブランチ名

# 別ブランチの変更を現在のブランチに取り込む（マージ）
git merge ブランチ名

# マージ後、不要なブランチを削除
git branch -d ブランチ名
```

---

## 4. リモート（GitHub）の基礎

### リモートとは
- **リモート** = インターネット上にあるGitリポジトリ（例：GitHub）
- ローカルの履歴を「バックアップ」したり、チームで共有するために使う

### リモートの基本コマンド
```bash
# リモート一覧を表示（名前とURL）
git remote -v

# リモートを追加（通常、名前は origin）
git remote add origin https://github.com/ユーザー名/リポジトリ名.git

# リモートのURLを変更
git remote set-url origin 新しいURL

# リモートを削除
git remote remove origin
```

### プッシュ（push）＝ ローカル → リモートへ送る
```bash
# 初回：ブランチをリモートに送り、追跡関係を設定
git push -u origin main
# または git push -u origin master

# 2回目以降（追跡設定済みなら）
git push
```

### プル（pull）＝ リモート → ローカルへ取得してマージ
```bash
# リモートの最新を取得してマージ
git pull origin main
# 追跡設定済みなら
git pull
```

### フェッチ（fetch）＝ リモートの情報だけ取得（マージしない）
```bash
# リモートの最新を取得するが、作業ツリーは変えない
git fetch origin

# 取得後、リモートのブランチをマージする場合
git merge origin/main
```

### クローン（clone）＝ リモートを丸ごと取得
```bash
# GitHubのリポジトリをローカルにコピー
git clone https://github.com/ユーザー名/リポジトリ名.git

# フォルダ名を指定してクローン
git clone https://github.com/ユーザー名/リポジトリ名.git フォルダ名
```

---

## 5. よく使うGitHubの用語

| 用語 | 説明 |
|------|------|
| **リポジトリ（Repo）** | プロジェクトのGitの保存場所。GitHub上では「レポジトリ」として1プロジェクト1つ作る |
| **origin** | リモートのデフォルト名。`origin` = 自分が push/pull するメインのリモート |
| **main / master** | デフォルトのメインブランチ名。GitHubでは「main」が標準 |
| **Fork** | 他人のリポジトリを自分のアカウントにコピーすること。改造や貢献の入口 |
| **Pull Request（PR）** | 「このブランチをマージしてください」と依頼すること。コードレビューや議論に使う |
| **Issue** | バグ報告・機能要望・質問などを書く「チケット」 |
| **README.md** | リポジトリの説明文。GitHubではトップに表示される |

### GitHubでよくやる操作の流れ
1. GitHubで **New repository** を作成
2. ローカルで `git remote add origin (URL)` → `git push -u origin main`
3. 修正後は `git add` → `git commit` → `git push`
4. 他人のリポジトリに貢献する場合は **Fork** → クローン → ブランチで修正 → **Pull Request**

---

## 6. 実践フロー例

### パターンA：新規プロジェクトをGitHubに上げる
```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/あなたのユーザー名/リポジトリ名.git
git branch -M main
git push -u origin main
```

### パターンB：既存のGitHubリポジトリを手元に持ってくる
```bash
git clone https://github.com/ユーザー名/リポジトリ名.git
cd リポジトリ名
# 編集後
git add .
git commit -m "変更内容"
git push
```

### パターンC：毎日の作業（修正 → プッシュ）
```bash
git status
git add .
git commit -m "〇〇を修正"
git pull   # 先に他人の変更があれば取り込む
git push
```

### パターンD：新機能をブランチで開発してPR
```bash
git switch -c feature/新機能名
# 編集・コミット
git add .
git commit -m "新機能を追加"
git push -u origin feature/新機能名
# → GitHubで Pull Request を作成
```

---

## 困ったときの参考

| やりたいこと | コマンド・手順 |
|--------------|----------------|
| 直前のコミットメッセージを直したい | `git commit --amend -m "新しいメッセージ"` |
| 直前のコミットに変更を足したい | `git add .` → `git commit --amend --no-edit` |
| リモートのURLを確認したい | `git remote -v` |
| どのブランチにいるか確認 | `git branch` または `git status` |
| コンフリクトが出た | ファイルを編集して競合を解消 → `git add` → `git commit` |

---

**Git** = バージョン管理のツール（ローカル）  
**GitHub** = Gitのリポジトリをホストするサービス（リモート）

まずは `git status`・`git add`・`git commit`・`git push` に慣れるところから始めましょう。
