# Tasks: Arc-like Chrome拡張機能の初期仕様定義

## 1. プロジェクト基盤
- [ ] 1.1 Manifest V3 + Side Panel の Chrome 拡張プロジェクト構造を作成
- [ ] 1.2 Vite + React + TypeScript のビルド環境を構築
- [ ] 1.3 pnpm で依存関係を管理

## 2. Storage Layer
- [ ] 2.1 chrome.storage.local のラッパーを実装
- [ ] 2.2 Zod でスキーマバリデーションを実装
- [ ] 2.3 初期データ構造（Normalized）を定義

## 3. Domain / Entities
- [ ] 3.1 Space / Folder / Item / Pin の型定義を作成
- [ ] 3.2 FolderTree 構築 selector を実装
- [ ] 3.3 Search filter selector を実装
- [ ] 3.4 Today Tabs 抽出 selector を実装
- [ ] 3.5 Pins 抽出 selector を実装

## 4. UI 骨格
- [ ] 4.1 Side Panel のセクション配置を実装
- [ ] 4.2 Header（Space Switcher）コンポーネントを実装
- [ ] 4.3 Search 入力コンポーネントを実装
- [ ] 4.4 Pins アイコン列コンポーネントを実装
- [ ] 4.5 New Tab ボタンを実装
- [ ] 4.6 Today Tabs リストを実装
- [ ] 4.7 Folder Tree コンポーネントを実装

## 5. CRUD 操作
- [ ] 5.1 Space CRUD を実装
- [ ] 5.2 Folder CRUD を実装
- [ ] 5.3 Item CRUD を実装
- [ ] 5.4 Pin 追加/削除を実装
- [ ] 5.5 Today Tab 削除を実装

## 6. Tab Opening
- [ ] 6.1 常に新規タブで URL を開く機能を実装
- [ ] 6.2 拡張経由で開いた URL を Today Tabs 先頭に追加

