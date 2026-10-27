# Zsh設定まとめ

## 設定ファイルの場所
`~/.zshrc`

## 概要
このドキュメントは、現在のzsh設定（`.zshrc`）の内容と、設定を正常に動作させるために必要なツールについてまとめています。

## PATH設定
- **asdf**: `~/.asdf/shims` をPATHの先頭に追加
- **WezTerm**: `/Applications/WezTerm.app/Contents/MacOS` をPATHに追加
- **~/.local/bin**: PATHの先頭に追加（Claude Code のネイティブインストール先など）
- **arekore/bin**: このリポジトリの `bin/` をPATHの先頭に追加（`cmux-setup` などの自作コマンド用。詳細は [bin/README.md](../bin/README.md)）

## 必要なツール・パッケージ

### 必須ツール
1. **asdf** - バージョン管理ツール
   - 用途: 複数言語のバージョン管理
   - インストール: `brew install asdf`（設定は `ASDF_DATA_DIR` と shims を使う v0.16 以降の方式）

2. **Homebrew** - パッケージマネージャー
   - 用途: macOS用パッケージ管理
   - インストール: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

### Homebrewでインストールが必要なパッケージ
```bash
brew install zsh-completions
brew install zsh-autosuggestions
brew install zsh-git-prompt
```

### その他の必須ツール
3. **lazygit** - Git GUIツール
   - エイリアス: `lg`
   - インストール: `brew install lazygit`

4. **pet** - コマンドスニペット管理ツール
   - 用途: コマンド履歴の検索・保存
   - インストール: `brew install knqyf263/pet/pet`

5. **neovim** - テキストエディタ
   - エイリアス: `vim`
   - インストール: `brew install neovim`

6. **container** - Apple 製のコンテナ実行ツール
   - エイリアス: `c`
   - インストール: [apple/container の Releases](https://github.com/apple/container/releases) から pkg をインストール

### 任意ツール（PATH に追加しているディレクトリ用）
7. **Claude Code** - `~/.local/bin/claude` に配置される
   - インストール: `curl -fsSL https://claude.ai/install.sh | bash`

8. **arekore リポジトリ** - `bin/` 配下の自作コマンドを使う場合に必要
   - `cmux-setup` は cmux / yazi / lazygit / jq にも依存（[bin/README.md](../bin/README.md) 参照）

## 設定機能

### プロンプト設定
- ユーザー名、アーキテクチャ、パス、Git状態を表示
- Git リポジトリ内では `git_super_status` を表示
- カラー表示対応

### 履歴設定
- 履歴ファイル: `~/.zsh_history`
- メモリ上の履歴数: 10,000件
- ファイル保存履歴数: 10,000件
- 重複コマンドの除外
- インクリメンタル履歴保存

### キーバインド・エイリアス
- `Ctrl+S`: petによるコマンド検索
- `python` → `python3`
- `lg` → `lazygit`
- `pc` → `pet search | pbcopy`
- `c` → `container`
- `vim` → `nvim`
- `gfp` → `git fetch --prune`

### カスタム関数
- `prev()`: 直前のコマンドをpetに保存
- `pet-select()`: petでコマンド検索してバッファに挿入
- `git_prompt()`: Git状態に応じたプロンプト表示

## インストール確認コマンド
```bash
# 必要なツールが正しくインストールされているか確認
which asdf
which brew
which lazygit
which pet
which nvim
which container
which claude      # 任意
which cmux-setup  # 任意（arekore/bin）
brew list | grep zsh
```

## 設定ファイル全体（移行用）
以下は現在の`~/.zshrc`の完全なコードです（個人情報はマスクしています）：

```bash
PATH="${ASDF_DATA_DIR:-$HOME/.asdf}/shims:$PATH"
PATH="$PATH:/Applications/WezTerm.app/Contents/MacOS"
export PATH

# append completions to fpath
fpath=(${ASDF_DATA_DIR:-$HOME/.asdf}/completions $fpath)
# initialise completions with ZSH's compinit
autoload -Uz compinit && compinit

if type brew &>/dev/null; then
  FPATH=$(brew --prefix)/share/zsh-completions:$FPATH
  source $(brew --prefix)/share/zsh-autosuggestions/zsh-autosuggestions.zsh
  autoload -Uz compinit
  compinit
fi

alias python="python3"

autoload -Uz colors && colors
source $(brew --prefix)/opt/zsh-git-prompt/zshrc.sh

git_prompt() {
  if [ "$(git rev-parse --is-inside-work-tree 2> /dev/null)" = true ]; then
    PROMPT="%F{green}%n%f %F{cyan}($(arch))%f:%F{blue}%~%f $(git_super_status)"$'\n'"%# "
  else
    PROMPT="%F{green}%n%f %F{cyan}($(arch))%f:%F{blue}%~%f "$'\n'"%# "
  fi
}

precmd() {
  git_prompt
}

alias lg='lazygit'

HISTFILE=~/.zsh_history       # 履歴ファイルのパス
HISTSIZE=10000                # メモリ上の履歴数
SAVEHIST=10000                # ファイルに保存する履歴数

setopt HIST_IGNORE_DUPS       # 同じコマンドは重複保存しない
setopt HIST_IGNORE_SPACE      # スペースで始まるコマンドは保存しない
setopt HIST_FIND_NO_DUPS      # 履歴検索時に重複をスキップ
setopt INC_APPEND_HISTORY     # コマンド実行後すぐ履歴ファイルに保存

function prev() {
  PREV=$(fc -lrn | head -n 1)
  sh -c "pet new `printf %q "$PREV"`"
}

function pet-select() {
  BUFFER=$(pet search --query "$LBUFFER")
  CURSOR=$#BUFFER
  zle redisplay
}
zle -N pet-select
stty -ixon
bindkey '^s' pet-select

alias pc='pet search | pbcopy'
alias c='container'
alias vim='nvim'
alias gfp='git fetch --prune'
export PATH="$HOME/.local/bin:$PATH"
export PATH="$HOME/projects/github/<GITHUB_USER>/arekore/bin:$PATH"
```

> `<GITHUB_USER>` は arekore リポジトリを clone したユーザー名に置き換えてください。

### 移行手順
1. 新しいPCで上記の内容を`~/.zshrc`に保存
2. 必要なツールをインストール：
   ```bash
   # Homebrew
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   
   # 必要なパッケージ
   brew install zsh-completions zsh-autosuggestions zsh-git-prompt
   brew install lazygit neovim
   brew install knqyf263/pet/pet
   
   # asdf
   brew install asdf

   # Claude Code（任意）
   curl -fsSL https://claude.ai/install.sh | bash
   ```
3. 自作コマンドを使う場合は arekore リポジトリを clone（`<GITHUB_USER>` は置き換え）：
   ```bash
   mkdir -p ~/projects/github/<GITHUB_USER>
   git clone https://github.com/<GITHUB_USER>/arekore.git ~/projects/github/<GITHUB_USER>/arekore
   ```
4. ターミナルを再起動するか`source ~/.zshrc`を実行
5. 各ツールが正常に動作することを確認（上記インストール確認コマンド参照）

---
**最終更新日時**: 2026-10-27