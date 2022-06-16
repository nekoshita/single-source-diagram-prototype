# SingleSourceDiagram Prototype

# SingleSourceDiagram とは？
複雑すぎて図で表現できない問題、図の数が膨大になりがち問題を解決するために試作したツールです。
最低限の試作のため、全く利用者に優しくないです、ご了承ください🙏

## SingleSourceDiagramの概要
- Yamlで図の要素を全て定義する
  - 1Yamlファイルが1要素となる
- `config.yaml`でPlantUMLとして描画する条件を指定できる

## SingleSourceDiagramの紹介記事
- https://qiita.com/nekoshita_yuki/private/cce2eb7f14a859b5737b

# 実行方法
1. ファイルの用意(このリポジトリにはすでに用意してあります)
   1. `sample-service/`図の要素を定義するYamlファイルが含まれるディレクトリ
   2. `config.yaml` 描画する対象の要素の条件を定義したYamlファイル
2. 下記コマンドを実行（描画結果はこのリポジトリに含まれています）
```
docker image pull nekoshita/single-source-diagram-prototype:latest
dct run --rm -v $PWD:/usr/local/src/ nekoshita/single-source-diagram-prototype:latest
```

PlantUMLの描画はローカルで環境構築してください。
ある程度小さな図であればオンラインで描画も可能です。
https://www.plantuml.com/plantuml/uml/SyfFKj2rKt3CoKnELR1Io4ZDoSa70000

# ディレクトリ構成
- sample-service
  - ここに図の定義が全て含まれています
- config.yaml
  - 描画するPlantUMLの条件を指定します
- sample-service-puml
  - PlantUMLとして出力されたファイルです

# Yaml定義

## 図の要素を表現するYaml定義
IDは拡張子のないファイルパスになります。
実際のyamlファイルの例は、`sample-service/`以下を参考にしてください。
```yaml
content: 図の要素の中に表示するテキスト
tags:
- config.yamlでtag名を指定することが可能
refer_to: 参照する要素のID。参照先の要素がこの要素の子要素となる。
links:
- id: リンク先の要素のID・
  type: flow,get,postのいずれか。矢印の見た目が変化する。
  text: 矢印に付与する文字
```

## 描画対象を指定するYaml定義
```yaml
diagram_root_dir: 図の要素が含まれたディレクトリパス
out_root_dir: PlantUMLファイルの出力先ディレクトリ
diagrams_to_render:
- output_path: 出力先のパス。out_root_dirに対する相対パス
  conditions:
  - id: 対象の要素のID。tagかidのどちらかのみに値が必須。
    tag: 対象のtag名
    enable: true/false。条件を満たした要素を有効にするか、無効にするか
    child_depth: 対象の要素の子要素を対象とする深さ
    link_to_depth: id,tag,child_depthの条件を満たす要素からリンク先を対象とする深さ
    link_from_depth: id,tag,child_depthの条件を満たす要素からリンク元を対象とする深さ
```
