teamLab.net コマンドラインツール
================================

Installation
------------

このディレクトリで、以下のコマンドを実行して実行環境を用意する。

  gem install bundle
  bundle install --path vendor/bundle


画像のダウンロード
==================

以下のコマンドを実行するとコンテンツページの画像がダウンロードされる

ruby Thorfile get_image --url=[コンテンツページのURL]

以下のオプションが使える
* --name=[ディレクトリ名/ファイル名]
* --output=[保存先のディレクトリパス]

