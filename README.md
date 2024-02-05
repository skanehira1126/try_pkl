# pkl

## 概要
appleが開発した設定ファイル用の静的型付け言語
https://github.com/apple/pkl?tab=readme-ov-file

標準機能として4種類のパッケージがあるらしいが、これに限った話ではなさそう.
- [pkl-config-java](https://pkl-lang.org/main/current/java-binding/pkl-config-java.html)
- [pkl-config-kotlin](https://pkl-lang.org/main/current/kotlin-binding/pkl-config-kotlin.html)
- [pkl-swift](https://pkl-lang.org/swift/current/index.html)
- [pkl-go](https://pkl-lang.org/go/current/index.html)

## install
https://pkl-lang.org/main/current/pkl-cli/index.html#installation

```bash
$ curl -L -o pkl https://github.com/apple/pkl/releases/download/0.25.1/pkl-macos-amd64
$ chmod +x pkl
$ ./pkl --version
Pkl 0.25.1 (macOS 14.1, native)

$ sudo mv pkl /usr/local/bi
```

## サンプル
###  とりあえず使ってみる
```bash
# intro.pklを作成し実行 
# -> エラーがないことがわかる
$ pkl eval pkl_files/intro.pkl
name = "Pkl: Configure your Systems in New Ways"
attendants = 100
isIteractive = true
amountLearned = 13.37

# -fオプションで出力フォーマットを変更できる
$ pkl eval -f json pkl_files/intro.pkl
{
  "name": "Pkl: Configure your Systems in New Ways",
  "attendants": 100,
  "isIteractive": true,
  "amountLearned": 13.37
}
```

今のバージョン(0.25.1)確認できた範囲で出力できるフォーマットは以下の8つ
- json
- jsonnet
- pcf
- properties
- plist
- textproto
- xml
- yaml

### object, elements, entry
構造体やクラスのようなものを作成できる

#### object
```bash
# simpleObjectWithPoperties.pkl
# bird objectを作成
$ pkl eval pkl_files/simpleObjectWithProperties.pkl -f json
{
  "bird": {
    "name": "Common wood pigeon",
    "diet": "Seeds",
    "taxonomy": {
      "species": "Columba palumbus"
    }
  }
}
```

#### element
```bash
# simpleObjectsWithElements.pklを作成
# 二つのエレメントを作成 -> 配列になる
$ pkl eval pkl_files/simpleObjectsWithElements.pkl -f json
{
  "exampleObjectWithJustIntElements": [
    100,
    42
  ],
  "exampleObjectWithMixedElements": [
    "Bird Breeder Conference",
    223,
    [
      100,
      42
    ]
  ]
}
```


#### entry
いまいち理解できていない.
ただ、動的にkeyを生成したいときに利用できるみたい.

```bash
# simpleObjectsWithEntry.pkl
$ pkl eval pkl_files/simpleObjectsWithEntries.pkl
pigeonShelter {
  ["bird"] {
    name = "Common wood pigeon"
    diet = "Seeds"
    taxonomy {
      species = "Columba palumbus"
    }
  }
  ["address"] = "355 Bird St."
}
birdCount {
  [new {
    ["bird"] {
      name = "Common wood pigeon"
      diet = "Seeds"
      taxonomy {
        species = "Columba palumbus"
      }
    }
    ["address"] = "355 Bird St."
  }] = 42
}
```

pigeonShelterをkeyとするbirdCountのobjectを作成しているのでjsonやyamlで出力することができない.  
このような複雑なObjectは`mixObject.pkl`に記載されている

#### Collections 
配列や辞書などを扱う

```bash
# collections.pklを作成
# 配列や辞書をのように扱える
$ pkl eval pkl_files/collections.pkl
birds {
  "Pigeon"
  "Parrot"
  "Barn owl"
  "Falcon"
}
habitats {
  ["Pigeon"] = "Streets"
  ["Parrot"] = "Parks"
  ["Barn owl"] = "Forests"
  ["Falcon"] = "Mountains"
}

# jsonで出力
$ pkl eval pkl_files/collections.pkl -f json
{
  "birds": [
    "Pigeon",
    "Parrot",
    "Barn owl",
    "Falcon"
  ],
  "habitats": {
    "Pigeon": "Streets",
    "Parrot": "Parks",
    "Barn owl": "Forests",
    "Falcon": "Mountains"
  }
}

# indexedListing.pklを作成
# 配列をindexで参照できる
# 0始まり
$ pkl eval pkl_files/indexedListing.pkl
birds {
  "Pigeon"
  "Parrot"
  "Barn owl"
  "Falcon"
}
relatedToSnowOwl = "Barn owl"

# jsonにしてみる
$ pkl eval pkl_files/indexedListing.pkl -f json
{
  "birds": [
    "Pigeon",
    "Parrot",
    "Barn owl",
    "Falcon"
  ],
  "relatedToSnowOwl": "Barn owl"
}
```

