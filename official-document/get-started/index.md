# Get started

- Flutter本体のインストール方法
  - Windows / macOS / Linux / Chrome OS（！）
- エディタの設定
  - Android Studio and IntelliJ
  - VS Code
  - Emacs
- テスト起動
  - iOSシミュレーターを利用
  - CLIで雛形を生成
- サンプルアプリケーションの開発
  - Startup Name Generator
    - [Part2](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt2#0)
- Learn more
  - 他の技術の利用者向けのガイド
    - [Flutter for React Native developers](https://flutter.dev/docs/get-started/flutter-for/react-native-devs)
    - [Flutter for web developers](https://flutter.dev/docs/get-started/flutter-for/web-devs)
- From another platform?
  - 他FW利用者向けのガイド

# Introduction to declarative UI

- https://flutter.dev/docs/get-started/flutter-for/declarative
- declarative UI = 宣言的UI
- inperative UI = 命令的UI
- declarative styleとimperative styleの比較
- declarative UIを使う理由
  - Win32・Web・Android・iOSではimperative styleを使う
  - UIView等々を使ってUIを構築し、メソッドやsetterを使って変更する
  - 様々なUIの状態の間のtransitionの実装の工数を軽くするため、Flutterでは開発者は現在のUIの状態のみを定義し、transitionはフレームワークに任せる
  - UI構築にリソースを割かなくてもよくなる
- declarative frameworkでUIがどう変わるか？
  - imperative styleではセレクタで要素を取得し、変更を適用する必要がある
  - declarative styleでは、ビュー（Flutterで言うWidget）の設定はイミュータブル
  - UIを変更するには、Widgetは自身でrebuildを実行することでWidgetのツリーを再構築する
    - Flutterでは一般的に `StatefulWidgets` の `setState()` を叩くことで実行
  - 既存のコンポーネントインスタンスを変更するのではなく、Flutterは新しいインスタンスを作成する

# Building a web application with Flutter

- https://flutter.dev/docs/get-started/web
- FlutterはまだWebを正式サポートしていない
  - Betaチャンネルを利用する必要あり
- Flutter Webの現状について
  - [Flutter Webの現状調査](https://ntaoo.hatenablog.com/entry/2019/05/22/152739) 
    - 2019/05/22
    - テキスト以外はほとんどCanvasでレンダリング
    - Custom ElementとCanvasのカタマリ
    - 位置はabsolute positionやtransformなどで調整
