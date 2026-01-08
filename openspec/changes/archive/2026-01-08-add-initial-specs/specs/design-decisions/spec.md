# Design Decisions（横断的設計判断）

> **この spec は他の全ての capability specs を読む前に必ず読む前提仕様である。**
> 全ての capability specs はこの design-decisions に従うものとする（SHALL）。

## ADDED Requirements

### Requirement: 管理対象の定義
システムが管理・表示するのは「論理タブ（Saved Items / Today Tabs）」のみとする（SHALL）。Chrome の実タブバーを置き換えてはならない（SHALL NOT）。実タブを自動分類・自動同期してはならない（SHALL NOT）。

#### Scenario: 論理タブのみ管理
- **GIVEN** ユーザーが Chrome で複数のタブを開いている
- **WHEN** Side Panel を開く
- **THEN** 拡張が管理する論理タブ（Saved Items / Today Tabs）のみが表示される
- **AND** Chrome の実タブ一覧は表示されない

#### Scenario: 実タブの自動分類禁止
- **GIVEN** ユーザーが Chrome 本体で新しいタブを開く
- **WHEN** 任意の URL に移動する
- **THEN** その実タブは自動的に Folder に分類されない
- **AND** 論理タブとして自動生成されない

### Requirement: Today Tabs への自動追加ルール
Today Tabs に自動追加するのは「拡張 UI 経由で開いた URL」のみとする（SHALL）。以下の操作から開いた URL は Today Tabs に追加されるものとする：
- New Tab ボタン
- Pins のクリック
- Folder 内 Saved Item のクリック
- 検索結果のクリック

#### Scenario: 拡張 UI 経由での Today Tabs 追加
- **GIVEN** ユーザーが Side Panel を開いている
- **WHEN** Pins / Saved Item / 検索結果のいずれかをクリックする
- **THEN** その URL が Today Tabs の先頭に追加される

#### Scenario: 重複 URL の処理
- **GIVEN** Today Tabs に URL A が既に存在する
- **WHEN** 同じ URL A を拡張 UI から開く
- **THEN** 新しい Item は作成されない
- **AND** 既存の Item が先頭に移動する

### Requirement: Today Tabs への自動追加の対象外
以下の操作で開いたタブは Today Tabs に自動追加してはならない（SHALL NOT）：
- Chrome 本体の「＋」ボタン（新規タブ）
- Chrome のアドレスバーへの URL 入力
- 外部アプリケーション（メール、Slack 等）からのリンク
- ブックマークからの直接オープン

#### Scenario: Chrome 本体からの操作は対象外
- **GIVEN** ユーザーが Chrome 本体で新規タブを開く
- **WHEN** アドレスバーに URL を入力して移動する
- **THEN** Today Tabs には追加されない

#### Scenario: 外部アプリからのリンクは対象外
- **GIVEN** ユーザーがメールアプリでリンクをクリックする
- **WHEN** Chrome でそのリンクが開かれる
- **THEN** Today Tabs には追加されない

### Requirement: Today Tabs の自動削除禁止
システムは Today Tabs を自動的に削除・アーカイブしてはならない（SHALL NOT）。Today Tabs の削除はユーザーの明示的な操作によってのみ行われるものとする（SHALL）。

#### Scenario: 時間経過による自動削除の禁止
- **GIVEN** Today Tabs に Item が1週間前から存在する
- **WHEN** 時間が経過する
- **THEN** その Item は自動で削除されない

#### Scenario: ユーザー操作による削除のみ許可
- **GIVEN** Today Tabs に Item が存在する
- **WHEN** ユーザーが×ボタンをクリックする
- **THEN** その Item が削除される

### Requirement: Saved Item のクリック挙動（迷子リセット）
Folder 内の Saved Item は「起点 URL」として固定されるものとする（SHALL）。Saved Item をクリックすると、常にその起点 URL を新規タブで開くものとする（SHALL）。実タブ内でどれだけページ遷移していても、再クリックで起点に戻れることを保証する。

