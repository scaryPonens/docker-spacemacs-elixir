FROM buildpack-deps:xenial-scm
MAINTAINER ScaryPonens "github.com/scaryPonens"

RUN mkdir -p /home/emacsdev \
		&& git clone https://github.com/syl20bnr/spacemacs /home/emacsdev/.emacs.d \
		&& chown -R 1000:1000 /home/emacsdev

VOLUME /home/emacsdev/.emacs.d
