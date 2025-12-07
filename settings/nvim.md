# Nvim設定まとめ

## 設定ファイルの場所
`~/.config/nvim/init.vim`

## 現在の設定

### 基本設定
- **行番号**: 有効 (`set number`)
- **インデント**: ソフトタブを4スペースに設定 (`set softtabstop=4`)

## プラグインマネージャー
- **vim-plug**: プラグイン管理に使用
- プラグインインストール先: `~/.config/nvim/plugged`

## インストール済みプラグイン

### 1. vim-airline & vim-airline-themes
- **目的**: テーマ付きの拡張ステータスライン
- **リポジトリ**: `vim-airline/vim-airline`, `vim-airline/vim-airline-themes`
- **機能**: 様々なカラーテーマでカスタマイズ可能なステータスバーを提供

### 2. vim-commentary
- **目的**: コードのコメントアウト/アンコメントを簡単に
- **リポジトリ**: `tpope/vim-commentary`
- **使用方法**: `gc`で選択行やモーションのコメントを切り替え

### 3. coc.nvim
- **目的**: Language Server Protocolサポートと自動補完
- **リポジトリ**: `neoclide/coc.nvim` (releaseブランチ)
- **機能**: 自動補完、診断、言語サポートなどIDE的機能を提供

### 4. NERDTree
- **目的**: ファイルエクスプローラー
- **リポジトリ**: `preservim/nerdtree`
- **機能**: ツリー形式でファイルシステムを閲覧・操作できるサイドバー

### 5. lazygit.nvim
- **目的**: Neovim内でlazygitを使用
- **リポジトリ**: `kdheepak/lazygit.nvim`
- **機能**: Neovim内でGit操作を行うターミナルUIを提供

### 6. markdown-preview.nvim
- **目的**: Markdownのリアルタイムプレビュー
- **リポジトリ**: `iamcco/markdown-preview.nvim`
- **機能**: ブラウザでMarkdownファイルのプレビューを表示
- **インストール**: プラグインインストール時に`cd app && npx --yes yarn install`を実行

## 正常動作に必要なツール

### 必須依存関係
1. **vim-plug**: プラグインマネージャーのインストールが必要
   ```bash
   curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
   ```

2. **Node.js**: coc.nvimとmarkdown-preview.nvimに必要
   - パッケージマネージャーまたはnodejs.orgからインストール
   - 最小バージョン: Node.js 12以上

3. **yarn**: markdown-preview.nvimのビルドに必要
   ```bash
   npm install -g yarn
   ```

4. **lazygit**: lazygit.nvimプラグインを使用するために必要
   ```bash
   # Homebrew経由でインストール
   brew install lazygit
   ```

### プラグインインストール
依存関係確認後:
1. nvimを開く
2. `:PlugInstall`を実行してすべてのプラグインをインストール
3. coc.nvim用に必要な言語サーバーを`:CocInstall`で追加

### オプション拡張
- **言語サーバー**: 使用するプログラミング言語の言語サーバーを`:CocInstall`でインストール
- **フォント**: airlineの表示向上のためPowerline対応フォント
- **カラースキーム**: vim-airline-themesに追加テーマを導入可能

## セットアップコマンド
```bash
# vim-plugのインストール
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

# 必要なツールのインストール（macOS/Homebrew）
brew install node yarn lazygit

# nvimを開いてプラグインをインストール
nvim +PlugInstall +qall
```

## 設定ファイル全体（移行用）
以下は現在の`~/.config/nvim/init.vim`の完全なコードです：

```vim
"行番号
set number
"インデントの設定
set softtabstop=4

call plug#begin('~/.config/nvim/plugged')
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'
Plug 'tpope/vim-commentary'
Plug 'neoclide/coc.nvim', {'branch': 'release'}
Plug 'preservim/nerdtree'
Plug 'kdheepak/lazygit.nvim'
Plug 'iamcco/markdown-preview.nvim', { 'do': 'cd app && npx --yes yarn install' }

call plug#end()
```

## 新しいPCへの移行手順

### 1. 前提条件の確認
```bash
# Homebrewがインストールされているか確認
which brew

# Neovimがインストールされているか確認
which nvim
```

### 2. Neovimのインストール（必要な場合）
```bash
brew install neovim
```

### 3. 設定ファイルのディレクトリ作成
```bash
mkdir -p ~/.config/nvim
```

### 4. 設定ファイルのコピー
上記「設定ファイル全体（移行用）」のコードを`~/.config/nvim/init.vim`に保存します。

```bash
# エディタで開いて貼り付け
nvim ~/.config/nvim/init.vim
```

### 5. vim-plugのインストール
```bash
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

### 6. 必要な依存ツールのインストール
```bash
# Node.js、yarn、lazygitをインストール
brew install node yarn lazygit

# インストール確認
node --version
yarn --version
lazygit --version
```

### 7. プラグインのインストール
```bash
# nvimを開いてプラグインをインストール
nvim +PlugInstall +qall
```

### 8. 動作確認
```bash
# nvimを起動
nvim

# 以下のコマンドを試してプラグインが正常動作するか確認
# :NERDTree - ファイルエクスプローラーを開く
# :CocInfo - coc.nvimの情報を表示
# :LazyGit - lazygitを開く
# :MarkdownPreview - Markdownプレビューを開く（.mdファイル内で）
```

### 9. 追加設定（オプション）
必要に応じて言語サーバーをインストール：
```vim
" nvim内で実行
:CocInstall coc-tsserver coc-python coc-json
```

---
**最終更新日時**: 2025-12-07
