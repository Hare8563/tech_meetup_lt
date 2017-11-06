# gulpで始めるフロントエンドアセット管理

晴佐久 哲士

---

皆さん、フロントエンドのアセット管理ってどうやってますか  


※ アセットとはcss, javascriptはもちろん、画像ファイルやjsonファイルといったviewに絡むもの全般のこと

---

近年、フロントエンド周りの変化は著しく、それらすべてを効率的に処理するのは難しいです

---

- メタ言語からのコンパイル
- ES6からES5へのトランスパイル
- 分割したモジュールの結合
- スクリプトの圧縮
- 暗号化
- テスト
- Lintを使った静的解析

---

こんなにたくさんのこと毎回やってらんないよ

---

そこでgulpです

---

## gulpとはなんだ

- 「タスクランナー」と呼ばれるツール
- ウェブ制作でやるべきことを「タスク」というかたちで定義しておくことで、アセット管理の一連の作業を自動で行ってくれる
- ファイルの処理をストリームで行う「ストリーミングビルドシステム」が一番の特徴
  - イメージとしては、シェルスクリプトのパイプが近い

---

``` #!javascript
// gulpfile.js

const gulp = require("gulp");

gulp.task("copy", () => {                 // "copy"はタスク名
  return gulp.src("./*.js")               // リソースファイルを指定
             .pipe(dest("dist/script"));  // 処理結果のファイルをアウトプット
});

```

---

Gulpなんてこれだけです

---

基本この組み合わせです

---

おわり

---

ちょっと味気ないんで、実際にはこんな感じで使っていきます

---
## タスク1 coffeeスクリプトコンパイル

``` #!javascript
// build.js

const gulp = require("gulp");
const coffee = require("gulp-coffee");

gulp.task("build", () => {
    return gulp.src("./*.coffee")
               .pipe(coffee())
               .pipe(gulp.dest("dist/"));
});

```

---
## タスク2 圧縮

```#!javascript
// minimize.js

const gulp = require("gulp");
const rename = require('gulp-rename');
const uglify = require('gulp-uglify');

gulp.task("minimize", () => {
    return gulp.src("./dist/*.js")
               .pipe(uglifiy())
               .pipe(rename({
                 extname: ".min.js"
                }))
               .pipe(gulp.dest("dist/"));
});

```

---
## タスク3 組み合わせ

``` #!javascript
// gulpfile.js
const gulp = require("gulp");
const runSequence = require('run-sequence');

// タスク読み込み
require("./tasks/build");
require("./tasks/minimize");

// `npm run gulp`のようにコマンド入力すると実行される
gulp.task("default", () => {
  return runSequence("build_coffee", "minimize"); // coffeeビルド -> 圧縮
});

```

---
## 某FX案件採用例

- webpackと組み合わせて、モジュール解決と適切なファイル配置
- coffeeスクリプトのビルド
- ES6 -> ES5へのトランスパイル
- 文字列の置換

---

## 今後やりたいこと

- watchでビルド処理の自動化
- karmaを使ったテスト自動化

---

今度こそ終わり

---

次回(次々回?)予告

- webpackで始めるアセットバンドル
- FLUXってなんですか
- Reactぶっちゃけ

のどれかします
