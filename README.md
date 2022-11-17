# rails&jekyll
Railsとjekyllの共存ができるかの確認と、gitコマンドの練習とブランチの管理の練習。

## ブランチの作成。
ブランチの構成を以下の通りにする。

|ブランチ名|内容|
|-|-|
|main|Railsの変更を記録する|
|gh-pages|jekyllを使ったGitHub Pagesの内容|

[Creating a GitHub Pages site with Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)を参考にブランチの作成をする。

以下コマンドメモ。
```bash
$ rails new .
$ git add .
$ git commit -m "first commit"
$ git branch main
$ git checkout --orphan gh-pages
$ git clean -fdx && test $(git ls-files | wc -l) -eq 0 || git rm -rf .
$ mkdir -v docs && cd docs
$ jekyll new --skip-bundle .
# Gemfileを編集。github-pagesを有効に、jekyllを無効に
$ bundle install
$ cd ../
$ git add .
$ git commit -m "jekyllプロジェクトの作成"
$ git checkout main
$ git remote add origin git@github.com:Nagasaka-Hiroki/rails_and_jekyll.git
$ git push -u origin main
$ git push origin gh-pages
```
これで一応デプロイまでされているが、URLの設定はしていなかったので再設定して実行する。

しかし、`checkout`は一度コミットしないと切り替えられないらしく（切り替えられるがコミット前のデータが消失？）少し都合が悪い？  
もしrailsプロジェクトとjekyllプロジェクトの両方の書き換えが激しい場合、単純にリポジトリを分けたほうがいいかもしれない。

```bash
$ git add .
$ git commit -m "readme.mdの編集"
$ git checkout gh-pages
$ cd docs
# _config.ymlの編集
$ git add .
$ git commit -m "_config.ymlの編集"
$ git push origin gh-pages
```
どうやら、普通にpushした場合ディレクトリは`root`になるらしいので、設定で`./docs`に変更しなければダメそうだ。

結果問題なくデプロイはできた。
> - [https://nagasaka-hiroki.github.io/rails_and_jekyll/](https://nagasaka-hiroki.github.io/rails_and_jekyll/)

ひとまずメモは以上。

## 参考
> - [Creating a GitHub Pages site with Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)
> - [Gitリポジトリの中身を全部一気に消す](https://blog.s64.jp/entry/git-rm-all_safely/)
> - [git clean](https://www.atlassian.com/ja/git/tutorials/undoing-changes/git-clean)

## 追記
頻繁にブランチを移動して編集する場合、リポジトリを分けたほうがいいと書いたが、やり方としてリポジトリを一つにしてブランチを２つ作成。

その後、元のディレクトリを`main`ブランチにして、別のディレクトリにクローンしてブランチを`gh-pages`に切り替えて編集すればよいと思った。  
このようにすれば、リポジトリを一つにして毎回ブランチを切り替えて編集しなくともディレクトリで分けて管理することができる、と思った。

この方法が現状思いつく限りのベストだと思う。（本来の使用方法とは違うかもしれないが…。）