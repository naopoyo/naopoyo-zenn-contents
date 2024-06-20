---
title: VSCodeのFront Matter CMSでZennの記事を管理しよう
emoji: 📚
published: false
date: 2024-06-20
topics:
  - vscode
  - zenn
type: idea
---

## はじめに

![ダッシュボード](/images/front-matter-cms-for-zenn/image01.jpg)

ZennとGitHubを連携してVSCodeで記事を書いてる人向けの内容です。「Front Matter CMS」という拡張機能を使うことで、VSCode内で上記の画像のような記事の管理ができるようになります。

:::message
2024/06/20時点で日本語対応はしていませんが、難しい操作は無いのでドキュメントを翻訳しながら読めばなんとかなります。
:::

## 便利なところ

- **タイトルの一覧表示ができる**  
  Zennの記事をGitHubで管理する場合はファイル名をslug（スラッグ）にしなければいけないため、タイトルから編集する記事を探すというのが大変です。これができると助かります。
- **フィルタリングと並べ替え**  
  前述の一覧表示は「トピック」「公開/下書き」でフィルタリングしたり、「日付」で並べ替えなどもできます。
- **Frontmatter部分をGUIで編集できる**  
  下記の画像のようなGUIの画面で編集できます。トピックの追加削除や「公開/下書き」の切り替えの時にタイピングしなくて良くなります。

![GUIでFrontmatterの編集](/images/front-matter-cms-for-zenn/image02.jpg)

## VSCode拡張機能のインストール

```bash
code --install-extension eliostruyf.vscode-front-matter
```

上記のコマンド、またはVSCodeから `eliostruyf.vscode-front-matter` で検索してインストールできます。

## Front Matter CMSの初期設定

![初期設定](/images/front-matter-cms-for-zenn/image03.jpg)

拡張機能をインストールするとVSCodeのプライマリサイドバーにアイコンが追加されます。選択して「Initialize Project」ボタンをクリックするとタブが開くので手順通りに進めていきます。

:::message
この初期設定は `frontmatter.json` という設定ファイルを作成して、項目を埋めていくということを行なっているだけなので、読み飛ばして、この記事で紹介している `frontmatter.json` を手動で作成してしまっても良いです。
:::

### 初期設定手順1. INITIALIZE PROJECT

クリックすると以下のファイルが作成されます。

- `.frontmatter/database/taxonomyDb.json`
- `frontmatter.json`  
  Front Matter CMSの設定ファイルです。

### 初期設定手順2. FRAMEWORK PRESET

`other` を選択します。

### 初期設定手順3. REGISTER CONTENT FOLDER(S)

1. プライマリサイドバーからエクスプローラを開き、`articles` ディレクトリを右クリックしてコンテクストメニューを開きます。
2. 「Front Matter」→「Register folder」を選択します。

### 初期設定手順4. IMPORT ALL TAGS AND CATEGORIES (OPTIONAL)

ここで設定する必要はないのでスキップします。

### 初期設定手順5. SHOW THE DASHBOARD

「SHOW THE DASHBOARD」をクリックしてダッシュボードを開くと完了です。

## Zennの記事を管理するためのおすすめ設定

Zennの記事を管理する場合のおすすめ設定です。このままコピペして使えば簡単に始められます。各項目の解説も記載しておきます。

:::details おすすめの frontmatter.json

