# Ncat

Port forwarding as below. 

```bash
$ ncat -l 8081 --sh-exec 'ncat localhost 7890' -k
```



