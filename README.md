# grouping-tool
Grouping Web tool For Eldenring Nightrein.

https://usagiga.github.io/grouping-tool/

It devides 4 members into 2 groups(3-1, 2-2).

This tool is positioned as Just PoC of [usagiga/discord-grouping-bot](https://github.com/usagiga/discord-grouping-bot).
Created by Gemini 3.5 Pro.
Hosted on GitHub Pages.

## Prompt

>  4人を最大3人のグループに分けるアプリを作ってほしいです

<details>

4人を「最大3人」のグループに分けるということは、以下の2つのパターンのどちらかになるということですね。

1. **3人と1人** に分ける
2. **2人と2人** に分ける

すぐに使える**「ブラウザで動く簡易アプリ（HTMLファイル）」**と、ロジックを確認するための**「Pythonコード」**の2種類を作成しました。

---

### 方法1：ブラウザですぐ使えるアプリ（推奨）

このコードをメモ帳などに貼り付け、`grouping.html` という名前で保存し、ブラウザ（ChromeやSafari）で開くだけで動きます。

**特徴：**

* 4人の名前を入力できます。
* ボタンを押すたびに、「2対2」または「3対1」にランダムで振り分けられます。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>グループ分けアプリ</title>
    <style>
        body { font-family: sans-serif; text-align: center; padding: 20px; background-color: #f0f4f8; }
        .container { max-width: 400px; margin: 0 auto; background: white; padding: 20px; border-radius: 12px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
        input { width: 80%; padding: 10px; margin: 5px 0; border: 1px solid #ddd; border-radius: 4px; }
        button { background-color: #007bff; color: white; border: none; padding: 12px 24px; border-radius: 6px; font-size: 16px; cursor: pointer; margin-top: 15px; width: 100%; }
        button:hover { background-color: #0056b3; }
        #result { margin-top: 20px; text-align: left; }
        .group-box { background: #e9ecef; padding: 10px; margin-bottom: 10px; border-radius: 6px; }
        h3 { margin-top: 0; color: #333; }
    </style>
</head>
<body>

<div class="container">
    <h2>👥 4人をグループ分け</h2>
    <p>名前を入力してください</p>
    <input type="text" id="p1" value="Aさん">
    <input type="text" id="p2" value="Bさん">
    <input type="text" id="p3" value="Cさん">
    <input type="text" id="p4" value="Dさん">

    <button onclick="makeGroups()">グループ分けする！</button>

    <div id="result"></div>
</div>

<script>
    function makeGroups() {
        // 入力された名前を取得
        const names = [
            document.getElementById('p1').value,
            document.getElementById('p2').value,
            document.getElementById('p3').value,
            document.getElementById('p4').value
        ];

        // シャッフル（ランダムに並び替え）
        for (let i = names.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [names[i], names[j]] = [names[j], names[i]];
        }

        // 分け方をランダムに決定（0なら2vs2、1なら3vs1）
        // ※完全に運任せにする場合
        const splitPattern = Math.random() < 0.5 ? 2 : 3; 

        const group1 = names.slice(0, splitPattern);
        const group2 = names.slice(splitPattern);

        // 結果を表示
        const resultDiv = document.getElementById('result');
        resultDiv.innerHTML = `
            <div class="group-box">
                <h3>グループ1 (${group1.length}人)</h3>
                ${group1.join(', ')}
            </div>
            <div class="group-box">
                <h3>グループ2 (${group2.length}人)</h3>
                ${group2.join(', ')}
            </div>
        `;
    }
</script>

</body>
</html>

```

---

### 方法2：Pythonコード（ロジックの確認用）

もしプログラミングの勉強中であったり、コンソールで実行したい場合はこちらのPythonコードをご利用ください。

```python
import random

def split_groups(members):
    # メンバーをシャッフルする
    random.shuffle(members)
    
    # パターンをランダムに決める
    # split_index 2 = [2人, 2人]
    # split_index 3 = [3人, 1人]
    split_index = random.choice([2, 3])
    
    group_a = members[:split_index]
    group_b = members[split_index:]
    
    return group_a, group_b

# 実行例
members = ["Aさん", "Bさん", "Cさん", "Dさん"]
g1, g2 = split_groups(members)

print(f"--- 結果（{len(g1)}対{len(g2)}）---")
print(f"グループ1: {', '.join(g1)}")
print(f"グループ2: {', '.join(g2)}")

```

### ポイント

* **「最大3人」の解釈:** 4人の場合、全員を1グループにすると4人になってしまい条件違反（最大3人）となるため、必ず2つのグループに分かれます。
* **ランダム性:** 上記のアプリは、「2対2」になるか「3対1」になるかも運任せにしていますが、もし「常に2対2がいい」などの希望があれば、コード内の数値を固定するだけで変更可能です。

---

**ユーザー様のためにできる次のステップ:**
もし、このアプリを**「スマホのホーム画面に追加してアプリのように使いたい」**場合や、**「毎回2対2に固定したい」**などのカスタマイズが必要であれば、その方法をお伝えしますか？

</details>

>  いいですね！これを、実行時に入力内容・実行結果をローカルストレージに保存して、読み込み時にローカルストレージの内容から復元するようにして。あと、実行結果をクリップボードにコピーするボタンも作って。 

<details>

ご要望の機能（ローカルストレージ保存、復元、クリップボードコピー）を追加しました。

このHTMLファイルを保存してブラウザで開けば、**「ページを閉じても前回の名前と結果が残っている」**状態で使えます。また、結果をLINEやSlackなどにすぐに貼り付けられます。

### 📱 機能強化版 グループ分けアプリ

以下のコードをすべてコピーして、`grouping_v2.html` などの名前で保存してください。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>グループ分けアプリ（保存機能付き）</title>
    <style>
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; text-align: center; padding: 20px; background-color: #f0f4f8; color: #333; }
        .container { max-width: 400px; margin: 0 auto; background: white; padding: 25px; border-radius: 16px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
        h2 { margin-top: 0; font-size: 1.5rem; }
        .input-group { margin-bottom: 10px; text-align: left; }
        label { font-size: 0.85rem; color: #666; margin-left: 5px; }
        input { width: 100%; padding: 12px; margin-top: 5px; border: 1px solid #ddd; border-radius: 8px; box-sizing: border-box; font-size: 16px; }
        
        /* ボタンのデザイン */
        .btn { border: none; padding: 14px; border-radius: 8px; font-size: 16px; font-weight: bold; cursor: pointer; width: 100%; margin-top: 10px; transition: opacity 0.2s; }
        .btn-primary { background-color: #007bff; color: white; }
        .btn-secondary { background-color: #28a745; color: white; display: none; /* 初期状態は非表示 */ }
        .btn:hover { opacity: 0.9; }
        .btn:active { transform: scale(0.98); }

        /* 結果表示エリア */
        #result { margin-top: 25px; text-align: left; }
        .group-box { background: #f8f9fa; padding: 15px; margin-bottom: 15px; border-radius: 10px; border-left: 5px solid #007bff; }
        .group-box:last-child { border-left-color: #17a2b8; }
        h3 { margin: 0 0 8px 0; font-size: 1.1rem; }
        p { margin: 0; font-size: 1rem; }
    </style>
</head>
<body>

<div class="container">
    <h2>👥 4人をグループ分け</h2>
    
    <div class="input-group">
        <label>メンバー1</label>
        <input type="text" id="p1" placeholder="名前を入力">
    </div>
    <div class="input-group">
        <label>メンバー2</label>
        <input type="text" id="p2" placeholder="名前を入力">
    </div>
    <div class="input-group">
        <label>メンバー3</label>
        <input type="text" id="p3" placeholder="名前を入力">
    </div>
    <div class="input-group">
        <label>メンバー4</label>
        <input type="text" id="p4" placeholder="名前を入力">
    </div>

    <button class="btn btn-primary" onclick="makeGroups()">🎲 グループ分け実行</button>
    
    <div id="result"></div>
    
    <button id="copyBtn" class="btn btn-secondary" onclick="copyResult()">📋 結果をコピー</button>
</div>

<script>
    // ページ読み込み時にローカルストレージから復元
    window.addEventListener('load', () => {
        if(localStorage.getItem('p1')) document.getElementById('p1').value = localStorage.getItem('p1');
        if(localStorage.getItem('p2')) document.getElementById('p2').value = localStorage.getItem('p2');
        if(localStorage.getItem('p3')) document.getElementById('p3').value = localStorage.getItem('p3');
        if(localStorage.getItem('p4')) document.getElementById('p4').value = localStorage.getItem('p4');

        // 前回の結果があれば復元して表示
        const lastResult = localStorage.getItem('lastResultHTML');
        if (lastResult) {
            document.getElementById('result').innerHTML = lastResult;
            document.getElementById('copyBtn').style.display = 'block';
        }
    });

    function makeGroups() {
        const inputs = [
            document.getElementById('p1'),
            document.getElementById('p2'),
            document.getElementById('p3'),
            document.getElementById('p4')
        ];

        // 入力値を保存（次回訪問時のため）
        inputs.forEach((input, index) => {
            localStorage.setItem('p' + (index + 1), input.value);
        });

        // 名前リストを作成（空欄ならデフォルト名を入れる）
        let names = inputs.map((input, index) => input.value.trim() || `メンバー${index + 1}`);

        // シャッフル
        for (let i = names.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [names[i], names[j]] = [names[j], names[i]];
        }

        // 分け方をランダム決定 (2vs2 または 3vs1)
        const splitPattern = Math.random() < 0.5 ? 2 : 3;
        const group1 = names.slice(0, splitPattern);
        const group2 = names.slice(splitPattern);

        // 結果のHTML生成
        const resultHTML = `
            <div class="group-box">
                <h3>🟦 グループ1 (${group1.length}人)</h3>
                <p>${group1.join(', ')}</p>
            </div>
            <div class="group-box">
                <h3>🟩 グループ2 (${group2.length}人)</h3>
                <p>${group2.join(', ')}</p>
            </div>
        `;

        // 表示と保存
        const resultDiv = document.getElementById('result');
        resultDiv.innerHTML = resultHTML;
        localStorage.setItem('lastResultHTML', resultHTML);

        // コピーボタンを表示
        document.getElementById('copyBtn').style.display = 'block';
    }

    function copyResult() {
        const resultDiv = document.getElementById('result');
        
        // クリップボード用にテキストを整形
        // innerTextを使うとHTMLタグが消えて見た目通りの改行が残ります
        let clipText = "【グループ分け結果】\n\n" + resultDiv.innerText;

        // クリップボードに書き込み
        navigator.clipboard.writeText(clipText).then(() => {
            const btn = document.getElementById('copyBtn');
            const originalText = btn.innerText;
            btn.innerText = "✅ コピーしました！";
            setTimeout(() => {
                btn.innerText = originalText;
            }, 2000);
        }).catch(err => {
            alert('コピーに失敗しました');
        });
    }
</script>

</body>
</html>

```

### 追加・変更した点の解説

1. **ローカルストレージへの保存 (`localStorage`):**
* **入力内容の保存:** 「実行」ボタンを押すと、入力された名前がブラウザに保存されます。ページをリロードしても名前が消えません。
* **結果の保存:** 計算されたグループ分けの結果（HTML）も保存されます。ブラウザを誤って閉じても、再度開けば「さっきの結果」が表示されます。


2. **クリップボードへコピー機能:**
* 実行後に緑色の**「📋 結果をコピー」**ボタンが出現します。
* これを押すと、以下のようなテキスト形式でクリップボードにコピーされます。LINEなどに貼り付けたときに読みやすくなるよう整形しています。
```text
【グループ分け結果】

🟦 グループ1 (2人)
Aさん, Bさん

🟩 グループ2 (2人)
Cさん, Dさん

```




3. **UI（見た目）の調整:**
* スマホで操作しやすいよう、ボタンや入力欄を少し大きくしました。
* コピーが成功したときにボタンの文字が「✅ コピーしました！」に一瞬変わるアニメーションを入れました。



---

**ユーザー様のためにできる次のステップ:**
これを使って実際に試してみて、もし**「過去の履歴を5件くらい残したい」**とか**「名前の横に点数を入れて、戦力が均等になるように分けたい」**といった要望が出てきたら、さらにロジックを組むことができますので教えてくださいね。

</details>

>  最高です！これのローカルストレージへの保存方法ですが、入力内容・出力結果をひとつの Object にまとめて JSON.stringify してから保存するように変えてください。

<details>

データの管理方法として、とても良い着眼点ですね！
`localStorage` のキーが散らからず、1つのキー（例えば `groupingAppData`）だけで管理できるので、データの整合性も保ちやすく、拡張もしやすくなります。

ご希望通り、**「入力されたメンバー名」と「結果のHTML」を1つのオブジェクトにまとめ、JSON形式で保存・復元する形**に書き換えました。

以下のコードを `grouping_v3.html` として保存してください。

### 💾 JSON保存対応版 グループ分けアプリ

```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>グループ分けアプリ（JSON保存版）</title>
    <style>
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; text-align: center; padding: 20px; background-color: #f0f4f8; color: #333; }
        .container { max-width: 400px; margin: 0 auto; background: white; padding: 25px; border-radius: 16px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
        h2 { margin-top: 0; font-size: 1.5rem; }
        .input-group { margin-bottom: 10px; text-align: left; }
        label { font-size: 0.85rem; color: #666; margin-left: 5px; }
        input { width: 100%; padding: 12px; margin-top: 5px; border: 1px solid #ddd; border-radius: 8px; box-sizing: border-box; font-size: 16px; }
        
        .btn { border: none; padding: 14px; border-radius: 8px; font-size: 16px; font-weight: bold; cursor: pointer; width: 100%; margin-top: 10px; transition: opacity 0.2s; }
        .btn-primary { background-color: #007bff; color: white; }
        .btn-secondary { background-color: #28a745; color: white; display: none; }
        .btn:hover { opacity: 0.9; }

        #result { margin-top: 25px; text-align: left; }
        .group-box { background: #f8f9fa; padding: 15px; margin-bottom: 15px; border-radius: 10px; border-left: 5px solid #007bff; }
        .group-box:last-child { border-left-color: #17a2b8; }
        h3 { margin: 0 0 8px 0; font-size: 1.1rem; }
        p { margin: 0; font-size: 1rem; }
    </style>
</head>
<body>

<div class="container">
    <h2>👥 4人をグループ分け</h2>
    
    <div class="input-group">
        <label>メンバー1</label>
        <input type="text" id="p1" placeholder="名前を入力">
    </div>
    <div class="input-group">
        <label>メンバー2</label>
        <input type="text" id="p2" placeholder="名前を入力">
    </div>
    <div class="input-group">
        <label>メンバー3</label>
        <input type="text" id="p3" placeholder="名前を入力">
    </div>
    <div class="input-group">
        <label>メンバー4</label>
        <input type="text" id="p4" placeholder="名前を入力">
    </div>

    <button class="btn btn-primary" onclick="makeGroups()">🎲 グループ分け実行</button>
    
    <div id="result"></div>
    
    <button id="copyBtn" class="btn btn-secondary" onclick="copyResult()">📋 結果をコピー</button>
</div>

<script>
    // 保存に使うキー名
    const STORAGE_KEY = 'grouping_app_data_v1';

    // ① ページ読み込み時：JSONを取得して復元
    window.addEventListener('load', () => {
        const jsonString = localStorage.getItem(STORAGE_KEY);
        
        if (jsonString) {
            try {
                // JSONをオブジェクトに戻す
                const data = JSON.parse(jsonString);

                // メンバー名の復元 (配列から各inputへ)
                if (data.members && Array.isArray(data.members)) {
                    document.getElementById('p1').value = data.members[0] || '';
                    document.getElementById('p2').value = data.members[1] || '';
                    document.getElementById('p3').value = data.members[2] || '';
                    document.getElementById('p4').value = data.members[3] || '';
                }

                // 結果HTMLの復元
                if (data.resultHTML) {
                    document.getElementById('result').innerHTML = data.resultHTML;
                    document.getElementById('copyBtn').style.display = 'block';
                }
            } catch (e) {
                console.error("保存データの読み込みに失敗しました", e);
            }
        }
    });

    function makeGroups() {
        // 入力値を取得
        const inputs = [
            document.getElementById('p1').value,
            document.getElementById('p2').value,
            document.getElementById('p3').value,
            document.getElementById('p4').value
        ];

        // 名前リストを作成（空欄ならデフォルト名）
        let names = inputs.map((val, index) => val.trim() || `メンバー${index + 1}`);

        // シャッフル
        for (let i = names.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [names[i], names[j]] = [names[j], names[i]];
        }

        // 分け方を決定
        const splitPattern = Math.random() < 0.5 ? 2 : 3;
        const group1 = names.slice(0, splitPattern);
        const group2 = names.slice(splitPattern);

        // 結果のHTML生成
        const resultHTML = `
            <div class="group-box">
                <h3>🟦 グループ1 (${group1.length}人)</h3>
                <p>${group1.join(', ')}</p>
            </div>
            <div class="group-box">
                <h3>🟩 グループ2 (${group2.length}人)</h3>
                <p>${group2.join(', ')}</p>
            </div>
        `;

        // 画面に表示
        const resultDiv = document.getElementById('result');
        resultDiv.innerHTML = resultHTML;
        document.getElementById('copyBtn').style.display = 'block';

        // ② 保存処理：オブジェクトにまとめてJSON化
        const dataToSave = {
            members: inputs,      // 入力された生の値を配列で保存
            resultHTML: resultHTML, // 生成された結果のHTML
            timestamp: new Date().toISOString() // おまけ：保存日時も入れておくと後で便利かも
        };

        localStorage.setItem(STORAGE_KEY, JSON.stringify(dataToSave));
    }

    function copyResult() {
        const resultDiv = document.getElementById('result');
        let clipText = "【グループ分け結果】\n\n" + resultDiv.innerText;

        navigator.clipboard.writeText(clipText).then(() => {
            const btn = document.getElementById('copyBtn');
            const originalText = btn.innerText;
            btn.innerText = "✅ コピーしました！";
            setTimeout(() => { btn.innerText = originalText; }, 2000);
        }).catch(err => {
            alert('コピーに失敗しました');
        });
    }
</script>

</body>
</html>

```

### 変更のポイント（技術的な解説）

1. **保存時 (`makeGroups`関数内):**
* 以前は `localStorage.setItem` を何度も呼んでいましたが、今回は `dataToSave` というオブジェクトを1つ作りました。
* ```javascript
  const dataToSave = {
      members: inputs,       // ["名前A", "名前B", ...]
      resultHTML: resultHTML // "<div>...</div>"
  };

```


* これを `JSON.stringify(dataToSave)` で文字列に変換して保存しています。


2. **読み込み時 (`window`の`load`イベント):**
* `localStorage.getItem` で取得できるのは文字列なので、`JSON.parse()` を使って元のオブジェクトに戻しています。
* `data.members` 配列を各 `<input>` に割り当て、`data.resultHTML` を結果エリアに挿入しています。



これでデータ構造がすっきりしましたね。将来的に「過去履歴を配列で持つ」といった改造をする際も、このJSON形式であればスムーズに移行できます。

**ユーザー様のためにできる次のステップ:**
もし、このアプリを他の人にも配りたい場合、このHTMLファイルをメールやチャットで送るだけで使えますが、**「QRコードにしてスマホで読み取れるようにしたい」**（Webサーバーに置くなど）といったニーズはありますか？

</details>
