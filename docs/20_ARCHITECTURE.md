# Architecture（MV3 + Side Panel + React）

## 概要
- Manifest V3 の Chrome 拡張として実装する。
- UI は Side Panel 上に React アプリとして構築する。
- 永続データは chrome.storage.local に保存する（key: arcish_state）。
- UI 状態（activeSpaceId / folder展開状態など）も同じストアに含める。

## 主要コンポーネント
### 1) Background（Service Worker）
- 役割：タブ作成、必要なら権限系の処理、メッセージ中継
- 主な責務：
  - URLを新規タブで開く（tabs.create）
  - （将来）権限依存処理の集約
- UIからのリクエストは message 経由で受けることを想定（最初はUI側から直接 tabs API を呼べるならそれでも可）

### 2) Side Panel UI（React）
- ユーザー操作は基本ここで完結。
- 表示セクション：Search / Pins / New Tab / Today Tabs / Folder Tree
- 操作イベントは store（storage wrapper）へ書き込み→再描画。

### 3) Storage Layer
- chrome.storage.local の薄いラッパを用意する。
- set/get のI/Fを統一し、zod でバリデーション＆将来のマイグレーションに備える。

### 4) Domain / Selectors
- entities（Space/Folder/Item/Pin）型定義
- FolderTree を組み立てる selector
- Search filter selector

## データモデル（Normalized 推奨）
- spaces: byId / allIds
- folders: byId / allIds
- items: byId / allIds
- pins: bySpaceId[spaceId] = itemId[]
- ui: activeSpaceId, expandedFolderIds
- item.bucket: "today" | "saved"（today tabs か folder tree かの分類）

## MVPの実装順（推奨）
1. ストア（schema + get/set + 初期データ）
2. selector（Space内のFolderTree構築 / Today抽出 / Pins抽出 / Search）
3. UI骨格（セクション配置）
4. CRUD（Space/Folder/Item/Pin/Today削除）
5. タブオープン（常に新規タブで開く + Today先頭追加）
