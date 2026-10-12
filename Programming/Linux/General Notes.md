### View and Unblock Ports
I have a network port that is blocked
Probably because I'm trying to start a program that is already running

Trying to install netstat with
```
sudo apt install net-tools
```
Tried listing everything to no avail:
```
sudo netstat -putan | grep ssh
```
This seemed to work
```
ss -a4 | less
```

