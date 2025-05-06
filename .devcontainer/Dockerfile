FROM n8nio/base:20

RUN apt-get update && \
    apt-get install -y --no-install-recommends openssh-client sudo shadow bash && \
    rm -rf /var/lib/apt/lists/*
RUN echo node ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/node && chmod 0440 /etc/sudoers.d/node
RUN mkdir /workspaces && chown node:node /workspaces
USER node
RUN mkdir -p ~/.pnpm-store && pnpm config set store-dir ~/.pnpm-store --global
