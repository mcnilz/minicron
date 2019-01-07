# minicron

Painless cron jobs in docker.

```Dockerfile
FROM debian
RUN apt-get update && apt-get install -y curl

RUN curl https://raw.githubusercontent.com/mcnilz/minicron/master/minicron > /usr/local/bin/minicron && \
    chmod +x /usr/local/bin/minicron

COPY crontab /crontab
CMD cat /crontab | minicron -e
```

```cron
* * * * * echo "every minute"
*/2 * * * * echo "every 2 minutes"
0 0 * * 1-5 echo "weekdays midnight"
```

# benefits

- job output written to stdout and stderr going to docker log
- works as non-root, no setuid permission required
- very simple

## create and execute (`-e`) job script

```bash
minicron -e << EOF
* * * * * echo "every minute"
*/2 * * * * echo "every 2 minutes"
0 0 * * 1-5 echo "weekdays midnight"
EOF
```

## output job script

```bash
minicron -e << EOF
* * * * * echo "every minute"
*/2 * * * * echo "every 2 minutes"
0 0 * * 1-5 echo "weekdays midnight"
EOF
```

will output the following bash script:

```bash
#!/bin/bash
while true; do
  sleep $(expr 60 - $(date +%S))
  (echo "every minute") &
  if [[ $(date +"%-M") =~ ^((0|2|4|6|8|10|12|14|16|18|20|22|24|26|28|30|32|34|36|38|40|42|44|46|48|50|52|54|56|58))$ ]]; then (echo "every 2 minutes") & fi
  if [[ $(date +"%-M %-k %w") =~ ^((0) (0) (1|2|3|4|5))$ ]]; then (echo "weekdays midnight") & fi
done
```

It is just an endless loop running every minute. To check if a job must be executeed date and bash regular expressions are used.
