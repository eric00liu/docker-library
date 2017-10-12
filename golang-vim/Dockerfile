FROM golang:latest
MAINTAINER Michele Bertasi

ADD fs/ /

# install pagkages
RUN apt-get update                                                      && \
        apt-get install -y ncurses-dev libtolua-dev exuberant-ctags         && \
        ln -s /usr/include/lua5.2/ /usr/include/lua                         && \
        ln -s /usr/lib/x86_64-linux-gnu/liblua5.2.so /usr/lib/liblua.so     && \
        cd /tmp                                                             && \
# build and install vim
        git clone https://github.com/vim/vim.git                            && \
            cd vim                                                              && \
            ./configure --with-features=huge --enable-luainterp                    \
            --enable-gui=no --without-x --prefix=/usr                       && \
            make VIMRUNTIMEDIR=/usr/share/vim/vim80                             && \
            make install                                                        && \
# get go tools
            go get golang.org/x/tools/cmd/godoc                                 && \
                go get github.com/nsf/gocode                                        && \
                go get golang.org/x/tools/cmd/goimports                             && \
                go get github.com/rogpeppe/godef                                    && \
                go get golang.org/x/tools/cmd/gorename                              && \
                go get github.com/golang/lint/golint                                && \
                go get github.com/kisielk/errcheck                                  && \
                go get github.com/jstemmer/gotags                                   && \
                go get github.com/tools/godep                                       && \
                mv /go/bin/* /usr/local/go/bin                                      && \
# cleanup
rm -rf /go/src/* /go/pkg                                            && \
apt-get remove -y ncurses-dev                                       && \
apt-get autoremove -y                                               && \
apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# install vim plugins
RUN curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim && \
vim +PlugInstall +qall
