# SSH

## config file: .ssh/config

With the following in the config file, `ssh orion` will connect to the server:

```bash
Host orion
    User scott
    HostName orion.dev
    IdentityFile ~/.ssh/id_rsa
```

## SCP

```bash
# Send local file to user home folder in server. Note the colon at the end.
scp localfile.txt scott@orion.dev:
# Send local file to a specific path in server
scp localfile.txt scott@orion.dev:/var/tmp
# Get file from server to current local folder
scp scott@orion.dev:filename.txt .
```

## SSH Tunnel

Example: a remote database server \(`orion.dev:3306`\) only allows local access. On local machine we have a GUI tool, which can't connect to the remote database directly.

Solution: On local machine, use the following line to connect local port 9000 with server's port 3306 \(via localhost:22, the port SSH uses\). Note that the `localhost` in the following line refers to the remote server.

```bash
ssh -L 9000:localhost:3306 scott@orion.dev
```

Then the GUI tool can connect to `localhost:9000` to access the remote database.

