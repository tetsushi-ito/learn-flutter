- Flutterではほぼ全てがWidget
- 見えないWidgetもある
  - Row, Col etc
- `debugPaintSizeEnabled` をセットすると見えないレイアウトも可視化できる
- `Container` を使うことでpadding, margin, borderを設定できる

# Lay out a widget

- [Layout widgets](https://flutter.dev/docs/development/ui/widgets/layout)にあるレイアウトを選ぶ
  - 一覧で視覚的にWidgetを並べてくれている
- `Text` や `Image` や `Icon` のような見えるWidgetを配置する
- レイアウトWidgetに作ったWidgetを配置する

```dart
Center(
  child: Text('Hello World'),
),
```

- レイアウトWidgetをページに配置する
  - Material appでは `Scaffold` Widgetを使うことができる
    - ヘッダー・背景色・サイドメニュー等のAPIを備える
- UIを構築する際は、widgets libraryかMaterial libraryから選ぶ
  - 既存のWidgetをカスタマイズすることも、新しく定義することもできる

# Lay out multiple widget vertically and horizontally

- Row, Colの組み合わせでだいたい表現できる
  - この辺はWebのFlexboxの考え方で良さそう

# Aligning widgets

- `Image.asset` を使って画像を配置する場合、 `pubspec.yaml` に記述する必要がある
  - `Image.network` を使って画像をインターネットから取得する場合は不要
- 配置方法は `mainAxisAlignment` / `crossAxisAlignment` プロパティで制御できる
  - `MainAxisAlignment.spaceEvenly`
- 画面にWidgetが収まらない場合の伸縮方法も指定可能
  - `Expand` Widget
  - その `flex` プロパティ

# Common layout widgets

- 以下で探す
  - https://flutter.dev/docs/development/ui/widgets
  - https://api.flutter.dev/index.html
- widgets libraryのWidgetはいつでも使えるが、Material Components libraryはMaterial appのみで利用可能

# Standard widgets

- Container
- GridView
- ListView
- Stack

# Material widgets

- Card
- ListTile

# Container

```dart
Widget _buildImageColumn() => Container(
      decoration: BoxDecoration(
        color: Colors.black26,
      ),
      child: Column(
        children: [
          _buildImageRow(1),
          _buildImageRow(3),
        ],
      ),
    );
```

- 角丸も可能

```dart
Container(
  decoration: BoxDecoration(
    border: Border.all(width: 10, color: Colors.black38),
    borderRadius: const BorderRadius.all(const Radius.circular(8)),
  ),
  margin: const EdgeInsets.all(4),
  child: Image.asset('images/pic$imageIndex.jpg'),
),
```

# GridView

- グリッドの作成

# ListView

- リストの作成

# Stack

- HTML/CSSで言う `position: absolute`

# Card

- Material card

# ListTile

- Rowより設定が少なくて使いやすい

# Constraints

- https://flutter.dev/docs/development/ui/layout/constraints

# Videos

- [How to Create Stateless Widgets - Flutter Widgets 101 Ep. 1](https://www.youtube.com/watch?v=wE7khGHVkYY)
