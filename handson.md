---
id: dist
{{if .Meta.Status}}status: {{.Meta.Status}}{{end}}
{{if .Meta.Summary}}summary: {{.Meta.Summary}}{{end}}
{{if .Meta.Author}}author: {{.Meta.Author}}{{end}}
{{if .Meta.Categories}}categories: {{commaSep .Meta.Categories}}{{end}}
{{if .Meta.Tags}}tags: {{commaSep .Meta.Tags}}{{end}}
{{if .Meta.Feedback}}feedback link: {{.Meta.Feedback}}{{end}}
{{if .Meta.GA}}analytics account: {{.Meta.GA}}{{end}}

---

# LINE Clova + enebular連携ハンズオン
## Clovaプロジェクトを作ろう！
### 1-1. スキルチャネルの作成
下記にアクセスしてログインしてください。
[https://clova-developers.line.biz/](https://clova-developers.line.biz/#/)

［スキルを開発する］をクリックします。

![s300](images/s300.png)

［LINE Developersでスキルチャネルを新規作成］ボタンをクリックします。

![s301](images/s301.png)

プロバイダーがまだ無い方は作成お願いします。
新規チャネルを作成します。

チャネルは`handson-enebular`としました。［チャネルを作成してClova Developer Centerに移動］をクリックします。

![s302](images/s302.png)

2つのチェックを入れてから、［スキル開発を始める］ボタンをクリックします。

![s303](images/s303.png)

### 1-2. スキルの設定
Extension IDとスキル名を入力します。

| 項目       |       値 |
|:-----------------|:------------------|
|Extension ID|com.あなたの名前.handsonenebular <br/>※例 com.gaomar.handsonenebular|
|スキル名|コネクトスキル|

![s304](images/s304.png)

呼び出し名（メイン）を設定します。

| 項目       |       値 |
|:-----------------|:------------------|
|呼び出し名（メイン）|コネクトスキル|

![s305](images/s305.png)

最後に［作成］ボタンをクリックします。

![s306](images/s306.png)

### 1-3. 対話モデルを設定する
Clovaと対話するためのモデルを作成します。
左側メニューの対話モデルをクリックし、［対話モデルを編集する］をクリックします。

![s400](images/s400.png)

ビルトインスロットタイプの［＋］をクリックします。

![s401](images/s401.png)

「数値と単位を取得」部分を展開して`CLOVA.NUMBER`を有効にします。

![s402](images/s402.png)

カスタムインテントの右側にある［＋］をクリックします。インテント名は「MainIntent」と入力し、［作成］ボタンをクリックします。

![s403](images/s403.png)

スロットリスト部分に「num」と入力し、［＋］をクリックします。

![s404](images/s404.png)

スロットタイプのプルダウンメニューから`CLOVA.NUMBER`を選択します。

![s405](images/s405.png)

サンプル発話リスト部分に「num」を入力し、［＋］をクリックします。

![s406](images/s406.png)

`num`部分をマウスで選択して、出てきたポップアップメニューでnumスロットを選択します。最後に必ず［保存］ボタンをクリックします。

![s407](images/s407.png)

ビルドを行います。約3分ほどビルドに時間がかかります。

![s408](images/s408.png)

## enebularでフローを作成しよう！
### 2-1. enebularプロジェクトを作成する
いよいよenebularを使ってClovaと繋いでいきます。
enebularのページにログインしてください。

[https://enebular.com/sign-in](https://enebular.com/sign-in)

ログインしたら［Create Project］ボタンをクリックします。

![s500](images/s500.png)

プロジェクト名を入力して、［Submit］ボタンをクリックします。

| 項目       |       値 |
|:-----------------|:------------------|
|プロジェクト名|handson-enebular|

![s501](images/s501.png)

左側メニューの「Flows」をクリックし、右下の［＋］ボタンをクリックします。

![s502](images/s502.png)

Flow名を入力して、カテゴリは`other`を選択し、［Continue］ボタンをクリックします。

![s503](images/s503.png)

［Edit］ボタンをクリックして、Flow画面を表示します。

![s504](images/s504.png)

### 2-2. Flowを作成する
Flow画面を表示して、Clovaに発話させるフローを作成します。
左側メニューの「入力」カテゴリにある`http`ノードをエディタにドラッグアンドドロップします。

ノードをクリックして、メソッドは`POST`を選択し、URLに`/clova`と入力します。

![s505](images/s505.png)

機能カテゴリにある`switch`ノードをドラッグアンドドロップします。
ノードをクリックして、各項目を埋めていきます。

| 項目       |       値 |
|:-----------------|:------------------|
|名前|request.type 判定|
|③プロパティ|payload.request.type|
|④ == |LaunchRequest|

![s506](images/s506.png)

`http`ノードと`switch`ノードを繋ぎます。

![s507](images/s507.png)

機能カテゴリにある`functions`ノードをドラッグアンドドロップします。ノードをクリックして、コード部分に書きコードを記述します。

| 項目       |       値 |
|:-----------------|:------------------|
|名前|LaunchRequestセリフ|

```javascript
msg.payload =
{
    "version": "1.0",
    "sessionAttributes": {},
    "response": {
        "outputSpeech": {
            "type": "SimpleSpeech",
            "values": {
                "type": "PlainText",
                "lang": "ja",
                "value": "エネブラーからこんにちは！"
            }
        },
        "card": {},
        "directives": [],
        "shouldEndSession": false
    }
}
return msg;
```

![s508](images/s508.png)

`switch`ノードを`function`ノードを繋ぎます。

![s509](images/s509.png)

出力カテゴリにある`http response`ノードをドラッグアンドドロップします。
このノードは特に設定することはありません。

![s510](images/s510.png)

`function`ノードと`http response`ノードを繋ぎます。

![s511](images/s511.png)

### 2-3. Clovaと連携する
画面右上の［デプロイ］ボタンをクリックして、デプロイを行います。

![s512](images/s512.png)

デプロイボタンの左にiボタンがあるのでマウスオーバーします。すると、ポップアップで表示されるので、アクセスURLをメモしておきます。

![s513](images/s513.png)

Clova Developer Centerページを開きます。左側メニューの開発設定にある［サーバー設定］をクリックします。
サーバーURLに先程メモしたURLを貼り付けて、その末尾に`/clova`を追加します。

![s514](images/s514.png)

### 2-4. シミュレーターで確認する
Clova Developer Centerページのテストをクリックします。
シナリオテストに切り替えて、［○○を起動して］ボタンをクリックします。すると、enebularで設定した値が返ってきます。

![s515](images/s515.png)

## BMI測定に対応しよう！
### 3-1. セッションの受け渡し
身長と体重を答えさせて、結果を発話する流れを作っていきます。
セッションを使って、身長データを一時的に保持して、次に来る体重の数値を使って計算してBMIを求めていきます。

まず、LaunchRequestの発話を「エネブラーからこんにちは！」から「BMIを測定するよ！身長を教えてね！」に変更します。

```コード
msg.payload =
{
    "version": "1.0",
    "sessionAttributes": {},
    "response": {
        "outputSpeech": {
            "type": "SimpleSpeech",
            "values": {
                "type": "PlainText",
                "lang": "ja",
                "value": "BMIを測定するよ！身長を教えてね！"
            }
        },
        "card": {},
        "directives": [],
        "shouldEndSession": false
    }
}
return msg;
```
![s600](images/s600.png)

request.type判定をクリックして、［+追加］ボタンを2回クリックします。
プロパティ値に`IntentRequest`と`SessionEndedRequest`を追加します。
順番も気をつけてください。

| 項目       |       値 |
|:-----------------|:------------------|
|→ 2|IntentRequest|
|→ 3|SessionEndedRequest|

![s601](images/s601.png)

機能カテゴリにある`switch`ノードをドラッグアンドドロップします。
ノードをクリックし、セッション値のnullチェックを行います。

payloadのsessionAttributesにセッション情報が格納されています。初回の場合は、セッション情報が無いので、null値だったらslotのheightから値を取得しています。

| 項目       |       値 |
|:-----------------|:------------------|
|名前|セッション情報取得|
|プロパティ|payload.session.sessionAttributes.height|
|→ 1| is not null  ※プルダウンメニューから選択|
|→ 2| is null ※プルダウンメニューから選択|

![s602](images/s602.png)

ノードを繋ぎます

![s603](images/s603.png)

機能カテゴリにある`change`ノードをドラッグアンドドロップします。
このノードは指定した値を別の変数に格納することができます。

| 項目       |       値 |
|:-----------------|:------------------|
|名前|セッションから身長取得|
|③値の代入|height|
|④対象の値プルダウンメニュー|msg.|
|⑤対象の値|payload.session.sessionAttributes.height|

![s604](images/s604.png)

ノードを繋ぎます。

![s605](images/s605.png)

機能カテゴリにある`change`ノードをドラッグアンドドロップし、ノードを繋ぎます。
ノードをクリックして、スロットから対象の値を取得して変数に代入します。

| 項目       |       値 |
|:-----------------|:------------------|
|名前|体重取得|
|④値の代入|weight|
|⑤対象の値プルダウンメニュー|msg.|
|⑥対象の値|payload.request.intent.slots.num.value|

![s606](images/s606.png)

機能カテゴリにある`functions`ノードをドラッグアンドドロップし、ノードを繋ぎます。
ノードをクリックして、BMI値の計算を行います。

コードは以下の通り。

```javascript
const height = msg.height;
const weight = msg.weight;
const bmiVal = (parseFloat(weight) / (parseFloat(height)/100 * parseFloat(height)/100)).toFixed(1);
const speechText = `あなたのBMIは${bmiVal}です。`;

msg.payload =
{
    "version": "1.0",
    "sessionAttributes": {},
    "response": {
        "outputSpeech": {
            "type": "SimpleSpeech",
            "values": {
                "type": "PlainText",
                "lang": "ja",
                "value": speechText
            }
        },
        "card": {},
        "directives": [],
        "shouldEndSession": false
    }
}
return msg;
```

| 項目       |       値 |
|:-----------------|:------------------|
|名前|BMI計算|

![s607](images/s607.png)

出力カテゴリにある`http response`ノードをドラッグアンドドロップし、ノードを繋ぎます。

![s608](images/s608.png)

### 3-2. 体重を聞き出す
身長のセッションがまだ格納されていない場合は値を一旦変数に格納しておきます。
機能カテゴリにある`change`ノードをドラッグアンドドロップし、線で繋ぎます。
ノードをクリックして、変数に格納していきます。

| 項目       |       値 |
|:-----------------|:------------------|
|名前|身長取得|
|④値の代入|height|
|⑤対象の値プルダウンメニュー|msg.|
|⑥対象の値|payload.request.intent.slots.num.value|

![s609](images/s609.png)

機能カテゴリにある`function`ノードをドラッグアンドドロップし、ノードを繋ぎます。
ノードをクリックして、Clovaのセッションに身長データを渡しています。
4行目の`sessionAttributes`にセッション情報を格納することができます。

```javascript
msg.payload =
{
    "version": "1.0",
    "sessionAttributes": {
        "height": msg.height
    },
    "response": {
        "outputSpeech": {
            "type": "SimpleSpeech",
            "values": {
                "type": "PlainText",
                "lang": "ja",
                "value": "体重をキログラムで教えてね"
            }
        },
        "card": {},
        "directives": [],
        "shouldEndSession": false
    }
}
return msg;
```

| 項目       |       値 |
|:-----------------|:------------------|
|名前|体重入力セリフ|

![s610](images/s610.png)

出力カテゴリにある`http response`ノードをドラッグアンドドロップし、ノードを繋ぎます。

![s611](images/s611.png)

### 3-3. スキル終了に対応する
機能カテゴリの`functions`ノードをドラッグアンドドロップし、ノードを繋ぎます。
ノードをクリックして、コードを入力します。
16行目の`shouldEndSession`がtrueだと会話が終わり、スキルを終了することができるようになります。

```javascript
msg.payload =
{
    "version": "1.0",
    "sessionAttributes": {},
    "response": {
        "outputSpeech": {
            "type": "SimpleSpeech",
            "values": {
                "type": "PlainText",
                "lang": "ja",
                "value": "ばいばい！"
            }
        },
        "card": {},
        "directives": [],
        "shouldEndSession": true
    }
}
return msg;
```

| 項目       |       値 |
|:-----------------|:------------------|
|名前|SessionEndedRequestセリフ|


![s612](images/s612.png)

出力カテゴリにある`http response`ノードをドラッグアンドドロップし、ノードを繋ぎます。

![s613](images/s613.png)

### 3-4. デプロイする
全体の図は以下の通りです。全てのノードが繋がっているか確認して、
デプロイボタンをクリックしてください。

![s614](images/s614.png)

デプロイが終われば、シミュレーターでテストしてみましょう。
身長と体重を入力するとBMIが返ってきます。

![s615](images/s615.png)

