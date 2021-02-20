# SSH

Secure Shell

## Basic

You need `ssh` on local machine and `sshd` on the server.

`ssh-keygen` on your machine

Generate public (~/.ssh/id_rsa.pub) and private key (~/.ssh/id_rsa)

`cat ~/.ssh/id_rsa.pub | ssh wusuowei@192.168.1.10 "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys"`

Put public key into `~/.ssh/authorized_keys` on the server (need password).

`scp ~/test.txt wusuowei@192.168.1.10:~`

Secure copy from local machine to server

`ssh-add ~/.ssh/id_rsa_another`

Add key (or you will get permission denied error if you are using a different key).

## Disable root password login (must use key)

`sudo nano /etc/ssh/sshd_config`

and set

`PermitRootLogin no` to avoid brute-force attack

`PasswordAuthentication no`

`sudo systemctl reload sshd`

## Use github with SSH

Make sure to set the owner of `~/.ssh` to the user instead of root, generate a key and add it to local and github.

``eval `ssh-agent -s` `` to activate the ssh agent

If could not open a connection to your authentication agent.
