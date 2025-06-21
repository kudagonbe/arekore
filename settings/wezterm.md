# WezTerm設定まとめ

## 設定ファイルの場所
`~/.wezterm.lua`

## 基本設定

### 自動更新・リロード機能
- **設定ファイル自動リロード**: 有効 (`automatically_reload_config = true`)
- **WezTerm更新チェック**: 有効、24時間間隔 (`check_for_updates = true`)

### システム設定
- **ターミナルベル**: 無効 (`audible_bell = "Disabled"`)
- **IME**: 有効 (`use_ime = true`) - 日本語入力対応

## 外観設定

### カラーテーマ
- **カラースキーム**: `Belge (terminal.sexy)`

### 背景・透過効果
- **背景透過度**: 65% (`window_background_opacity = 0.65`)
- **macOS背景ぼかし**: 30レベル (`macos_window_background_blur = 30`)
- **背景グラデーション**: 黒色ベース (`colors = { "#000000" }`)

### ウィンドウデザイン
- **ウィンドウ装飾**: リサイズのみ (`window_decorations = "RESIZE"`)
  - タイトルバーを非表示にしてクリーンな見た目
- **タブバー**: 透明背景 (`inactive_titlebar_bg = "none"`, `active_titlebar_bg = "none"`)

## タブカスタマイズ

### タブデザイン
- **タブ形状**: Nerd Fontsの三角形アイコンを使用
  - 左矢印: `ple_lower_right_triangle`
  - 右矢印: `ple_upper_left_triangle`

### タブ色設定
- **非アクティブタブ**:
  - 背景色: `#5c6d74` (グレーブルー)
  - 文字色: `#FFFFFF` (白)
- **アクティブタブ**:
  - 背景色: `#ae8b2d` (ゴールド)
  - 文字色: `#FFFFFF` (白)

## 特徴的な機能

### カスタムタブタイトル
- `format-tab-title`イベントでタブの見た目をカスタマイズ
- タブタイトルの両端にスペースを追加してパディング調整
- アクティブ/非アクティブでの色分け

### 日本語対応
- IME有効化により日本語入力が可能
- 設定ファイル内のコメントも日本語で記述

## 全体的な特徴
この設定は**視覚的に美しく機能的**なターミナル環境を提供します：
- 透過背景とぼかし効果でモダンな見た目
- カスタムタブデザインで視認性向上
- 日本語入力対応で実用性重視
- 自動更新機能で最新機能を活用

## 設定ファイル全体（移行用）
以下は現在の`~/.wezterm.lua`の完全なコードです：

```lua
local wezterm = require 'wezterm'

local config = {}

if wezterm.config_builder then
  config = wezterm.config_builder()
end

-- wezterm.luaの更新を検知して自動的にreload
config.automatically_reload_config = true

-- WezTermの更新検知
config.check_for_updates = true
config.check_for_updates_interval_seconds = 86400

-- Terminalのbellを止める
config.audible_bell = "Disabled"

-- IMEの有効化
config.use_ime = true

-- カラースキームの設定
config.color_scheme = 'Belge (terminal.sexy)'

-- 背景透過と背景ぼかし
config.window_background_opacity = 0.65
config.macos_window_background_blur = 30

-- タイトルバーの削除
config.window_decorations = "RESIZE"

-- タブバーを透明にする
config.window_frame = {
  inactive_titlebar_bg = "none",
  active_titlebar_bg = "none",
}

-- タブバーの色を設定する
config.window_background_gradient = {
  colors = { "#000000" },
}

-- タブの形
local SOLID_LEFT_ARROW = wezterm.nerdfonts.ple_lower_right_triangle
local SOLID_RIGHT_ARROW = wezterm.nerdfonts.ple_upper_left_triangle

-- アクティブタブに色をつける
wezterm.on("format-tab-title", function(tab, tabs, panes, config, hover, max_width)
  local background = "#5c6d74"
  local foreground = "#FFFFFF"
  local edge_background = "none"

  if tab.is_active then
    background = "#ae8b2d"
    foreground = "#FFFFFF"
  end

  local edge_foreground = background

  local title = "   " .. wezterm.truncate_right(tab.active_pane.title, max_width - 1) .. "   "

  return {
    { Background = { Color = edge_background } },
    { Foreground = { Color = edge_foreground } },
    { Text = SOLID_LEFT_ARROW },
    { Background = { Color = background } },
    { Foreground = { Color = foreground } },
    { Text = title },
    { Background = { Color = edge_background } },
    { Foreground = { Color = edge_foreground } },
    { Text = SOLID_RIGHT_ARROW },
  }
end)

return config
```

### 移行手順
1. 新しいPCで上記の内容を`~/.wezterm.lua`に保存
2. WezTermをインストール（[公式サイト](https://wezfurlong.org/wezterm/)からダウンロード）
3. Nerd Fonts対応フォントをインストール（タブアイコン表示のため）
4. 設定は自動で反映されます（`automatically_reload_config = true`のため）

---
**最終更新日時**: 2025-06-21