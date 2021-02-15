# Ephemeral vs app state

- Ephemeral state
  - UI stateやlocal stateとも呼ばれる
  - 例
    - current page in a PageView
    - current progress of a complex animation
    - current selected tab in a BottomNavigationBar
- Ephemeral stateを扱うために、状態管理ライブラリを用いる必要はなく、単に `StatefulWidget` を用いれば良い

```dart
class MyHomepage extends StatefulWidget {
  @override
  _MyHomepageState createState() => _MyHomepageState();
}

class _MyHomepageState extends State<MyHomepage> {
  int _index = 0;

  @override
  Widget build(BuildContext context) {
    return BottomNavigationBar(
      currentIndex: _index,
      onTap: (newIndex) {
        setState(() {
          _index = newIndex;
        });
      },
      // ... items ...
    );
  }
}
```

- `setState()` とフィールド（ `_index` ）は純粋なDartコードのみ
- アプリの他の場所は `_index` にアクセスする必要がない
- アプリが終了されたり、再起動したりされても、 `_index` をゼロに戻すことを考える必要はない

## App state

- not ephemeralなState
  - shared stateとも
  - アプリの多くの場所で使いたい状態、ユーザーセッション間で維持したい状態
- 例
  - User preferences
  - Login info
  - Notifications in a social networking app
  - The shopping cart in an e-commerce app
  - Read/unread state of articles in a news app

## There is no clear-cut rule

- 先の例のナビゲーションの選択状態は、アプリケーションの要件によってはapp stateに移しても良い

# Simple app state management

- `provider` パッケージを使う
  - 他の選択肢もある
  - まずは `provider` がおすすめ

# Accessing the state

```dart
@override
Widget build(BuildContext context) {
  return SomeWidget(
    // Construct the widget, passing it a reference to the method above.
    MyListItem(myTapCallback),
  );
}

void myTapCallback(Item item) {
  print('user tapped on $item');
}
```

- これだと、同じようなコールバックの実装をさまざまな箇所に用意する必要がある
- `provider` パッケージを導入して以下の基本概念を押さえればうまく対処できる
  - ChangeNotifier
  - ChangeNotifierProvider
  - Consumer

# ChangeNotifier

- `ChangeNotifier` はFlutter SDKで提供されているクラス
  - Rxで言う `Observable`
  - シンプルなアプリでは、一つの `ChangeNotifier` があれば十分
- `ChangeNotifierProvider` は `ChangeNotifier` のインスタンスを提供するWidget
  - `ChangeNotifierProvider` はそれにアクセスしたいWidgetの上位に配置する
  - 複数のクラスを扱いたい場合は `MultiProvider` を使う
- `Consumer` は `ChangeNotifierProvider` の中に入れるWidgetの最上位に配置する

```
return Consumer<CartModel>(
  builder: (context, cart, child) {
    return Text("Total price: ${cart.totalPrice}");
  },
);
```

- `builder` の第三引数の `child` は、下位にコストが大きいWidgetがある場合に利用する

```dart
return Consumer<CartModel>(
  builder: (context, cart, child) => Stack(
        children: [
          // Use SomeExpensiveWidget here, without rebuilding every time.
          child,
          Text("Total price: ${cart.totalPrice}"),
        ],
      ),
  // Build the expensive widget here.
  child: SomeExpensiveWidget(),
);
```

## Provider.of

- Stateの変更をキックするだけで状態の監視が必要ないWidgetでは以下のようにすることで効率化できる

```dart
Provider.of<CartModel>(context, listen: false).removeAll();
```

# サンプルアプリケーションのコードリーディング

- https://github1s.com/flutter/samples/blob/master/provider_shopper/lib/screens/catalog.dart
- ストアへのアクセスは以下のように行う
  - `context.select` ストリームを購読
  - `context.read` 現在の値を一件取得

```dart
Widget build(BuildContext context) {
    // The context.select() method will let you listen to changes to
    // a *part* of a model. You define a function that "selects" (i.e. returns)
    // the part you're interested in, and the provider package will not rebuild
    // this widget unless that particular part of the model changes.
    //
    // This can lead to significant performance improvements.
    var isInCart = context.select<CartModel, bool>(
      // Here, we are only interested whether [item] is inside the cart.
      (cart) => cart.items.contains(item),
    );
```
