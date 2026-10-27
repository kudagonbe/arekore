# bin

パスを通して使う自作コマンド置き場。

## セットアップ

`~/.zshrc` に以下を追加する。

```zsh
export PATH="$HOME/projects/github/kudagonbe/arekore/bin:$PATH"
```

## コマンド一覧

### cmux-setup

開発用レイアウトの cmux ワークスペースを新規作成する。cmux のターミナル内で実行する。

```zsh
cmux-setup            # カレントディレクトリで作業する
cmux-setup <name>     # worktree を作って作業する
```

```
┌──────────────┬──────────────┐
│              │ yazi|lazygit │  右上: yazi / lazygit の2サーフェス（6）
│ Claude Code  │              │
│              ├──────────────┤
│              │ terminal     │  右下: コマンド実行用（4）
└──────────────┴──────────────┘
```

- 左右の比率は 5:5
- ワークスペース名: 引数なしは `⚡<フォルダ名>`、引数ありは `⚡<リポジトリ名> (<name>)`
- 引数ありの場合
  - `<repo>/.claude/worktrees/<name>` に worktree を作成し、全ペインをそこで起動する
  - ブランチ `<name>` が無ければ現在の HEAD から作成、あればそのブランチを使う
  - worktree が既にあれば再利用する
  - `.claude/worktrees/` を `.git/info/exclude` に追加する
- 依存: `cmux` `claude` `yazi` `lazygit` `jq` `git`
