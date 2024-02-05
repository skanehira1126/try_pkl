# Modules
外部のpklファイルを参照する機能がある.  
これについていくつかやり方がありそう

## Normal Module

```bash
# modules/pigeon.pkl
$ pkl eval pkl_files/modules/pigeon.pkl
name = "Common wood pigeon"
diet = "Seeds"
taxonomy {
  species = "Columba palumbus"
}

# parrot.pkl
$ pkl eval pkl_files/parrot.pkl
parrot {
  name = "Great green macaw"
  diet = "Berries"
  taxonomy {
    species = "Ara ambiguus"
  }
}

# parrot_amend.pkl
$ pkl eval pkl_files/parrot_amend.pkl
name = "Great green macaw"
diet = "Seeds"
taxonomy {
  species = "Columba palumbus"
}
```

## Templates
pythonのようにpklファイルもスクリプト自身を利用できるし、importできる.  
明示的にモジュールとして定義できる.

```pkl
module acmecicd

class Pipeline {
    name: String(nameRequiresBranchName)?

    // branchNameが設定されていない場合にエラー
    hidden nameRequiresBranchName = (_) ->
        if (branchName == null)
            thrhow("Pipelines that set ad 'name' must also set a 'branchName'.")
        else true

    branchName: String?
}

// ここから下がProperty
// timeoutは整数で3以上
timeout: Int(this >= 3)

pipelines: Listing<Pipeline>

// 出力に関する設定
output {
    renderer = new YamlRenderer {}
}
```

```bash
# cicd.pkl
$ pkl eval pkl_files/cicd.pkl
timeout: 3
pipelines:
- name: prb
  branchName: main

# yamlの出力設定を入れているので、-fを入れてもyamlとして出てくる
$ pkl eval pkl_files/cicd.pkl -f json
timeout: 3
pipelines:
- name: prb
  branchName: main
```
```
