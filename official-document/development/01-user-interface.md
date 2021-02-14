# Intoroduction to widgets

- FlutterはReactの影響を受けている
- UIはWidgetから構築する
- Widgetは見た目と現在の設定と状態を表現する
- 状態が変わるとき、Widgetは再構築される
  - Flutterが必要最小限のパッチを計算する
- 画面を覆うルートWidgetが必ず必要
- アプリを構築する中で、 `StatelessWidget` か `StatefulWidget` のどちらかのサブクラスを使う
  - 状態を管理するかどうかで分かれる
- Widgetのメインの仕事は `build()` 関数を実装すること
  - 子のWidgetのレイアウト
- Flutterは各Widgetの位置と座標を計算する `RenderObject` 中でWidgetを構築する

# Basic widgets

- Text
  - テキスト
- Row, Column
  - フレキシブルなレイアウト
  - Webで言うFlexbox
- Stack
  - Widgetを重ねる
  - Webで言う `position: absolute`
- Container
  - 四角形を作る
  - BoxDecprationで装飾できる
    - 背景色
    - 線の色
    - 影
  - margin, paddingを持てる
- IconButton
  - アイコン付きのボタン
- Expanded
  - 親要素の空いている領域を埋めるまで広がる
- Material
  - Material is a conceptual piece of paper on which the UI appears.
  - 試しに削除したらエラーが出た

# Using Material Components

- Material appは `MaterialApp` コンポーネントを設置することで開発できる
- ルーティングを実現する `Navigator` も含まれる
- `MaterialApp` を使うかは自由だが便利
- `AppBar` Widget
  - `leading`
    - 左上のメニュー（？）
    - 単一のWidget（IconButton）を指定している
  - `actions`
    - 右上のエリア（？）
    - 複数のWidget（現在IconButtonのみ）を指定している
- FloatingActionButton
  - フローティングボタン
- MaterialはFlutterに含まれる2つのデザインのうちの一つ
  - iOSのデザインは「Cupertino components」
- GestureDetector
  - ジェスチャをハンドリングできる

# Changing widgets in response to input

- `StatefulWidgets` を使うことで状態を持ったWidgetを作ることができる
- `StatefulWidget` と `State` が別々のオブジェクトになっている
  - ライフサイクルが違うため
  - `Widgets` は現在の状態を反映した一時的なオブジェクト
  - `State` は状態を保持するために `build()` の呼び出し間で維持される
- Widgetの階層構造で、上方向へのイベント通知はコールバックで行う
- 一方、現在の状態は下位の stateless widget へ適用されていく

```dart
class CounterDisplay extends StatelessWidget {
  CounterDisplay({this.count});

  final int count;

  @override
  Widget build(BuildContext context) {
    return Text('Count: $count');
  }
}

class CounterIncrementor extends StatelessWidget {
  CounterIncrementor({this.onPressed});

  final VoidCallback onPressed;

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: onPressed,
      child: Text('Increment'),
    );
  }
}

class Counter extends StatefulWidget {
  @override
  _CounterState createState() => _CounterState();
}

class _CounterState extends State<Counter> {
  int _counter = 0;

  void _increment() {
    setState(() {
      ++_counter;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Row(children: <Widget>[
      CounterIncrementor(onPressed: _increment),
      CounterDisplay(count: _counter),
    ]);
  }
}
```

# Bringing it all together

- 構造
  - `Product` クラス
  - `CartChangedCallback` 型
    - 
  - `ShoppingListItem` ウィジェット
    - StatelessWidget
    - `_getColor` メソッド
    - `_getTextStyle` メソッド
    - `_build` メソッド
  - `ShoppingList` ウィジェット
    - StatefulWidget
  - `ShoppingListState` クラス
    - State
  - `main` 関数
- Widgetのプロパティの変更のタイミングで任意の処理を実行したい場合は `didUpdateWidget()` を使う
  - `oldWidget` が渡される
- Stateの変更をFlutterに伝えるために、状態の変更は `setState()` の中で行う必要がある
- `createState()` が呼ばれた後、Flutterは新しいStateのオブジェクトをツリーに構築し、 `initState()` をコールする
  - `initState` はStateのサブクラスでオーバーライドできる
    - アニメーションの設定
    - OSイベントの購読
- Stateオブジェクトが不要になったとき、Flutterは `dispose()` を呼び出す
  - `dispose` もオーバーライドできる
    - タイマーの終了
    - 購読終了
    - 一般的に `super.dispose` も最後に配置する

# Keys

- 状態間の変更の計算を効率的に行うために使われる

# Global keys

- Widgetに関連づけられているStateを取得するために使うことができる
- アプリケーション全体で一意である必要がある
