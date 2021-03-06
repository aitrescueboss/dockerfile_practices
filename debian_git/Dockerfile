###############################################################
#                     Debian + git                            #
###############################################################

FROM debian

###############################################################
# 目的: 
# debianベースで, イメージビルド時点で最新verのgitを使えるdocker環境の構築
# 
# 基本的な戦略:
# 
# (1) apt-getでgitを入れる
# (2) (1)で入れたgitを使って, gitの最新版ソースをクローンする
# (3) (2)をmake
# (4) 余分なものを消す
#
# 詳細:
# - (1)のapt-getでは，gitの他にgitのソースmakeに必要な以下のものを入れる
#   (情報は [1] より)
#     * libcurl4-gnutls-dev
#     * libexpat1-dev gettext
#     * libz-dev
#     * libssl-dev
#     * make
#     * gcc
###############################################################
RUN set -ex && \
    cd /tmp && \
    # (1) apt-getで，「gitの最新版ソースを落とすためのgit」と，gitのmakeに必要なパッケージをインストール
    apt-get update && \
    apt-get install -y git libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev make gcc  && \
    # (2) gitの最新版ソースをクローン
    git clone git://git.kernel.org/pub/scm/git/git.git && \
    # (3) クローンしてきたソースをmake
    cd ./git  && \
    make prefix=/usr/local all  && \
    make prefix=/usr/local install  && \
    # (4) 余分なものを消す
    #     クローンしたソースのディレクトリ
    cd .. && rm -r git  && \
    #     gitのmakeに必要なパッケージ
    apt-get remove -y git libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev make gcc  && \
    #     各種依存パッケージ
    apt-get autoremove -y && \
    apt-get clean


#
# 参考文献
# [1] https://git-scm.com/, 「1.4 使い始める - Gitのインストール」
#
# メモ
#   Dockerのイメージ生成
#     $ docker build -t <tag> <Dockerfile-dir-path>
#   Dockerイメージの実行
#     $ docker run <tag> [-it|--rm|...]