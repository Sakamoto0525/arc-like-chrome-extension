# Tasks: Arc-like Chrome 拡張機能 MVP 実装

---

## Phase 1: プロジェクト基盤

### 1.1 プロジェクト初期化
- [ ] 1.1.1 pnpm init でプロジェクト作成
- [ ] 1.1.2 TypeScript 設定（tsconfig.json, strict mode）
- [ ] 1.1.3 Vite 設定（vite.config.ts, Chrome 拡張用）
- [ ] 1.1.4 React 依存関係インストール（react, react-dom, @types/react, @types/react-dom）

### 1.2 Chrome 拡張基盤
- [ ] 1.2.1 manifest.json 作成（Manifest V3, side_panel, storage 権限）
- [ ] 1.2.2 Background Service Worker 雛形（src/background/index.ts）
- [ ] 1.2.3 Side Panel HTML/エントリポイント（src/sidepanel/index.html, main.tsx）
- [ ] 1.2.4 ビルド & 拡張読み込み動作確認

**検証**: Chrome に拡張をロードし、Side Panel が開くことを確認

---

## Phase 2: Storage Layer

### 2.1 スキーマ定義
- [ ] 2.1.1 Zod インストール
- [ ] 2.1.2 AppState スキーマ定義（src/storage/schema.ts）
  - version: number
  - spaces: { byId, allIds }
  - folders: { byId, allIds }
  - items: { byId, allIds }
  - pins: { bySpaceId }
  - ui: { activeSpaceId, expandedFolderIds }
- [ ] 2.1.3 Entity スキーマ定義（Space, Folder, Item）

### 2.2 Storage ラッパー
- [ ] 2.2.1 Storage ラッパー実装（src/storage/storage.ts）
  - get(): Promise<AppState>
  - set(state: AppState): Promise<void>
  - Zod バリデーション統合
- [ ] 2.2.2 初期データ生成関数（createInitialState）
- [ ] 2.2.3 Storage 読み込み時の初期化処理

**検証**: DevTools で chrome.storage.local にデータが保存されることを確認

---

## Phase 3: Domain 層

### 3.1 型定義
- [ ] 3.1.1 Space 型定義（src/domain/types.ts）
  - id, name, createdAt
- [ ] 3.1.2 Folder 型定義
  - id, name, spaceId, parentId (null | string)
- [ ] 3.1.3 Item 型定義
  - id, url, title, faviconUrl, bucket ("today" | "saved"), folderId (null | string), spaceId
- [ ] 3.1.4 NormalizedState 型定義

### 3.2 Selectors
- [ ] 3.2.1 selectActiveSpace: アクティブ Space 取得
- [ ] 3.2.2 selectFolderTree: Space 内の Folder を木構造で取得
- [ ] 3.2.3 selectTodayTabs: Space 内の Today Tabs 取得（bucket="today"）
- [ ] 3.2.4 selectPins: Space の Pins（Item 参照）取得
- [ ] 3.2.5 selectItemsInFolder: Folder 内の Items 取得
- [ ] 3.2.6 selectSearchResults: 検索クエリでフィルタ

**検証**: 単体テストまたは console.log で Selector 動作確認

---

## Phase 4: UI 骨格

### 4.1 基本構造
- [ ] 4.1.1 App コンポーネント作成（src/sidepanel/App.tsx）
- [ ] 4.1.2 AppState のロード & React Context/State 管理
- [ ] 4.1.3 犬アイコン SVG 作成（src/assets/dog-icon.svg）

### 4.2 セクションレイアウト
- [ ] 4.2.1 Header セクション雛形
- [ ] 4.2.2 Search セクション雛形
- [ ] 4.2.3 Pins セクション雛形
- [ ] 4.2.4 NewTab ボタン雛形
- [ ] 4.2.5 TodayTabs セクション雛形
- [ ] 4.2.6 FolderTree セクション雛形

### 4.3 スタイリング
- [ ] 4.3.1 基本 CSS 設定（src/sidepanel/styles.css）
- [ ] 4.3.2 セクション順序の確認（Header → Search → Pins → NewTab → TodayTabs → FolderTree）

**検証**: Side Panel に全セクションが正しい順序で表示されることを確認

---

## Phase 5: Space 管理

### 5.1 Space 表示
- [ ] 5.1.1 Header に現在の Space 名表示
- [ ] 5.1.2 Space Switcher（ドロップダウン）実装

### 5.2 Space CRUD
- [ ] 5.2.1 Space 作成機能（UI + ロジック）
- [ ] 5.2.2 Space 切り替え機能（activeSpaceId 更新）
- [ ] 5.2.3 Space 名前変更機能
- [ ] 5.2.4 Space 削除機能（関連データも削除）

**検証**: Space 作成/切り替え/削除が動作し、リロード後も状態が維持されることを確認

---

## Phase 6: Folder 管理

### 6.1 Folder Tree 表示
- [ ] 6.1.1 FolderTree コンポーネント実装（再帰的レンダリング）
- [ ] 6.1.2 Folder 展開/折りたたみ UI
- [ ] 6.1.3 展開状態の永続化（expandedFolderIds）
- [ ] 6.1.4 Folder 内 Item 表示

