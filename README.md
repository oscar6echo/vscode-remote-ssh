# VS Code Remote SSH - User Guide

## Generic Method

- Create key pair:

  ```bash
  mkdir -p ~/.ssh
  # 2048 or 4096
  ssh-keygen -t rsa -b 2048 -C oscar@central -f ~/.ssh/id_rsa_lagrange
  ```

- Show public key:

  ```bash
  cat ~/.ssh/id_rsa_lagrange.pub
  ```

- Copy paste to VM instance on [gcloud console](https://console.cloud.google.com/compute/instances)

  ```bash
  # Example
  # project "remote-dev-o6e"
  # zone "europe-west1-c"
  # instance "lagrange"
  ```

- Connect:

  ```bash
  # get external-ip from glcoud console

  # ssh -i ~/.ssh/id_rsa oscar6@external-ip
  ssh -i ~/.ssh/id_rsa_lagrange oscar@34.77.53.10
  ssh oscar@34.77.53.10

  # ssh -i ~/.ssh/id_rsa oscar6@machine-name
  ssh -i ~/.ssh/id_rsa_lagrange oscar@lagrange.europe-west1-c.remote-dev-o6e
  ```

- Add remote machine to `~/.ssh/known_hosts`:

  ```bash
  ssh-keyscan -H 34.77.53.10 >> ~/.ssh/known_hosts
  ```

- Append/create `~/.ssh/config` file with:

  ```bash
    # manual
    #
    Host lagrange
        HostName 34.77.53.10
        IdentityFile /home/olivier/.ssh/id_rsa_lagrange
        User oscar
        UserKnownHostsFile=/home/olivier/.ssh/known_hosts
        IdentitiesOnly=yes
        CheckHostIP=no
    # end manual
  ```

## GCloud Specific

- Create ssh key pair if necessary:

  ```bash
  gcloud compute config-ssh
  ```

- Check result

  ```bash
  cd ~/.ssh
  cat config # human readable info
  cat google_compute_engine # RSA private key
  cat google_compute_engine.pub # RSA public key
  cat google_compute_known_hosts # machines confirmed as known by user
  ```

- Connect

  ```bash
   ssh oscar@lagrange.europe-west1-c.remote-dev-o6e
  ```

# VS Code

- Install [Remote SSH extention](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)
- "**Remote-SSH: Connect to Host...**"
- Select "lagrange"
- Once connected
- Fix terminal:

  - "**Open Folder**" `/home/oscar`
  - Navigate to and create or edit `/home/oscar/.vscode-server/data/Machine/settings.json`
  - Add `"terminal.integrated.shell.linux": "/usr/bin/bash"`
  - Cf this [vscode-remote-release issue #1154](https://github.com/microsoft/vscode-remote-release/issues/1154#issuecomment-714660942)

- Open terminal to see where you are:

  ```bash
   # hardware
   lscpu
   # os
   uname -a
  ```
