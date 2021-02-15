# 背景

- APIでやりとりする際はJSONに変換・復元する必要がある

```dart
class User {
  final String name;
  final String email;

  User(this.name, this.email);

  User.fromJson(Map<String, dynamic> json)
      : name = json['name'],
        email = json['email'];

  Map<String, dynamic> toJson() =>
    {
      'name': name,
      'email': email,
    };
}
```

- モデルクラス自身に `fromJSON` を実装するパターン

# Serializing JSON using code generation libraries

- APIが複雑化するとそうはいかなくなる
  - さまざまなモデルに渡ってデータが含まれていて、ネストされている、等
- `json_serializable` パッケージを使う

```dart
/// Tell json_serializable to use "defaultValue" if the JSON doesn't
/// contain this key or if the value is `null`.
@JsonKey(defaultValue: false)
final bool isAdult;

/// When `true` tell json_serializable that JSON must contain the key, 
/// If the key doesn't exist, an exception is thrown.
@JsonKey(required: true)
final String id;

/// When `true` tell json_serializable that generated code should 
/// ignore this field completely. 
@JsonKey(ignore: true)
final String verificationCode;
```

- 呼び出し方
  - `String json = jsonEncode(user);`
- ネストされたフィールドは `@JsonSerializable(explicitToJson: true)` を使ってパース
