FROM debian

RUN apt-get update && apt-get install -y curl

RUN curl https://raw.githubusercontent.com/mcnilz/minicron/master/minicron > /usr/local/bin/minicron && \
    chmod +x /usr/local/bin/minicron

COPY crontab /crontab

CMD cat /crontab | minicron -e
