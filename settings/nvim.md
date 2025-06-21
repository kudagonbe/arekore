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

## 正常動作に必要なツール

### 必須依存関係
1. **vim-plug**: プラグインマネージャーのインストールが必要
   ```bash
   curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
   ```

2. **Node.js**: coc.nvimに必要
   - パッケージマネージャーまたはnodejs.orgからインストール
   - 最小バージョン: Node.js 12以上

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

call plug#end()
```

### 移行手順
1. 新しいPCで上記の内容を`~/.config/nvim/init.vim`に保存
2. vim-plugをインストール（上記セットアップコマンド参照）
3. nvimを開いて`:PlugInstall`でプラグインをインストール
4. Node.jsがインストールされていることを確認（coc.nvim用）