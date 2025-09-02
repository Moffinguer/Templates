FROM debian:bullseye-slim

# LABEL maintainer="tu-email@ejemplo.com"
# LABEL description="Container with static analysis and formatting tools for multiple languages with configs"

ENV DEBIAN_FRONTEND=noninteractive
WORKDIR /app

# System deps + languages
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl wget git unzip build-essential \
    openjdk-17-jdk \
    clang clang-format clang-tidy \
    nodejs npm \
    python3 python3-pip python3-venv \
    perl cpanminus \
    golang-go \
    shellcheck \
    && rm -rf /var/lib/apt/lists/*

# Tools installation
# Java
RUN curl -o /usr/local/bin/checkstyle.jar -L \
    https://github.com/checkstyle/checkstyle/releases/download/checkstyle-10.12.7/checkstyle-10.12.7-all.jar

# JS
RUN npm install -g prettier eslint

# Python
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

# Shell
RUN curl -sS https://webi.sh/shfmt | bash \
    && mv ~/.local/bin/shfmt /usr/local/bin/shfmt

# Perl
RUN cpanm --notest \
    Perl::Critic \
    Perl::Tidy \
    App::perlimports

# Copy tools configs
COPY tools /opt/tools

RUN ln -s /opt/tools/clang/.clang-format /.clang-format && \
    ln -s /opt/tools/clang/.clang-tidy /.clang-tidy

# Environment vars
ENV PERLCRITIC=/opt/tools/perl/.perlcriticrc
ENV PERLTIDY=/opt/tools/perl/.perltidyrc
ENV PERLIMPORTS_TOML=/opt/tools/perl/.perlimports.toml
ENV PYLINTRC=/opt/tools/python/.pylintrc
ENV CHECKSTYLE_JAR=/opt/tools/java/checkstyle.jar
ENV CHECKSTYLE_CONFIG=/opt/tools/java/google_checks.xml

# Cleanup
RUN apt-get clean && rm -rf /tmp/* /var/tmp/*

# Entry
COPY pre-commit.sh /usr/local/bin/pre-commit
RUN chmod +x /usr/local/bin/pre-commit

ENTRYPOINT ["/usr/local/bin/pre-commit"]
