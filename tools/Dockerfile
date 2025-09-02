FROM debian:bullseye-slim

# LABEL maintainer="tu-email@ejemplo.com"
# LABEL description="Container with static analysis and formatting tools for multiple languages"

ENV DEBIAN_FRONTEND=noninteractive
WORKDIR /app

# Install system dependencies and base languages
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl git \
    openjdk-17-jdk \
    clang clang-format clang-tidy \
    nodejs npm \
    python3 python3-pip python3-venv \
    perl cpanminus \
    golang-go \
    shellcheck \
    && rm -rf /var/lib/apt/lists/*

# Java tools
RUN curl -o /usr/local/bin/checkstyle.jar -L \
    https://github.com/checkstyle/checkstyle/releases/download/checkstyle-10.12.7/checkstyle-10.12.7-all.jar

# JavaScript tools
RUN npm install -g prettier eslint

# Python tools
RUN pip install --no-cache-dir \
    autopep8 \
    unify \
    eradicate \
    isort \
    ssort \
    teyit \
    pylint \
    black \
    pybetter

# Shell tools
RUN curl -sS https://webi.sh/shfmt | bash \
    && mv ~/.local/bin/shfmt /usr/local/bin/shfmt

# Perl tools
RUN cpanm --notest \
    Perl::Critic \
    Perl::Tidy \
    App::perlimports

# Cleanup
RUN apt-get clean && rm -rf /tmp/* /var/tmp/*

# Add entrypoint
COPY pre-commit.sh /usr/local/bin/pre-commit
RUN chmod +x /usr/local/bin/pre-commit

ENTRYPOINT ["/usr/local/bin/pre-commit"]
