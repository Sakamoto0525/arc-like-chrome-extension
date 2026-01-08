# Tab Opening

## ADDED Requirements

### Requirement: 常に新規タブで開く
拡張 UI から URL を開く場合、常に新規タブで開くものとする（SHALL）。既存タブの再利用やタブの置き換えは行わない。

#### Scenario: URL を新規タブで開く
- **GIVEN** ユーザーが Side Panel を開いている
- **WHEN** Item、Pin、検索結果、New Tab ボタンのいずれかをクリックして URL を開く
- **THEN** Chrome の新規タブで URL が開かれる

### Requirement: New Tab ボタン
Side Panel に New Tab ボタンを配置し、クリックすると空の新規タブ（chrome://newtab）を開くものとする（SHALL）。

#### Scenario: New Tab ボタンクリック
- **GIVEN** Side Panel が開いている
- **WHEN** ユーザーが New Tab ボタンをクリックする
- **THEN** chrome://newtab が新規タブで開かれる

### Requirement: Tab 作成時の Today Tabs 追加
拡張 UI 経由で URL を開いた場合、その URL を Today Tabs の先頭に追加するものとする（SHALL）。

#### Scenario: Tab 作成と Today Tabs 追加
- **GIVEN** Folder 内に Item（URL: https://example.com）が存在する
- **WHEN** ユーザーが Item をクリックする
- **THEN** https://example.com が新規タブで開かれる
- **AND** Today Tabs の先頭に追加される（bucket="today" の Item として）

### Requirement: 重複 URL の処理
既に Today Tabs に同じ URL が存在する場合、重複作成せず既存 Item を先頭に移動するものとする（SHALL）。

#### Scenario: 重複 URL を開く
- **GIVEN** Today Tabs に URL A の Item が存在する
- **WHEN** ユーザーが同じ URL A を別の場所から開く
- **THEN** 新規タブは開かれる
- **AND** 既存の Today Tab Item が先頭に移動する（重複作成しない）

### Requirement: Background Service Worker での Tab 作成
タブ作成は Background Service Worker（tabs.create API）を通じて行うものとする（SHALL）。

#### Scenario: tabs.create 呼び出し
- **GIVEN** ユーザーが URL を開く操作を行う
- **WHEN** Side Panel が chrome.tabs.create を呼び出す
- **THEN** 新規タブが作成される

