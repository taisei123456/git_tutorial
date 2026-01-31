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
7. [Pull Request と Issue の使い方](#7-pull-request-と-issue-の使い方)

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

## 7. Pull Request と Issue の使い方

### Issue（イシュー）とは

**Issue** は、リポジトリ内の「タスク」や「話題」を1件ずつ管理するための投稿です。コードの変更とは別に、「何をすべきか」「何が起きたか」を記録・議論するために使います。

| 主な使い道 | 例 |
|------------|-----|
| バグ報告 | 「〇〇の画面でエラーになる」 |
| 機能要望 | 「△△機能を追加してほしい」 |
| 質問・相談 | 「この仕様で合っていますか？」 |
| タスク管理 | 「リリース前にやること」をリスト化 |

---

### Issue の基本的な使い方

#### 1. Issue を開く（作成する）

1. リポジトリのトップで **Issues** タブをクリック
2. **New issue** をクリック
3. **タイトル**と**本文**を書く
   - タイトル：何についてかが一目でわかる短い文（例：「ログイン時にエラーが発生する」）
   - 本文：状況・再現手順・希望する結果などを書く
4. 必要なら **Labels**（ラベル）や **Assignees**（担当者）を設定
5. **Submit new issue** で作成

#### 2. 本文に書くとよいこと（バグ報告の例）

- **再現手順**：どの操作をすると起きるか
- **期待する結果**：本来どう動くべきか
- **実際の結果**：今どうなっているか
- **環境**：OS・ブラウザ・バージョンなど（必要な場合）

#### 3. Issue を閉じる

- 対応が終わったら、その Issue のページで **Close issue** をクリック
- コメントで「Fix #3」のように `Fix #Issue番号` と書いてコミットすると、その PR/コミットがマージされたときに Issue が自動で閉じる

#### 4. Issue 同士・PR との紐づけ

- 本文やコメントで `#1` のように書くと、Issue/PR 番号へのリンクになる
- 「この PR は Issue #5 を対応します」と書いておくと、後から追いやすい

---

### Pull Request（PR）とは

**Pull Request** は、「このブランチの変更を、指定したブランチ（多くは `main`）に取り込んでほしい」と依頼する機能です。変更内容の確認（コードレビュー）や、マージ前の議論に使います。

| 主な使い道 | 例 |
|------------|-----|
| 機能追加・修正の依頼 | 「feature/login を main にマージしてください」 |
| コードレビュー | 他人に変更を見てもらい、コメントをもらう |
| 議論 | 「この実装方針でよいか」を Issue のように議論する |

---

### Pull Request の基本的な使い方

#### 1. PR を出すまでの流れ（自分のリポジトリの場合）

1. **ブランチを作って作業**
   ```bash
   git switch -c feature/新機能名
   # 編集
   git add .
   git commit -m "新機能を追加"
   ```

2. **ブランチをリモートに送る**
   ```bash
   git push -u origin feature/新機能名
   ```

3. **GitHub で PR を作成**
   - リポジトリのトップに「Compare & pull request」が出ていればそれをクリック
   - 出ていなければ **Pull requests** タブ → **New pull request**
   - **base**：取り込み先のブランチ（例：`main`）
   - **compare**：自分のブランチ（例：`feature/新機能名`）
   - タイトルと本文を書いて **Create pull request**

#### 2. PR の本文に書くとよいこと

- **変更の目的**：何のためか（例：「〇〇機能を追加」）
- **やったこと**：変更内容の要約
- **関連 Issue**：`Closes #3` のように書くと、マージ時にその Issue が自動で閉じる
- **レビューポイント**：特に見てほしいファイルや考え方

#### 3. レビューと修正

- レビュアーがコメントを付けることがある
- 修正する場合は、**同じブランチでコミットして push** すれば、PR に自動で反映される（新しい PR は作らない）
  ```bash
  git add .
  git commit -m "指摘を反映"
  git push
  ```

#### 4. マージする

- 問題なければ、権限がある人が **Merge pull request** をクリック
- **Confirm merge** で確定
- マージ後、不要なら「Delete branch」でブランチを削除してよい

#### 5. 他人のリポジトリに PR を出す場合（Fork する場合）

1. 相手のリポジトリで **Fork** をクリック（自分のアカウントにコピーができる）
2. 自分の Fork を **clone** して、ブランチで修正
3. 自分の Fork に **push**
4. 自分の Fork のページから **Compare & pull request** を押すと、**元のリポジトリ**に向けて PR が作成される
5. 元のリポジトリのオーナーがレビュー・マージする

---

### Issue と PR を一緒に使う流れ（おすすめ）

1. **Issue でタスクを立てる**  
   「〇〇機能を追加する」という Issue を1つ作る。
2. **ブランチ名や PR で Issue を参照する**  
   ブランチ名を `feature/3-add-login` のようにするか、PR の本文に「Closes #3」と書く。
3. **PR をマージする**  
   「Closes #3」が書いてある PR をマージすると、Issue #3 が自動で閉じる。
4. **必要なら Issue で振り返り**  
   同じ Issue のコメントで「対応内容はこのPRで」とリンクを貼っておくと、後から追いやすい。

---

### 用語の整理

| 用語 | 意味 |
|------|------|
| **Open / Close** | Issue や PR を「進行中」にするか「終了」にするか |
| **Assign** | その Issue / PR の担当者を決める |
| **Label** | 種類（bug / feature / question など）を付けて整理する |
| **Review** | PR のコードを見て、承認やコメントをすること |
| **Merge** | PR の変更を base ブランチに取り込むこと |
| **Closes #番号** | PR の本文に書くと、マージ時に指定した Issue が自動で閉じる |

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
