# dasadc.github.ioをローカルでプレビューする方法

1. はじめに  

    dasadc.github.ioではマークダウン形式(*.md)のページを多用しているが、
    マークダウンの解釈が処理系によって異なるのが原因でレイアウトが期待した
    通りにならない場合がある。この問題を解決するため、ローカルでプレビュー
    する方法をがあることが富田氏のご指摘でわかったので、その方法を本資料に
    まとめた。なお、本資料は下記のWebページをベースにしているが、記載された
    方法そのままではうまくゆかない箇所があったため、その点を補足している。
    なお、以下はMacOS X 10.12.5を使用し、ターミナルでtcshを使用して操作した
    場合にうまくいった方法を記載している。それ以外のOS and/or shellを使用して
    いる場合は、下記を参考に使用しているOS/shellに合った方法を探索して
    頂けると幸いである。

    [1] [https://jekyllrb.com/docs/quickstart/](https://jekyllrb.com/docs/quickstart/)  
    [2] [https://github.com/pages-themes/cayman](https://github.com/pages-themes/cayman)  

2. 必要となるソフトウェアのインストール

    上記[1]の"requirements"のリンクにある通り、rubyの2.1以上が必要であるが、
    MacOS Xにプレインストールされているrubyは2.0であったため、最新版を
    インストールした。rubyのインストール方法としては、Homebrewを使用するのが
    ポピュラーな様だったので、まずHomebrewをインストールし、インストール
    したHomebrewを使用してrubyの最新版をインストールした。

    (1) Homebrewのインストール  

    下記の通りコマンドを入力する。途中でAppleのコマンドラインツールの
    インストールが行われ、これに結構時間がかかる。上記[1]にgccやmakeも
    必要とあるが、コマンドラインツールのインストールを行うとgccやmakeも
    インストールされるため、Homebrewをインストールするとgccやmakeを
    インストールする手間が省ける。
    筆者がインストールした時点では"brew \-\-version"で表示されるバージョンは
    1.2.3 であった。

    ```
    curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install | /usr/bin/ruby -
    (パスワード入力を求められたらスーパーユーザのパスワードを入力)
    rehash
    brew --version
    ```

    (2) ruby最新版のインストール

    下記の通りコマンドを入力する。筆者がインストールした時点では
    "ruby \-\-version"で表示されるバージョンは 2.4.1p111 であった。
    ```
    brew install ruby
    (パスワード入力を求められたらスーパーユーザのパスワードを入力)
    rehash
    ruby --version
    ```

3. jekyllツールのインストール

    下記の通りコマンドを入力する。
    ```
    sudo gem install jekyll bundler
    (パスワード入力を求められたらスーパーユーザのパスワードを入力)
    rehash
    ```

4. dasadc.github.ioのページの表示

    まず、GitHubからデータをダウンロードする。
    ```
    git clone https://github.com/dasadc/dasadc.github.io.git
    cd dasadc.github.io
    ```

    次に、Gemfile という名前のファイルを作成し、下記の２行を入力する。
    ```
    source 'https://rubygems.org'
    gem "github-pages", group: :jekyll_plugins
    ```

    最後に、下記の２つのコマンドを入力する。最初のコマンドで必要なファイルを
    インストールするためのものなので、１度実行すると２回目以降は不要である。
    ```
    bundle install
    bundle exec jekyll serve
    ```

    webブラウザでURLに http://127.0.0.1:4000/ と入力するとローカルに
    ダウンロードしたファイルが表示される。
    GitHubにアクセスした場合とほぼ同じレイアウトで表示されるが、
    上部に"View on GitHub"のボタン、
    下部に"dasadc.github.io is maintained by dasadc."のコメントが
    追加されている。また、マークダウン(\*.md)はHTML(\*.html)に変換して
    表示するしくみになっているので、URLを他人に連絡する場合は、
    拡張子を\*.htmlとして連絡する必要がある。マークダウン内のリンクの
    記述は同一サイト内であれば\*.mdと書いておくと\*.htmlに自動で変換されるが、
    "bundle exec jekyll serve"を実行した後に作成したファイルは同一サイト内の
    ファイルと認識されないため、新しいファイル作成した後には
    "bundle exec jekyll serve"を再実行する必要がある。

5. ページの修正

    本題から外れるが、ファイルを修正してGitHubに反映するまでの手順も
    記載しておく。まず、作業するbranchを作成する。例えば、adc2017dev
    というbranchを作成する場合は下記の通り(GitHub上に既に存在している
    branchをダウンロードして作業する場合は不要)。
    ```
    git branch adc2017dev
    ```

    次に、ローカルの環境を作業するbranchに切り替え、
    ```
    git checkout adc2017dev
    ```
    切り替えが正しく行われたことを確認する(この例の場合、adc2017devの
    左に"*"が表示されていればOK)。
    ```
    git branch
    ```

    この後ファイルを編集し、編集したファイルを指定してcommitする
    (ここでは、 memo/HowToPreviewLocally.md を編集した場合の例を示す)。
    git add で -A オプションを使用する場合は、OSが知らないうちに
    作成したファイルがcommitされない様に注意する。
    ```
    git add memo/HowToPreviewLocally.md
    git commit
    (エディタが開くのでcommitメッセージを入力して終了する)
    ```

    最後に、GitHubにpushするが、branchがGitHub上に存在しているかどうかで
    コマンドが異なる(いずれの場合もGitHubのID/パスワードの入力を求められる
    ので、入力する)。
    ```
    (a) branchがGitHub上に存在しない場合
    git push --set-upstream origin adc2017dev

    (b) branchがGitHub上に存在する場合
    git push
    ````

    あとはGitHub上でpullを行い、masterに反映する。

