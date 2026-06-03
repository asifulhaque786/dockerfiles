FROM ubuntu:resolute
LABEL maintainer="Divyam Azad divyam@obmondo.com"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN apt-get update     \
    && apt-get install --no-install-recommends -y \
        apt-utils \
        ca-certificates \
        lsb-release \
        pigz \
        python3-pip \
        python3-setuptools \
        curl \
        jq \
        gnupg \
        gcc \
        libffi-dev \
        unzip \
    && curl -sL https://aka.ms/InstallAzureCLIDeb | bash \
    && export CPU_ARCHITECTURE=$([ "$(uname -m)" = "x86_64" ] && echo "x86_64" || echo "aarch64") \
    && curl -sSf "https://awscli.amazonaws.com/awscli-exe-linux-${CPU_ARCHITECTURE}.zip" -o /tmp/awscli.zip \
    && unzip -q /tmp/awscli.zip -d /tmp \
    && /tmp/aws/install \
    && rm -rf /tmp/awscli.zip /tmp/aws \
    && pip3 install --no-cache-dir --break-system-packages gsutil \
    && mkdir -p /etc/apt/keyrings \
    && curl --silent https://www.postgresql.org/media/keys/ACCC4CF8.asc | gpg --dearmor > /etc/apt/keyrings/pgdg.gpg \
    && echo "deb [signed-by=/etc/apt/keyrings/pgdg.gpg] http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list \
    && cat /etc/apt/sources.list.d/pgdg.list \
    && apt-get update \
    && apt-get install --no-install-recommends -y  \
        postgresql-client-18  \
        postgresql-client-17  \
        postgresql-client-16  \
        postgresql-client-15  \
        postgresql-client-14  \
        postgresql-client-13  \
        postgresql-client-12  \
        postgresql-client-11  \
        postgresql-client-10  \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

COPY ./* ./

ENV PG_DIR=/usr/lib/postgresql


ENTRYPOINT ["/dump.sh"]
