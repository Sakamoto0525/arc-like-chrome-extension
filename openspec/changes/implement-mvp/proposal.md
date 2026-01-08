# Change: Arc-like Chrome 拡張機能 MVP 実装

## Why
10 の capability specs が確定したので、これらの仕様に基づいて MVP を実装する。プロジェクト作成から全機能の実装までを段階的に進める。

## What Changes
- Chrome 拡張機能（Manifest V3）のプロジェクト基盤を構築
- Storage Layer（chrome.storage.local + Zod）を実装
- Domain 層（型定義 + Selectors）を実装
- Side Panel UI（React）を実装
- 全ての CRUD 操作を実装
- Tab Opening 機能を実装

## Impact
- Affected specs: 全 10 specs の実装
- Affected code: 新規プロジェクト作成

## Phases

### Phase 1: プロジェクト基盤
プロジェクト構造、ビルド環境、Chrome 拡張の基本設定

### Phase 2: Storage Layer
データ永続化、スキーマ定義、初期データ生成

### Phase 3: Domain 層
型定義、Selectors、ビジネスロジック

### Phase 4: UI 骨格
Side Panel の基本構造、各セクションのレイアウト

### Phase 5: Space 管理
Space CRUD、切り替え機能

### Phase 6: Folder 管理
Folder CRUD、Tree 構造、展開/折りたたみ

### Phase 7: Item 管理
Item CRUD、Today Tabs / Saved Items 操作

### Phase 8: Pins
Pins 追加/削除、表示

### Phase 9: Search
インクリメンタル検索、フィルタリング

### Phase 10: Tab Opening
新規タブで開く、Today Tabs 自動追加

## 実装順序の根拠
1. **依存関係**: Storage → Domain → UI → Features
2. **検証可能性**: 各 Phase 終了時に動作確認できる
3. **MVP スコープ**: design-decisions の Non-Goals を守る

