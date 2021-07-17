# SSH

Secure Shell

## Ref

[How Secure Shell Works (SSH) - Computerphile - YouTube](https://www.youtube.com/watch?v=ORcvSkgdA58&t=473s)

[SSH Tunneling Explained - YouTube](https://www.youtube.com/watch?v=AtuAdk4MwWw)

## Basic

You need `ssh` on local machine and `sshd` on the server.

Note that the owner of `~/.ssh` must be the user instead of root.

Advanced usages such as local & remote port forwarding will not be covered here.

### Key generation

```bash
ssh-keygen
```

on your machine to generate public (`~/.ssh/id_rsa.pub`) and private key `~/.ssh/id_rsa`

### Sending key to server

Put public key into `~/.ssh/authorized_keys` on the server (password needed).

```bash
cat ~/.ssh/id_rsa.pub | ssh wusuowei@192.168.1.10 "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

or

```
ssh-copy-id -i ~/.ssh/id_rsa.pub wusuowei@192.168.1.10
```

`-i ssh/id_rsa.pub` is the default and can be omitted.

### Secure copy

```bash
scp ~/test.txt wusuowei@192.168.1.10:~
```

Secure copy from local machine to server.

## Disable root password login (and thus enforce using key)

```bash
sudo nano /etc/ssh/sshd_config
```

and set `PermitRootLogin no` to avoid brute-force attack.

Also, set `PasswordAuthentication no` and `sudo systemctl reload sshd`.

## ssh-agent

This is a little bit complex.

For the functionality of `ssh-agent`, refer to [ssh转发代理：ssh-agent用法详解 - 骏马金龙 - 博客园 (cnblogs.com)](https://www.cnblogs.com/f-ck-need-u/p/10484531.html). Super helpful.

## The difference between SSH and SSL (or TLS)

### Description and definition

According to Wikipedia,

> The Secure Shell Protocol (SSH) is a **cryptographic network protocol** for operating network services securely over an unsecured network. Typical applications include remote command-line, login, and remote command execution, but any network service can be secured with SSH.

> Transport Layer Security (TLS), the successor of the now-deprecated Secure Sockets Layer (SSL), is a **cryptographic protocol** designed to provide communications security over a computer network. Several versions of the protocol are widely used in applications such as email, instant messaging, and voice over IP, but its use as the Security layer in HTTPS remains the most publicly visible.

### Similarity

They exists for the same reason: Secure transport over network.

They both rely on things like TCP to establish connection. 

They can share the same encryption algorithm. By the way, they both leverage asymmetric
encryption.

### Difference

On the high level, they are quite different. SSH is specially for shell. SSH is a
protocol, but is more like an standalone application, while SSL is just a protocol and
the actual payload and transmission should rely on other protocols, such as HTTP.

SSL requires the third party (CA) to provide certification, while SSH uses a
username/password authentication system.

### OpenSSH and OpenSSL

These are open source implementation of SSH and SSL protocol.

OpenSSH is more of an application, and OpenSSL is more of an library. OpenSSH relies on
OpenSSL to do encryption, but of course doesn't need the protocol part of OpenSSL.

Like SSH, OpenSSH is specially for remote shell connection. It provides very powerful and
flexible functionality to do this well.

### Ref

Haven't found many helpful references on this. Maybe this is not a good question to ask.

[encryption - What is the difference between SSL vs SSH? Which is more secure? - Information Security Stack Exchange](https://security.stackexchange.com/questions/1599/what-is-the-difference-between-ssl-vs-ssh-which-is-more-secure)

[tls - How is OpenSSL related to OpenSSH? - Information Security Stack Exchange](https://security.stackexchange.com/questions/3424/how-is-openssl-related-to-openssh)

[ssh和ssl的联系和区别_weixin_33901843的博客-CSDN博客](https://blog.csdn.net/weixin_33901843/article/details/85865911)


## SSH config file

`man ssh-config` gives you enough information. There are a lot configuration options, but
key takeaways are:

- The config file is to spare you the effort of typing all these options at command line
  every time.
- Most of the options are advanced and will probably never be toggled.
- Above all,
  > SSH obtains configuration data from the following sources in the following order:
  >
  >        1.   command-line options
  >        2.   user's configuration file (~/.ssh/config)
  >        3.   system-wide configuration file (/etc/ssh/ssh_config)
  >
  > For each parameter, **the first obtained value will be used**. So  more host-specific declarations should be given near the beginning of the file, and general defaults at the end.
