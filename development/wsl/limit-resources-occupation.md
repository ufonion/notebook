# Limit the resouces occupation of WSL2

## Limit max memory occupation

By default, WSL 2 can use up to 80% memory of the Windows 10 host. We can add below configuraiton files to apply some hard-limition.

```conf:%UserProfile%\\.wslconfig
[wsl2]
memory=4GB
swap=8GB
localhostForwarding=true
```

## Clear memory cache periodically

1. Add cron job

  ```bash
  $ sudo crontab -e -u root
  */15 * * * * sync; echo 3 > /proc/sys/vm/drop_caches; touch /root/drop_caches_last_run
  ```

2. Start cron service by adding start script in user profile

  ```bash
  [ -z "$(ps -ef | grep cron | grep -v grep)" ] && sudo service cron start
  ```
3. Enable nopasswd to start cron service.

  ```bash
  $ sudo visudo
  %sudo   ALL=NOPASSWD: /usr/sbin/service cron start
  ```