### 6.2 Folder CRUD
- [ ] 6.2.1 Folder 作成機能（ルート / サブフォルダ）
- [ ] 6.2.2 Folder 名前変更機能
- [ ] 6.2.3 Folder 削除機能（子孫も削除）

**検証**: Folder 階層の作成/展開/削除が動作することを確認

---

## Phase 7: Item 管理

### 7.1 Item 表示
- [ ] 7.1.1 Item コンポーネント実装（favicon + title）
- [ ] 7.1.2 favicon フォールバック（犬アイコン）実装
- [ ] 7.1.3 Today Tabs リスト表示（×ボタン付き）
- [ ] 7.1.4 Folder 内 Saved Items 表示

### 7.2 Today Tabs 操作
- [ ] 7.2.1 Today Tab 削除（×ボタン）
- [ ] 7.2.2 Today Tab → Folder 保存（メニュー操作）

### 7.3 Item CRUD
- [ ] 7.3.1 Item 作成（URL を Folder に保存）
- [ ] 7.3.2 Item 名前変更
- [ ] 7.3.3 Item 削除（関連 Pin 参照も削除）
- [ ] 7.3.4 Item 移動（メニューで Folder 変更）

### 7.4 コンテキストメニュー
- [ ] 7.4.1 Item 用コンテキストメニュー実装
  - 名前変更
  - 移動
  - 削除
  - Pin に追加/削除
  - Folder に保存（Today Tab のみ）

**検証**: Item の作成/削除/移動が動作し、Today Tabs ⇔ Saved Items の変換ができることを確認

---

## Phase 8: Pins

### 8.1 Pins 表示
- [ ] 8.1.1 Pins アイコン列表示（横並び）
- [ ] 8.1.2 Pin クリックで URL を開く（+ Today Tabs 追加）

### 8.2 Pins 操作
- [ ] 8.2.1 Pin 追加機能（Item メニューから）
- [ ] 8.2.2 Pin 削除機能（Pin 右クリックメニュー）

**検証**: Pins への追加/削除が動作し、クリックで新規タブが開くことを確認

---

## Phase 9: Search

### 9.1 検索 UI
- [ ] 9.1.1 Search 入力フィールド実装
- [ ] 9.1.2 検索クエリ状態管理

### 9.2 検索ロジック
- [ ] 9.2.1 インクリメンタル検索実装（タイトル + URL）
- [ ] 9.2.2 検索結果表示（Today Tabs + Saved Items 混合）
- [ ] 9.2.3 検索結果クリックで URL を開く（+ Today Tabs 追加）
- [ ] 9.2.4 検索クリアで通常表示に戻る

**検証**: 検索入力でリアルタイムフィルタが動作することを確認

---

## Phase 10: Tab Opening

### 10.1 新規タブで開く
- [ ] 10.1.1 openUrl 関数実装（chrome.tabs.create）
- [ ] 10.1.2 Saved Item クリック → 起点 URL を新規タブで開く
- [ ] 10.1.3 Today Tab クリック → URL を新規タブで開く
- [ ] 10.1.4 Pin クリック → URL を新規タブで開く
- [ ] 10.1.5 検索結果クリック → URL を新規タブで開く

### 10.2 Today Tabs 自動追加
- [ ] 10.2.1 URL を開いた時に Today Tabs 先頭に追加
- [ ] 10.2.2 重複 URL の処理（既存を先頭に移動）

### 10.3 New Tab ボタン
- [ ] 10.3.1 New Tab ボタン実装（chrome://newtab を開く）

**検証**: 全ての箇所から URL を開くと新規タブで開き、Today Tabs に追加されることを確認

---

## Phase 11: 最終確認

### 11.1 統合テスト
- [ ] 11.1.1 全機能の動作確認
- [ ] 11.1.2 design-decisions の Non-Goals が実装されていないことを確認
- [ ] 11.1.3 ブラウザ再起動後のデータ永続化確認

### 11.2 コード品質
- [ ] 11.2.1 TypeScript エラーがないことを確認
- [ ] 11.2.2 不要な console.log の削除

**検証**: MVP 全体が仕様通りに動作することを確認

---

## 依存関係マップ

```
Phase 1 (プロジェクト基盤)
    ↓
Phase 2 (Storage Layer)
    ↓
Phase 3 (Domain 層)
    ↓
Phase 4 (UI 骨格)
    ↓
┌───┴───┬───────┬───────┐
↓       ↓       ↓       ↓
Phase 5 Phase 6 Phase 7 Phase 8
(Space) (Folder)(Item)  (Pins)
    └───────┬───────────┘
            ↓
        Phase 9 (Search)
            ↓
        Phase 10 (Tab Opening)
            ↓
        Phase 11 (最終確認)
```

## 並列化可能なタスク
- Phase 5, 6, 7, 8 は Phase 4 完了後に並列実行可能
- 各 Phase 内の UI 実装とロジック実装は分担可能

