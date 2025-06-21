# Claude Code カスタムコマンド

このディレクトリには、このプロジェクト専用のClaude Codeカスタムスラッシュコマンドが含まれています。

## 利用可能なコマンド

### `/project:update-settings`

設定ファイルから各種ツールの設定を読み込み、日本語のマークダウンファイルを生成・更新します。

**使用方法:**
```
/project:update-settings
```

**機能:**
- `~/.config/nvim/init.vim` から `settings/nvim.md` を更新
- `~/.wezterm.lua` から `settings/wezterm.md` を更新  
- `~/.zshrc` から `settings/zsh.md` を更新

**生成される内容:**
- 設定ファイルの場所
- 現在の設定内容の要約（日本語）
- 必要なツール・依存関係の情報
- インストール・セットアップコマンド
- 最終更新日時

## 使用例

Claude Codeで以下のようにコマンドを実行してください：

```
/project:update-settings
```

これにより、最新の設定ファイルから自動的にドキュメントが更新されます。