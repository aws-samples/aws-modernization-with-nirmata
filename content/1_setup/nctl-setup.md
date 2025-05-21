---
title: "Install NCTL" 
chapter: false
weight: 90 
---


This workflow requires [Nirmata Command Line Interface, nctl](https://downloads.nirmata.io/nctl/downloads/). Lets first install `nctl` on our VS Code Server:

```bash
curl -LO https://nirmata-downloads.s3.us-east-2.amazonaws.com/nctl/nctl_4.7.0/nctl_4.7.0_linux_386.zip
```

To unzip and add proper permissions:

```bash
unzip nctl_4.7.0_linux_386.zip
chmod +x nctl
sudo mv nctl /usr/local/bin/nctl
```

Let's verify the installation:

```bash
nctl version
```

Output should look similar to below as the version may have been updated since this publishing:

```
nctl version
Version: 4.7.0
Time: 2025-05-16T14:12:29Z
Git commit ID: 1035584dbfcb4ba2bd7fc7cfa33db8583e2919ed
```

You can also refer to the [documentation](https://downloads.nirmata.io/nctl/stablereleases/) for installation.

Log in to Nirmata, go to Settings → Profile, and copy the nctl login command to authenticate nctl with Nirmata.

![nctl login command](/images/nctl-copy.png)

Enter this command in the VS Code Server 

Output should look similar to below but with your Nirmata User ID and Token:
```
nctl login --url https://nirmata.io --userid youremail@email.com --token 7nE***
```
🚀 Congrats! You now have successfully installed NCTL and can move on to the next section! 
