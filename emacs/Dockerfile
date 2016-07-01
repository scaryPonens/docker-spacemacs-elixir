FROM buildpack-deps:xenial-scm
MAINTAINER ScaryPonens "github.com/scaryPonens"

RUN apt-get update && apt-get install -y --no-install-recommends \
				emacs24 \
				libglu1-mesa \
				unifont \
		&& rm -rf /var/lib/apt/lists/*

RUN useradd -p $(openssl passwd -crypt EmacsDev) -u 1000 -m emacsdev \
		&& chown -R 1000:1000 /home/emacsdev

WORKDIR /home/emacsdev
USER emacsdev
CMD emacs
