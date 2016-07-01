Containerized Elixir + Alchemist + Spacemacs
==
![](https://dl.dropboxusercontent.com/u/46951970/emacs-docker.png)


Emacs 24.5 "Process" container
--
```dockerfile
FROM buildpack-deps:xenial-scm
MAINTAINER ScaryPonens

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
```

Spacemacs "Data" container
--
```dockerfile
FROM buildpack-deps:xenial-scm
MAINTAINER ScaryPonens

RUN mkdir -p /home/emacsdev \
                && git clone https://github.com/syl20bnr/spacemacs /home/emacsdev/.emacs.d \
                && chown -R 1000:1000 /home/emacsdev

VOLUME /home/emacsdev/.emacs.d
```

Docker Compose
--
```yaml
utt_proj:
    image: busybox
    volumes:
        - /Lab/Elixir:/home/emacsdev/elixirproj
    command: chown -R 1000:1000 /home/emacsdev
    labels:
        - "nature=data"

elixir:
    image: elixir
    volumes:
        - /usr/local
        - /usr/lib/erlang
    labels:
        - "nature=binary"

emacsd:
    image: scaryponens/spacemacs
    volumes:
        - /home/emacsdev/.emacs.d
    labels:
        - "nature=data"

spacemacs:
    image: scaryponens/emacs
    volumes_from:
        - emacsd
        - elixir
        - utt_proj
    environment:
        - DISPLAY=10.0.2.2:0
        - SHELL=bash
    labels:
        - "nature=process"
```

References
--
[https://github.com/cwahl-Treeptik/jdev-env-java](https://github.com/cwahl-Treeptik/jdev-env-java)
