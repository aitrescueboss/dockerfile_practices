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
    apt-get update && \
    apt-get install -y git libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev make gcc  && \
    git clone git://git.kernel.org/pub/scm/git/git.git && \
    cd ./git  && \
    make prefix=/usr/local all  && \
    make prefix=/usr/local install  && \
    cd .. && rm -r git  && \
    apt-get remove -y git libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev make gcc  && \
    apt-get autoremove -y && \
    apt-get clean


#
# 参考文献
# [1] https://git-scm.com/, 「1.4 使い始める - Gitのインストール」