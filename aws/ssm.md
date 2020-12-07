# Session management

https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-sessions-start.html#sessions-start-ssh

## Install SSM plugin

```
brew search session-manager-plugin
```

## Start new session

```
aws ssm start-session --target i-12345
```

## SSH via SSM

https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started-enable-ssh-connections.html
https://dev.classmethod.jp/articles/session-manager-launches-tunneling-support-for-ssh-and-scp/

### Update ~/.ssh/config.

```
# SSH over Session Manager
host i-* mi-*
    ProxyCommand sh -c "aws ssm start-session --target %h --document-name AWS-StartSSHSession --parameters 'portNumber=%p'"
```

### ssh

```
ssh -i ~/path/to/key.pem ec2-user@i-12345
```

### ssh port forwarding

```
ssh -i ~/path/to/.pem ec2-user@i-123 -L 9999:remote-host:9999
```
