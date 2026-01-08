# Storage

## ADDED Requirements

### Requirement: chrome.storage.local の使用
データ永続化には chrome.storage.local を使用するものとする（SHALL）。ストレージキーは "arcish_state"。

#### Scenario: データ保存
- **GIVEN** ユーザーがデータを変更する
- **WHEN** 変更が確定される
- **THEN** chrome.storage.local の "arcish_state" キーにデータが保存される

#### Scenario: データ読み込み
- **GIVEN** 拡張が起動する
- **WHEN** Side Panel が初期化される
- **THEN** chrome.storage.local から "arcish_state" を読み込んで状態を復元する

### Requirement: Storage ラッパー
chrome.storage.local の薄いラッパーを用意し、set/get の I/F を統一するものとする（SHALL）。

#### Scenario: Storage ラッパー使用
- **GIVEN** UI コンポーネントがデータを操作する
- **WHEN** データの読み書きを行う
- **THEN** 統一された get/set API を通じてアクセスする

### Requirement: Zod によるスキーマバリデーション
ストレージに保存するデータは Zod でスキーマバリデーションを行うものとする（SHALL）。

#### Scenario: データ保存時のバリデーション
- **GIVEN** データを保存しようとする
- **WHEN** データが Zod スキーマに適合しない
- **THEN** エラーが発生し、不正なデータは保存されない

### Requirement: Normalized データ構造
データは正規化（Normalized）された構造で保存するものとする（SHALL）。

#### Scenario: Normalized 構造
- **GIVEN** データを保存する
- **WHEN** ストレージ構造を確認する
- **THEN** 以下の構造で保存されている:
  - version: number（スキーマバージョン）
  - spaces: { byId: {}, allIds: [] }
  - folders: { byId: {}, allIds: [] }
  - items: { byId: {}, allIds: [] }
  - pins: { bySpaceId: { [spaceId]: itemId[] } }
  - ui: { activeSpaceId, expandedFolderIds }

### Requirement: スキーマバージョン管理
ストレージに保存するデータには `version: number` フィールドを含めるものとする（SHALL）。初期値は `1` とする（SHALL）。将来のデータ構造変更時にマイグレーションを可能にするためのフィールドである。

#### Scenario: 初期バージョン
- **GIVEN** 拡張を初めてインストールする
- **WHEN** 初期データが作成される
- **THEN** version フィールドの値は 1 である

#### Scenario: バージョンの永続化
- **GIVEN** データがストレージに保存されている
- **WHEN** データを読み込む
- **THEN** version フィールドが含まれている

> **NOTE**: スキーマ変更が発生した場合、version を参照してマイグレーション処理を行う。MVP では version=1 のみをサポートし、マイグレーションロジックは将来の拡張として実装する。

### Requirement: 更新単位（全体保存）
MVP ではストレージは state 全体を1つのオブジェクトとして保存・更新するものとする（SHALL）。部分的な差分更新は行わないものとする（SHALL NOT）。

#### Scenario: 全体保存
- **GIVEN** ユーザーが Item を1つ追加する
- **WHEN** ストレージを更新する
- **THEN** state 全体が1つのオブジェクトとして chrome.storage.local に保存される

#### Scenario: 差分更新の禁止
- **GIVEN** state の一部のみが変更された
- **WHEN** ストレージを更新する
- **THEN** 変更された部分だけでなく、state 全体を保存する
- **AND** 部分的なキー更新（例: spaces のみ更新）は行わない

> **NOTE**: 全体保存はシンプルさを優先した MVP 方針である。パフォーマンス上の問題が発生した場合、将来的に差分更新を検討する。

### Requirement: UI 状態の永続化
UI 状態（activeSpaceId、expandedFolderIds など）も同じストアに含めて永続化するものとする（SHALL）。

#### Scenario: UI 状態の保存
- **GIVEN** ユーザーが Folder を展開する
- **WHEN** Side Panel を閉じて再度開く
- **THEN** 展開状態が復元される

#### Scenario: アクティブ Space の保存
- **GIVEN** ユーザーが Space を切り替える
- **WHEN** ブラウザを再起動する
- **THEN** 最後にアクティブだった Space が選択される

### Requirement: 初期データ
ストレージが空の場合、デフォルトの初期データを生成するものとする（SHALL）。

#### Scenario: 初回起動時の初期化
- **GIVEN** 拡張を初めてインストールする
- **WHEN** ストレージが空である
- **THEN** デフォルトの Space（例: "Default"）と空の構造が作成される

