---
title: "Zennの記事を管理しているGitHubリポジトリのライセンスをみんなはどうしているのか調べてみた"
emoji: "⚖️"
type: "idea"
topics: ["zenn"]
published: true
---

## はじめに

Zennの記事をGitHubで管理してて、そのリポジトリがパブリックな場合にライセンスはどうするのがスタンダードなのだろうかと思ったので調べてみました。

## 結果

"zenn contents" で検索して出てくるパブリックなリポジトリ376件中、ライセンスを設定しているリポジトリが21件でした。以下はその内訳です。

- MIT License (10件、内1件はzennの記事のリポジトリのテンプレートリポジトリ)
- Creative Commons Attribution Share Alike 4.0 International (4件)
- Creative Commons Zero v1.0 Universal (3件)
- Apache License 2.0 (1件)
- The Unlicense (1件)
- Other (2件)
  - GNU Free Documentation License
  - Attribution-NonCommercial 4.0 International

ドキュメント的なものでも自由に使えるということを示したい場合はMIT Licenseにしておくのが無難ということでしょうか。LISENCEファイルを置いていない方も多いので、それがスタンダードとも言えるのかもしれません。

## 調べ方

[GitHub APIのエクスプローラー](https://docs.github.com/ja/graphql/overview/explorer) で以下のクエリを実行して `licenseInfo` の値を見て調査しました。

```graphql
{
  search(
    first: 100
    after: "CURSOR"
    query: "zenn contents"
    type: REPOSITORY
  ) {
    edges {
      cursor
      node {
        ... on Repository {
          id
          owner {
            login
          }
          name
          licenseInfo {
            name
          }
        }
      }
    }
  }
}
```
