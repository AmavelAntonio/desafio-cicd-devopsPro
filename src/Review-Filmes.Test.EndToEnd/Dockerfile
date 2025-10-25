FROM mcr.microsoft.com/dotnet/sdk:8.0

# Definir shell com 'pipefail' para capturar falhas em pipes
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Instalar dependências com versões fixadas e limpar caches
RUN apt-get update -y && \
    apt-get install -y --no-install-recommends \
        wget=1.20.3-1ubuntu2.1 \
        xvfb=2:1.20.8-3 \
        unzip=6.0-25ubuntu1.2 \
        gnupg2=2.2.19-3ubuntu2.5 \
        curl && \
    rm -rf /var/lib/apt/lists/*

# Configurar o repositório do Google Chrome e instalar a versão estável
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
    echo "deb http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list && \
    apt-get update -y && \
    apt-get install -y --no-install-recommends \
        google-chrome-stable=126.0.6478.126-1 \
        curl && \
    rm -rf /var/lib/apt/lists/*

# Definir variáveis de ambiente para o Chromedriver
ENV CHROMEDRIVER_VERSION="126.0.6478.126"
ENV CHROMEDRIVER_DIR="/chromedriver"
RUN mkdir -p $CHROMEDRIVER_DIR

# Baixar e extrair o Chromedriver
RUN wget -q --continue -P $CHROMEDRIVER_DIR \
        "https://storage.googleapis.com/chrome-for-testing-public/$CHROMEDRIVER_VERSION/linux64/chromedriver-linux64.zip" && \
    unzip $CHROMEDRIVER_DIR/chromedriver-linux64.zip -d $CHROMEDRIVER_DIR && \
    rm $CHROMEDRIVER_DIR/chromedriver-linux64.zip

# Adicionar o Chromedriver ao PATH
ENV PATH="$CHROMEDRIVER_DIR:$PATH"

# Definir o ponto de entrada para o Xvfb (ajuste conforme necessário)
# ENTRYPOINT ["sh", "-c", "xvfb-run --server-args='-screen 0 1920x1080x24'"]
