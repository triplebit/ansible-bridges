FROM docker.io/library/debian:bookworm-slim

ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8

RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    rm -f /etc/apt/apt.conf.d/docker-clean && \
    apt update && \
	apt-get install -y --no-install-recommends \
    bash \
    git \
    curl \
    wget \
    openssh-client \
    sudo \
    gnupg \
    ca-certificates \
    apt-transport-https

RUN wget -O- "https://keyserver.ubuntu.com/pks/lookup?fingerprint=on&op=get&search=0x6125E2A8C77F2818FB7BD15B93C4A3FD7BB9C367" | sudo gpg --dearmour -o /usr/share/keyrings/ansible-archive-keyring.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/ansible-archive-keyring.gpg] http://ppa.launchpad.net/ansible/ansible/ubuntu jammy main" | sudo tee /etc/apt/sources.list.d/ansible.list && \
    apt update && \
    apt install -y --no-install-recommends \
    ansible \
    ansible-lint

# Preserve command history across container restarts
RUN SNIPPET="export PROMPT_COMMAND='history -a' && export HISTFILE=/commandhistory/.bash_history" \
    && echo "$SNIPPET" >> "/root/.bashrc"

ENTRYPOINT [ "/bin/bash" ]
