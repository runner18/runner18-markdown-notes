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

## Public and Private keys
If you use ssh for Git instead of https

On the local computer, generate a public key and private key
```
ssh-keygen -t rsa -b 4096 -C "gmylastname@gmail.com" -f ./my-ssh.key
```

- Public key: Copy-paste onto server
- Private key: Keep on local computer only

Keep the private key on your local computer only

Can place copy of private key into WSL

### Troubleshooting: UNPROTECTED PRIVATE KEY FILE
Linux doesn't want your private key readable/writable by everyone on the computer.
Change it using the chmod (change mode) command:
```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
ls -l ~/.ssh/id_rsa

```
