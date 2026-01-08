# Project Context

## Purpose
Arc ブラウザの「Spaces / Folders / Pins / Today Tabs」体験を、Google Chrome の拡張機能（Manifest V3）で再現する。Arc の Split View / Peek は不要。実用性重視。

## Tech Stack
- Platform: Chrome Extension (Manifest V3)
- UI: Side Panel
- Language: TypeScript
- Framework: React
- Build: Vite
- Package Manager: pnpm
- Storage: chrome.storage.local (key: arcish_state)
- Schema Validation: Zod

## Project Conventions

### Code Style
- TypeScript strict mode
- React functional components with hooks
- Normalized data structure for state management

### Architecture Patterns
- Side Panel UI (React)
- Background Service Worker for tab operations
- Storage layer with Zod validation
- Domain selectors for data transformation

### Testing Strategy
[TBD - to be defined during implementation]

### Git Workflow
[TBD - to be defined]

## Domain Context

### Core Concepts
- **Space**: 作業コンテキスト（Work / Private など）
- **Folder**: Space 内の階層構造（ディレクトリ）
- **Item (Saved Item / 論理タブ)**: 保存された URL（起点URL）
- **Pins**: Space ごとのお気に入り（アイコン列）
- **Today Tabs**: 未整理の作業中URL一覧（自動では消さない）

### Key Design Decisions
1. 管理対象は「論理タブ（Saved Items）」のみ。Chrome 実タブは置き換えない。
2. Today Tabs は「拡張経由で開いたもののみ」自動追加。Chrome本体の + やアドレスバー入力は対象外。
3. Today Tabs は自動で消さない（ユーザーが明示的に削除）。
4. 保存Item（Folder内Item）は「起点URL」。クリックすると常に起点URLを新規タブで開く（迷子リセット）。
5. favicon が取れない場合は犬アイコンを表示する。

## Important Constraints

### Non-Goals (MVP)
- Split View
- Arc Peek
- Chrome 実タブの完全同期/置換
- Today Tabs の自動アーカイブ
- 実タブから論理タブを推測して自動生成

## External Dependencies
- Chrome Extension APIs (tabs, storage, sidePanel)
