# minicron

Painless cron jobs in docker

## create and execute (`-e`) job script

```bash
minicron -e << EOF
* * * * * echo "every minute"
*/2 * * * * echo "every 2 minutes"
0 0 * * 1-5 echo "weekdays midnight"
EOF
```
