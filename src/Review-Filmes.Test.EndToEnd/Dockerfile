FROM mcr.microsoft.com/dotnet/sdk:8.0

# Configurar shell para detectar falhas em pipes
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Instalar dependências e limpar caches, sem pacotes extras
RUN apt-get update -y && \
    apt-get install -y --no-install-recommends wget xvfb unzip gnupg2 curl && \
    rm -rf /var/lib/apt/lists/*

# Configurar o Chrome PPA e instalar o Google Chrome
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
    echo "deb http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list && \
    apt-get update -y && \
    apt-get install -y --no-install-recommends google-chrome-stable && \
    rm -rf /var/lib/apt/lists/*

# Configurar variáveis de ambiente e diretório do Chromedriver
ENV CHROMEDRIVER_VERSION="126.0.6478.126"
ENV CHROMEDRIVER_DIR="/chromedriver"
RUN mkdir -p $CHROMEDRIVER_DIR

# Baixar, extrair e colocar Chromedriver no PATH
RUN wget -q --continue -P $CHROMEDRIVER_DIR "https://storage.googleapis.com/chrome-for-testing-public/$CHROMEDRIVER_VERSION/linux64/chromedriver-linux64.zip" && \
    unzip $CHROMEDRIVER_DIR/chromedriver* -d $CHROMEDRIVER_DIR && \
    rm $CHROMEDRIVER_DIR/chromedriver-linux64.zip

ENV PATH="$CHROMEDRIVER_DIR:$PATH"

# Entrypoint opcional para rodar Xvfb (ajuste conforme seus testes)
# ENTRYPOINT ["sh", "-c", "xvfb-run --server-args='-screen 0 1920x1080x24'"]
