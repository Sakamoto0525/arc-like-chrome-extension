# Change: Arc-like Chrome拡張機能の初期仕様定義

## Why
Arc ブラウザの「Spaces / Folders / Pins / Today Tabs」体験を Chrome 拡張機能（Manifest V3）で再現するプロジェクトの MVP 仕様を、OpenSpec 形式で機能ごとに整理・定義する。

## What Changes
- 10の機能別仕様（capability specs）を追加:
  - `design-decisions`: **横断的設計判断（他の spec より先に読む前提仕様）**
  - `space-management`: Space の CRUD と切り替え
  - `folder-management`: Folder 階層構造の管理
  - `item-management`: Saved Items（論理タブ）の管理
  - `pins`: Space ごとのお気に入りアイコン列
  - `today-tabs`: 未整理の作業中 URL 一覧
  - `search`: 全体検索機能
  - `tab-opening`: URL を新規タブで開く動作
  - `storage`: chrome.storage.local によるデータ永続化
  - `side-panel-ui`: Side Panel の UI 構成

## Impact
- Affected specs: 新規作成（既存 spec なし）
- Affected code: なし（仕様定義のみ）

