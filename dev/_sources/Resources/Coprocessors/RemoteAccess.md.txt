# Remote access

## SSH

## GNU Screen

[manual](https://www.gnu.org/software/screen/manual/html_node/Overview.html) ([pdf](https://www.gnu.org/software/screen/manual/screen.pdf))

GNU Screen lets you create sessions that stay running after disconnecting from SSH. This is useful if you need to run a long compile job or if your SSH connection is unreliable.

Install screen:

```bash
sudo apt install screen
```

Create a `screen` session with `screen -dm -S` [name of screen] [command to run], so that your session can stay after you disconnect from SSH.

To look at the screen session, use `screen -r` [name of screen].

To stop looking at the screen session, use `Ctrl+A` then press `d`.

If there's someone attached, and you want to kick them off and attach yourself, use `screen -d -r` [name of screen].

If there's someone attached and you want to attach yourself without kicking them off, use `screen -x` [name of screen].
