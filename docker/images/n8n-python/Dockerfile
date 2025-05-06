# Use the Debian-based n8n image as a starting point
FROM n8nio/n8n:debian

# Switch to root to install Python and build tools
USER root

# Install build dependencies and compile Python 3.12 from source
ARG PYTHON_VERSION=3.12.0
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      wget build-essential libssl-dev zlib1g-dev libncurses5-dev libncursesw5-dev \
      libreadline-dev libsqlite3-dev libgdbm-dev libdb5.3-dev libbz2-dev \
      libexpat1-dev liblzma-dev tk-dev ca-certificates && \
    rm -rf /var/lib/apt/lists/* && \
    cd /usr/src && \
    wget https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz && \
    tar xzf Python-${PYTHON_VERSION}.tgz && \
    cd Python-${PYTHON_VERSION} && \
    ./configure --enable-optimizations && \
    make -j$(nproc) && \
    make altinstall && \
    ln -sf /usr/local/bin/python3.12 /usr/bin/python3 && \
    cd / && \
    rm -rf /usr/src/Python-${PYTHON_VERSION}*

# Create and activate a Python virtual environment
RUN python3 -m venv /opt/venv
ENV PATH="/opt/venv/bin:${PATH}"

# Copy and install Python dependencies
# Ensure a requirements.txt file is present at the build context root
COPY requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir -r /tmp/requirements.txt





WORKDIR /usr/src

# 2. Copie uniquement ton dossier local 'ad-dition' dans /usr/src/ad-dition
COPY ad-dition /usr/src/ad-dition

# 3. Passe dedans et installe
WORKDIR /usr/src/ad-dition
RUN pip install --no-cache-dir -e '.[ui]'





# Install the Python Function node into n8n custom nodes directory
RUN mkdir -p /home/node/.n8n/custom && \
    npm install --prefix /home/node/.n8n/custom n8n-nodes-python && \
    chown -R node:node /home/node/.n8n

# Revert to non-root user
USER node

# Expose default n8n port
EXPOSE 5678