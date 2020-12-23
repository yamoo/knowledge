## Generate key

```
ssh-keygen -b 4096 -t rsa -C "" -m PEM
```

## Append pub key 

```
cat rsa.pub >> ~/.ssh/authorized_keys
```

## Access to remote

```
ssh -i rsa user@hostname
```

### To remote

```
scp -i rsa test.txt user@hostname:~/
```

### From remote

```
scp -i rsa user@hostname:~/test.txt ./
```
