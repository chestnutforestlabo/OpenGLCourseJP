---
layout: page
title: Makefileを用いたビルド
permalink: /setup/makefile/
---

MacやLinuxで作業する際に適当なIDEを使わずにターミナル上で作業する場合には`Makefile`を使ってビルドを行う方法もあります。

最初のプログラムである「[ウィンドウを開く]({% link pages/open_window.md %})」では全てのサンプルで使用可能な`Makefile`を用意しており、内容は以下のようになっています。このファイルは[GitHub](https://github.com/tatsy/OpenGLCourseJP/blob/master/src/open_window/Makefile)からもダウンロードできます。

{% highlight makefile linenos %}
# コンパイラの設定
CC          := clang
CXX         := clang++

# その他のコマンドの設定
RM          := rm
SH          := bash

# ソースコードの設定 (ファイルを追加する場合はここに足す)
SRC         := main.cpp
OBJS        := $(patsubst %.cpp, %.o, $(SRC))
DEPS        := $(patsubst %.cpp, %.d, $(SRC))

# コンパイラ引数の設定 (インクルード・ディレクトリ等)
CFLAGS      := -Wall -g -O2 -MP -MMD -I/usr/include -I/usr/local/include -I../../support -DGL_SILENCE_DEPRECATION
CXXFLAGS    := -std=c++11 $(CFLAGS)

# フレームワークの設定 (Mac特有のもの)
FRAMEWORKS  := -framework OpenGL -framework Cocoa -framework IOKit -framework CoreVideo

# リンカ引数の設定
LDFLAGS     := -L/usr/lib -L/usr/local/lib -lglfw3

# 出来上がるバイナリの名前 (適宜変更する)
PROGRAM     := open_window

# allターゲットの設定
.PHONY: all
all: $(PROGRAM)

# 依存ファイルのインクルード
-include $(DEPS)

# ソースコードのコンパイル
%.o: %.cpp
	$(CXX) $(CXXFLAGS) -c $< -o $@

# プログラムのリンク
$(PROGRAM): $(OBJS)
	$(CXX) -o $@ $^ $(LDFLAGS) $(FRAMEWORKS)

# プログラムの実行
.PHONY: run
run: $(PROGRAM)
	@./$(PROGRAM)

# コンパイル結果を削除する
.PHONY: clean
clean:
	@$(RM) $(PROGRAM) $(OBJS) $(DEPS)
{% endhighlight %}

自分の環境で使用する際には、コンパイラの設定である`CC`および`CXX`、そして出来上がるプログラムの名前を設定している`PROGRAM`を変更して使うようにしてください。