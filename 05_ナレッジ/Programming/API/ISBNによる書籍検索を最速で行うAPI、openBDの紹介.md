
KW：ISBN、openBD、Google Books API、機械学習

## 背景

書籍のデータセットを受け取ったのはいいが、カラムの自由度が低かったので、ISBNでopenBD経由して関連情報を取得したいというモチベーション。

状況：
- 約200,000件のISBNで検索かけて、書籍の周辺情報を取得したい(主に和書)

## 要件

要件：
- 出版日、書籍名などの最低限の情報を持っていること
- レスポンスが早く、数時間以内で終わること
- 無料であること

[ISBN書籍検索APIについて調べる](https://zenn.dev/banboobloom/articles/2025012600001)
↑ の記事を参考に、

1. [楽天ブックス書籍検索API（Books Book Search API）](https://webservice.rakuten.co.jp/documentation/books-book-search)
2. [Google Books API](https://developers.google.com/books/?hl=ja)
3. [openBD](https://openbd.jp/)

これら3つのAPIを検討した。

①レスポンスが早い
②1リクエストでMAX100件取得できる
③APIキーを発行することがない
④使用法がシンプル

これらの面で、openBDを採用。

ちなみにGoogle Books APIも試した。

- デフォルトで 1,000リクエスト / 日 
	- 上限引き上げの審査落ち：`Thank you for contacting Google Books. At this time we are unable to approve requests for an increased quota for the Google Books API. We do not have an estimated time as to when we will be able to approve said requests. We apologize for any inconvenience and thank you for your understanding.`
- 1リクエストで1件しか取得できない

これらが不便だった。
Google Books APIはopenBDにないサムネイルURLなど、レスポンスの情報量が多いことは強みだが...

## openBD 使用方法

下記の関連記事を利用

- [openBD の API で ISBN をキーに書誌情報を取得する ( jQuery 使用 )](https://qiita.com/slangsoft/items/c62023d227a9a9f43f8c)
- [openBDを使用して本の情報を表示させてみる](https://qiita.com/sekkenn1102/items/ae66b3aad3245344f46a)

使い方は、**「所定のURLにクエリパラメータとしてISBNを含めてHTTPリクエストを投げる」**、これだけです。

``` shell
curl https://api.openbd.jp/v1/get?isbn=[ISBN]
```

「鬼滅第1巻：978-4088807232」
これを試しに投げてみます。

```
curl https://api.openbd.jp/v1/get?isbn=978-4088807232
```

レスポンスはこちら。
(ISBNのハイフンは気にしなくてよいみたい)
``` shell
[
  {
    "onix": {
      "CollateralDetail": {
        "TextContent": [
          {
            "TextType": "04",
            "ContentAudience": "00",
            "Text": "残酷"
          }
        ]
      },
      "RecordReference": "9784088807232",
      "NotificationType": "03",
      "ProductIdentifier": {
        "ProductIDType": "15",
        "IDValue": "9784088807232"
      },
      "DescriptiveDetail": {
        "TitleDetail": {
          "TitleType": "01",
          "TitleElement": {
            "TitleElementLevel": "01",
            "TitleText": {
              "collationkey": "キメツ ノ ヤイバ",
              "content": "鬼滅の刃 1"
            }
          }
        },
        "Contributor": [
          {
            "SequenceNumber": "1",
            "ContributorRole": [],
            "PersonName": {
              "content": "吾峠, 呼世晴",
              "collationkey": "ゴトウゲ, コヨハル"
            }
          }
        ],
        "Collection": {
          "CollectionType": "10",
          "TitleDetail": {
            "TitleType": "01",
            "TitleElement": [
              {
                "TitleElementLevel": "02",
                "PartNumber": "",
                "TitleText": {
                  "collationkey": "",
                  "content": "ジャンプコミックス"
                }
              }
            ]
          }
        }
      },
      "PublishingDetail": {
        "Imprint": {
          "ImprintName": "集英社"
        },
        "PublishingDate": [
          {
            "PublishingDateRole": "11",
            "Date": "201606"
          }
        ]
      },
      "ProductSupply": {
        "SupplyDetail": {
          "ProductAvailability": "99",
          "Price": [
            {
              "PriceType": "01",
              "CurrencyCode": "JPY",
              "PriceAmount": "400"
            }
          ]
        }
      }
    },
    "hanmoto": {
      "datemodified": "2016-06-01 16:02:24",
      "datecreated": "2016-04-26 16:02:17",
      "reviews": [
        {
          "post_user": "genkina",
          "reviewer": "",
          "source_id": 7,
          "kubun_id": 1,
          "source": "産經新聞",
          "choyukan": "朝刊",
          "han": "",
          "link": "",
          "appearance": "2019-12-29",
          "gou": ""
        }
      ]
    },
    "summary": {
      "isbn": "9784088807232",
      "title": "鬼滅の刃 1",
      "volume": "",
      "series": "ジャンプコミックス",
      "publisher": "集英社",
      "pubdate": "201606",
      "cover": "",
      "author": "吾峠,呼世晴"
    }
  }
]
```

複数取得したい場合、コンマ区切りで投げる。

```
curl https://api.openbd.jp/v1/get?isbn=978-4088-807-232,978-4088815169
```

## 実際に使ったコード

使い慣れているJavaScript (Pythonじゃなくてすみません) で実装。
面倒だったのでGPT-5に書かせています。リファクタリングしてないです。

機能：
1. 元データのCSVファイルからISBNを取得し、100件ごとHTTPリクエスト
	- 実行ログをログデータに出力しておく
2. 5,000件ごとにJSONファイルとして保存
	- 一旦すべてのデータを取得して避難させたかった
3. すべて検索し終わるまで1, 2を繰り返す

``` JavaScript
import fs from "fs";
import fetch from "node-fetch";
import csv from "csv-parser";

// === 設定 ===

const INPUT_CSV = "./data/unique_isbn_list.";
const OUTPUT_JSON = "./data/openbd_responses.json"; // JSON出力
const LOG_FILE = "./data/progress_openbd.log"; // ログファイル
const BATCH_SIZE = 100; // 1リクエスト最大100件
const SAVE_INTERVAL = 5000; // 5,000件ごとに中間保存
const RETRY_LIMIT = 3; // リトライ回数

// === ログ追記 ===

function writeLog(message) {
  const timestamp = new Date().toISOString();
  const line = `[${timestamp}] ${message}\n`;
  fs.appendFileSync(LOG_FILE, line, "utf8");
}

// === リトライ付きfetch ===
async function fetchWithRetry(url, retries = RETRY_LIMIT) {
  for (let i = 0; i < retries; i++) {
    try {
      const res = await fetch(url);
      return await res.json();
    } catch (e) {
      if (i === retries - 1) throw e;
      writeLog(`⚠️ Retry ${i + 1}/${retries}: ${e.message}`);
      await new Promise((r) => setTimeout(r, 500 * (i + 1))); // バックオフ
    }
  }
}

async function main() {
  const results = [];
  const isbns = [];

  // ログ初期化
  fs.writeFileSync(LOG_FILE, "=== OpenBD API Fetch Log ===\n", "utf8");
  console.log("📖 CSV読み込み中...");
  writeLog("CSV読み込み開始");

  // === CSV読み込み ===
  await new Promise((resolve) => {
    fs.createReadStream(INPUT_CSV)
      .pipe(csv({ mapHeaders: ({ header }) => header.replace(/^\uFEFF/, "") }))
      .on("data", (row) => {
        const isbn = row.ISBN?.replace(/-/g, "").trim();
        if (isbn) isbns.push(isbn);
      })
      .on("end", () => resolve());
  });

  

  console.log(`✅ ISBN ${isbns.length} 件 読み込み完了`);
  writeLog(`ISBN ${isbns.length} 件 読み込み完了`);
  console.log("🔎 OpenBD API に問い合わせ開始...");
  writeLog("問い合わせ開始");

  // === バッチ処理 ===
  for (let i = 0; i < isbns.length; i += BATCH_SIZE) {
    const chunk = isbns.slice(i, i + BATCH_SIZE);
    const url = `https://api.openbd.jp/v1/get?isbn=${chunk.join(",")}`;
    
    try {
      const json = await fetchWithRetry(url);
      
      for (let j = 0; j < chunk.length; j++) {
        const isbn = chunk[j];
        const data = json?.[j];

        if (data) {
          const title = data.summary?.title || "(タイトルなし)";
          const author = data.summary?.author || "(著者不明)";
          const publisher = data.summary?.publisher || "(出版社不明)";
          const pubdate = data.summary?.pubdate || "";
          const logMsg = `📗 ${i + j + 1}/${isbns.length} title: ${title}`;

          console.log(logMsg);
          writeLog(logMsg);
          results.push({
            isbn,
            title,
            author,
            publisher,
            pubdate,
            response: data,
          });
        } else {
          const warnMsg = `⚠️ ISBN ${isbn}: データなし`;
          
          console.warn(warnMsg);
          writeLog(warnMsg);
          results.push({ isbn, response: null });
        }
      }
    } catch (e) {
      const errMsg = `❌ Error on batch starting ${i}: ${e.message}`;

      console.error(errMsg);
      writeLog(errMsg);

      chunk.forEach((isbn) =>
        results.push({ isbn, response: { error: e.message } })
      );
    }

  

    // === 定期的に途中保存 ===
    if (
      (i + BATCH_SIZE) % SAVE_INTERVAL === 0 ||
      i + BATCH_SIZE >= isbns.length
    ) {
      fs.writeFileSync(OUTPUT_JSON, JSON.stringify(results, null, 2), "utf8");
      const saveMsg = `💾 ${i + BATCH_SIZE}/${isbns.length} 件まで保存`;

      console.log(saveMsg);
      writeLog(saveMsg);
    }
  }

  console.log("🎉 完了! 出力:", OUTPUT_JSON);
  writeLog(`完了: 出力 ${OUTPUT_JSON}`);
}

main();
```