```json:frontmatter.json
{
  "$schema": "https://frontmatter.codes/frontmatter.schema.json",
  "frontMatter.experimental": true,
  "frontMatter.extensibility.scripts": ["[[workspace]]/.frontmatter/ui/external.js"],
  "frontMatter.taxonomy.fieldGroups": [
    {
      "id": "GeneralFields",
      "fields": [
        {
          "title": "Title",
          "name": "title",
          "type": "string"
        },
        {
          "title": "Publishing date",
          "name": "date",
          "type": "datetime",
          "default": "{{now}}",
          "isPublishDate": true,
          "dateFormat": "yyyy-MM-DD"
        },
        {
          "title": "Emoji",
          "name": "emoji",
          "type": "string",
          "single": true,
          "encodeEmoji": false
        },
        {
          "title": "Published",
          "name": "published",
          "type": "draft",
          "default": false
        },
        {
          "title": "Topics",
          "name": "topics",
          "type": "tags"
        }
      ]
    }
  ],
  "frontMatter.taxonomy.contentTypes": [
    {
      "name": "tech",
      "filePrefix": null,
      "pageBundle": false,
      "previewPath": null,
      "fields": [
        {
          "name": "fieldCollection",
          "type": "fieldCollection",
          "fieldGroup": "GeneralFields"
        }
      ]
    },
    {
      "name": "idea",
      "filePrefix": null,
      "pageBundle": false,
      "previewPath": null,
      "fields": [
        {
          "name": "fieldCollection",
          "type": "fieldCollection",
          "fieldGroup": "GeneralFields"
        }
      ]
    }
  ],
  "frontMatter.framework.id": "other",
  "frontMatter.content.pageFolders": [
    {
      "title": "articles",
      "path": "[[workspace]]/articles"
    }
  ],
  "frontMatter.dashboard.openOnStart": true,
  "frontMatter.content.draftField": {
    "name": "published",
    "type": "boolean",
    "invert": true
  }
}
```

:::

| 項目                                | 解説                                                                                                                                                                                 |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `frontMatter.experimental`          | 実験的な機能を有効化する場合は `true` にします。カード形式の一覧で絵文字を表示するために有効化しています。                                                                           |
| `frontMatter.extensibility.scripts` | カード形式の一覧で絵文字を表示するためのスクリプトへのパスです。                                                                                                                     |
| `frontMatter.taxonomy.fieldGroups`  | Frontmatter部分の各項目に関する設定です。`type` という項目が特殊な動きをするためこのような設定にしています。[関連Issue](https://github.com/estruyf/vscode-front-matter/issues/467)。 |
| `frontMatter.taxonomy.contentTypes` | 同上                                                                                                                                                                                 |
| `frontMatter.framework.id`          | Front Matter CMSでは様々なフレームワーク上のMarkdownファイルも管理できます。今回は単純な構成なので `other` にしています。                                                            |
| `frontMatter.content.pageFolders`   | Markdownファイルを置くディレクトリの設定です。                                                                                                                                       |
| `frontMatter.dashboard.openOnStart` | VSCodeを開いた時にFront Matter CMSのダッシュボードを自動で開く場合は `true` にします。                                                                                               |
| `frontMatter.content.draftField`    | 公開/下書きの項目のための設定です。デフォルトでは `draft:true` で下書きになるので、`published:true` で公開になるようにするための設定です。                                           |

## カード表示で絵文字を表示

記事のはじめにある画像のようにカード表示した記事の一覧に絵文字を表示するためにはカスタマイズが必要です。

`.frontmatter/ui/external.js` に以下のようにファイルを作成します。

```js:.frontmatter/ui/external.js
import { registerCardImage } from 'https://cdn.jsdelivr.net/npm/@frontmatter/extensibility/+esm'

registerCardImage(async (_filePath, metadata) => {
  const image = metadata.fmPreviewImage ? metadata.fmPreviewImage : null
  if (image) {
    return `
      <div class="h-full flex items-center justify-center bg-[var(--vscode-sideBar-background)] group-hover:bg-[var(--vscode-list-hoverBackground)]">
        <img src=${image} />
      </div>
    `
  } else if (metadata.emoji) {
    return `
      <div class="h-full w-full flex items-center justify-center">
        <div style="font-size: 64px;">
          ${metadata.emoji}
        </div>
      </div>
    `
  }

  return null
})
```

`.frontmatter.json` に以下の設定を追記します。おすすめ設定にはすでに含まれています。

```json:.frontmatter.json
{
  "frontMatter.experimental": true,
  "frontMatter.extensibility.scripts": ["[[workspace]]/.frontmatter/ui/external.js"]
}
```

## 以上

以上です。「Front Matter CMS」はZennの記事管理だけでなく、ローカルのMarkdownメモを管理するのにも便利です。ぜひ使ってみてください。

## リンク

### 公式サイト

https://frontmatter.codes/

### Visual Studio Marketplace

https://marketplace.visualstudio.com/items?itemName=eliostruyf.vscode-front-matter
