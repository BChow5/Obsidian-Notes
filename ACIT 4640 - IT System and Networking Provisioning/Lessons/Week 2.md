### SSH Review
### Generate a new key pair[](#generate-a-new-key-pair)

```
ssh-keygen -t ed25519 -f ~/.ssh/<key-name> -C "<commnet-to-identify-key>"
```

- t type, eg ed25519, rsa...
- f filename, specify the filename of the key you are creating
- C comment, provide a comment to help identify the key

### connect to a server using your ssh key[](#connect-to-a-server-using-your-ssh-key)

```
ssh -i ~/.ssh/<private-key> <user>@<host>
```

- i identity_file, your private key