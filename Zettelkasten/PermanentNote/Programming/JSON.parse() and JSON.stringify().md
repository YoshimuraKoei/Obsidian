---
title: How To Use JSON.parse() and JSON.stringify()
AI: "false"
published: 2025-07-17
description: 
tags:
  - permanent-note
  - qiita
  - programming
---
Updated on Novemper 25, 2021
https://www.digitalocean.com/community/tutorials/js-json-parse-stringify
Qiitaでも書いてみた
→ https://qiita.com/noeten916523/items/251263477cc95829f2cc

---
##### 導入
JSON objectはすべてのモダンブラウザで利用可能である。中でもJSON形式を扱える便利なメソッド、`parse`と`stringify`が存在する。

##### JSON.parse()
JSON.parse()はJSONの文字列をJavaScriptのオブジェクトに変換するメソッドである。
```
let userStr = '{"name":"Sammy","email":"sammy@example.com","plan":"Pro"}';

let userObj = JSON.parse(userStr);

console.log(userObj);
```
実行後の結果は下記のようになる。
```
{name: 'Sammy', email: 'sammy@example.com', plan: 'Pro'}
```
※ JavaScriptと異なり末尾のコンマはJSONでは無効であり、エラーになる。
```
let userStr = '{"name":"Sammy","email":"sammy@example.com","plan":"Pro",}';


Uncaught SyntaxError: Expected double-quoted property name in JSON at position 57 (line 1 column 58) at JSON.parse (<anonymous>) at <anonymous>:1:20

```

JSONからJavaScriptのオブジェクトへパースするとき、プロパティのkeyとvalueに対して操作を加えることもできる。
```
let userStr = '{"name":"Sammy","email":"sammy@example.com","plan":"Pro"}';

let userObj = JSON.parse(userStr, (key, value) => {
  if (typeof value === 'string') {
    return value.toUpperCase();
  }
  return value;
});

console.log(userObj);
// {name: 'SAMMY', email: 'SAMMY@EXAMPLE.COM', plan: 'PRO'}
```

##### JSON.stringify()
JavaScriptオブジェクトを受け取り、それをJSON文字列に変換する。これはJSON.parse()とは逆の効果になる。

```
let userObj = {
  name: "Sammy",
  email: "sammy@example.com",
  plan: "Pro"
};

let userStr = JSON.stringify(userObj);

console.log(userStr);
// {"name":"Sammy","email":"sammy@example.com","plan":"Pro"}
```

JSON.stringify()は任意で第二引数、第三引数をとることができ、それぞれreplacerコールバック関数と、文字列の間に挿入するStringかNumberである。replacer関数はkeyとvalueを引数に取ることができる。

```
let userObj = {
  name: "Sammy",
  email: "sammy@example.com",
  plan: "Pro"
};

function replacer(key, value) {
  console.log(typeof value);
  if (key === 'email') {
    return undefined;
  }
  return value;
}

let userStrReplacer = JSON.stringify(userObj, replacer);

console.log(userStrReplacer);
```

```
let userObj = {
  name: "Sammy",
  email: "sammy@example.com",
  plan: "Pro"
};

let userStrSpace = JSON.stringify(user, null, '...');

console.log(userStrSpace);
// '{\naaa"name": "Sammy",\naaa"email": "sammy@example.com",\naaa"plan": "Pro"\n}'
```