#### Scenario: 起点 URL への復帰
- **GIVEN** Saved Item（起点URL: https://docs.example.com）が存在する
- **AND** ユーザーがそのタブ内で https://docs.example.com/page1/page2 まで遷移している
- **WHEN** ユーザーが Side Panel で同じ Saved Item をクリックする
- **THEN** https://docs.example.com が新規タブで開かれる

#### Scenario: 常に新規タブで開く
- **GIVEN** Saved Item が存在する
- **WHEN** ユーザーが Saved Item をクリックする
- **THEN** 既存タブを再利用せず、新規タブで起点 URL が開かれる

### Requirement: favicon フォールバック
favicon が取得できない場合は、デフォルトの犬アイコン（SVG）を表示するものとする（SHALL）。これは全ての Item 表示箇所（Pins / Today Tabs / Folder Tree / 検索結果）に適用される。

#### Scenario: favicon 取得成功
- **GIVEN** Item の URL から favicon が取得可能
- **WHEN** Item が表示される
- **THEN** 取得した favicon が表示される

#### Scenario: favicon 取得失敗時のフォールバック
- **GIVEN** Item の URL から favicon が取得できない
- **WHEN** Item が表示される
- **THEN** デフォルトの犬アイコンが表示される

### Requirement: URL を開く操作は常に新規タブ
拡張 UI から URL を開く全ての操作において、常に新規タブで開くものとする（SHALL）。既存タブの URL 置き換えや再利用は行わないものとする（SHALL NOT）。

#### Scenario: 全ての URL オープンは新規タブ
- **GIVEN** ユーザーが Side Panel で任意の Item をクリックする
- **WHEN** URL が開かれる
- **THEN** Chrome の新規タブとして開かれる
- **AND** 既存のタブは変更されない

### Requirement: Non-Goals（MVP で実装しない機能）
以下の機能は MVP では実装しないものとする（SHALL NOT）。これらに関連する仕様・UI・ロジックを含めてはならない。

- **Split View**: 画面分割表示
- **Arc Peek**: プレビューポップアップ
- **Chrome 実タブの完全同期/置換**: 実タブバーの代替
- **Today Tabs の自動アーカイブ**: 時間経過による自動整理
- **実タブから論理タブの自動生成**: 実タブを解析して Item を作成

#### Scenario: Non-Goals の機能は存在しない
- **GIVEN** ユーザーが Side Panel を開いている
- **WHEN** UI を確認する
- **THEN** Split View / Peek / 自動アーカイブ設定などの UI は存在しない

### Requirement: データの永続化先
全てのデータは chrome.storage.local に保存するものとする（SHALL）。ストレージキーは "arcish_state" を使用するものとする（SHALL）。外部サーバーへの同期は行わないものとする（SHALL NOT）。

#### Scenario: ローカルストレージのみ使用
- **GIVEN** ユーザーがデータを変更する
- **WHEN** データが保存される
- **THEN** chrome.storage.local にのみ保存される
- **AND** 外部サーバーには送信されない

### Requirement: capability specs の優先順位
この design-decisions spec は他の全ての capability specs に優先するものとする（SHALL）。各 capability spec がこの design-decisions と矛盾する場合、design-decisions の内容が優先される。

#### Scenario: 矛盾時の優先順位
- **GIVEN** capability spec に design-decisions と異なる記述がある
- **WHEN** 実装判断を行う
- **THEN** design-decisions の内容を優先する

---

## Summary: 横断ルール一覧

| カテゴリ | ルール | 根拠 |
|---------|--------|------|
| 管理対象 | 論理タブのみ。実タブは管理しない | Chrome との責務分離 |
| Today Tabs 追加 | 拡張 UI 経由のみ | ユーザーの意図を尊重 |
| Today Tabs 削除 | 自動削除禁止。手動のみ | データ損失防止 |
| Saved Item クリック | 常に起点 URL を新規タブで開く | 迷子リセット |
| URL オープン | 常に新規タブ | 既存作業の保護 |
| favicon | 取得不可時は犬アイコン | 一貫した UI |
| データ保存 | chrome.storage.local のみ | プライバシー・オフライン対応 |
| Non-Goals | Split View / Peek / 自動アーカイブ等 | MVP スコープ |

