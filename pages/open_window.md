---
layout: page
title: ウィンドウを開く
permalink: /open_window/
source: "https://github.com/tatsy/OpenGLCourseJP/blob/master/src/open_window/main.cpp"
---

## OpenGLの初期化

GLFWを通してOpenGLを使用するには初期化処理が必要です。初期化には以下のソースコードが対応します。

```c++
// OpenGLを初期化する
if (glfwInit() == GL_FALSE) {
    fprintf(stderr, "Initialization failed!\n");
    return 1;
}
```

上記のコードの中で`glfwInit()`メソッドが初期化処理をします。もし初期化が成功すれば
``GL_TRUE`` を、失敗すれば`GL_FALSE`を返します。

ここでは初期化が失敗し`GL_FALSE`が返されたときにエラーメッセージを表示して、プログラムを停止しています。
少し難しくなりますが`fprintf`関数の第1引数に`stderr`と書くとエラーメッセージとして、
第2引数に渡された文字列である *Initialization failed* が出力されます。

---

## ウィンドウの作成

ウィンドウを作成するには`glfwCreateWindow`関数を使います。この関数は、必ず上記の初期化の後で呼び出してください。
対応するソースコードは以下のようになります。

```c++
// Windowの作成
GLFWwindow *window = glfwCreateWindow(WIN_WIDTH, WIN_HEIGHT, WIN_TITLE,
                                      NULL, NULL);
if (window == NULL) {
    fprintf(stderr, "Window creation failed!");
    glfwTerminate();
    return 1;
}
```

実際にウィンドウを作成する関数は`glfwCreateWindow`関数です。こちらは5つの引数を取ります。
重要なのは最初の3つで、先頭から順に「ウィンドウの幅」、「ウィンドウの高さ」、「ウィンドウの名前」です。

残りの4つ目の引数はフルスクリーンモードにするときに使用する引数、5つ目の引数は別のウィンドウを描画内容を
共有するときに使う引数なのですが、このコースでは使用しないので、基本は`NULL`を渡しておけば大丈夫です。

この`glfwCreateWindow`関数はウィンドウを表す`GLFWwindow`型のポインタを返します。
ウィンドウの作成に失敗した場合には、このポインタが`NULL`になるので、もし`NULL`であれば、
エラーメッセージを出力してプログラムを停止します。

この時、`glfwInit`関数を呼び出したあとであれば、必ず`glfwTerminate`関数を呼ぶ必要があるので注意してください。

---

## コンテキストの生成

OpenGLでは描画の状態を保存するためにコンテキストと呼ばれるオブジェクトを作る必要があります。
これを今の時点で正確に説明するのは難しいのですが、概ね、OpenGLで扱う描画の状態等を保存するためのものだと思ってください。

このコンテキストを生成するためのコードは以下の行になります。

```c++
// OpenGLの描画対象にWindowを追加
glfwMakeContextCurrent(window);
```

## 背景色の設定

今回のプログラムでは、背景に色をつけて画面を表示します。
その背景の描画色を決定する関数が`glClearColor`関数です。
対応するソースコードは以下の部分です。

```c++
// 背景色の設定
glClearColor(1.0f, 0.0f, 0.0f, 1.0f);
```

`glClearColor` 関数は4つの引数を持ち、それぞれが赤、緑、青、アルファ値に対応します。
各色ちゃんねるの値は0から1までの値を取り、例えば`(1, 1, 1, 1)`が与えられると白色に、
`(1, 0, 0, 0.5)`が与えられると透明度が50%の赤色になります。


## 描画ループの設定

```c++
// メインループ
while (glfwWindowShouldClose(window) == GL_FALSE) {
    // 描画
    drawOpenGL();

    // 描画用バッファの切り替え
    glfwSwapBuffers(window);
    glfwPollEvents();
}
```

この部分ではウィンドウが表示されている限り続くwhileループが書かれています。
まず、whileの終了判定に使われている`glfwWindowShouldClose`関数は、
現在ウィンドウが開かれているのか閉じられているのかを調べます。
閉じられているときには`GL_TRUE`が返ってくるので、そうでない場合、
すなわち`GL_FALSE`が返ってきている間は描画を継続します。

描画部分は`drawOpenGL`関数の中で処理を記述することにします。
現在のこの関数の定義は、このようになっています。

```c++
// ユーザ定義のOpenGL描画
void drawOpenGL() {
    // 背景色の描画
    glClear(GL_COLOR_BUFFER_BIT);
}
```

ここでは、先ほど`glClearColor`関数で設定した色を使って画面を塗りつぶす処理を書いています。
その役割を担うのが`glClear`関数です。この関数の引数には現在`GL_COLOR_BUFFER_BIT`という
定数が渡されていますが、この定数が渡されているときには`glClear`関数により画面の色が塗りつぶされます。


## 描画結果

全てのソースコードが上手くかけていれば、以下のような赤一色で塗りつぶされた画面が表示されるはずです。

{% include lightbox.html src="/assets/images/open_window/001_opengl_window.jpg" data="image-1" %}

---

## 練習

1. ウィンドウの幅や高さを変更したときの変化を調べる
2. ウィンドウのタイトルを変更したときの変化を調べる
3.`glClearColor`に渡されている色を別の色に変更して、変化を調べる
4.`glClear`をコメントアウトしたときに背景色がどうなるかを調べる
