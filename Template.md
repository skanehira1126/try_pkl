# Template

propertyに対して型を宣言できる

## Basic

```pkl: pklTutorialPart3.pkl
name: String = "Writing a Template"
part: Int = 3
hasExercises: Boolean = true
amountLearned: Float = 13.37
duration: Duration = 30.min
bandwidthRequirementPerSecond: DataSize = 50.mb
```

確認する

```bash
$ pkl eval pklTutorialPart3.pkl
name = "Writing a Template"
part = 3
hasExercises = true
amountLearned = 13.37
duration = 30.min
bandwidthRequirementPerSecond = 50.mb
```

時間の長さ（Duration）やデータの大きさ（DataSize）というあまり見ない型がある.  
k8sの設定ファイルなどでメモリの大きさとしてデータの大きさがあったり、argo-workflowでジョブの実行時間があるので、こういうところも設定ファイル特化という感じがする

## 型
クラスを利用して値のValidationができる

```pkl
// String型のkey nameしか受け付けることができない.
class Language {
    name: String
}

// OK
bestForConfig: Language = new {
    name = "Pkl"
}

// NG
bestForConfig: Language = new {
    name = 1
}
bestForConfig: Language = new {
    name = "Pkl"
    is_use = true
}
}
```


型を使って設定を使い回すことができる

```bash
# pklTutorialPart1をベースに2, 3を作成
$ pkl eval pklTutorialParts.pkl
pklTutorialPart1 {
  name = "Basic Configuration"
  part = 1
  hasExercises = true
  amountLearned = 13.37
  duration = 30.min
  bandwidthRequirementsPerSecond = 52.4288.mb
}
pklTutorialPart2 {
  name = "Filling out a Template"
  part = 2
  hasExercises = true
  amountLearned = 13.37
  duration = 30.min
  bandwidthRequirementsPerSecond = 52.4288.mb
}
pklTutorialPart3 {
  name = "Writing a Template"
  part = 3
  hasExercises = true
  amountLearned = 13.37
  duration = 30.min
  bandwidthRequirementsPerSecond = 52.4288.mb
}
```


