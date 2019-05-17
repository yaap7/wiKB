## auto-ssh

Simple example:

```
autossh -M 10984 -N -f -R 4422:localhost:22 ubuntu@192.168.0.1 -p 80 -i ~/.ssh/private_key.pem &
```