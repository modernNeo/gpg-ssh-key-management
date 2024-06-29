# Setting up GPG authentication with Git on Debian

 - [Install GPG program](#install-gpg-program)
 - [GPG Secret Key Management](#gpg-secret-key-management)
    - [List Keys to get IDs](#list-keys-to-get-ids)
    - [Create](#create)
    - [Export](#export)
    - [Import](#import) 
 - [Turn on signing for Git](#turn-on-signing-for-git)
 - [Direct Git to use gnupg for GPG program](#direct-git-to-use-gnupg-for-gpg-program)
 - [Setup Git Identity to match the identify on the GPG key](#setup-git-identity-to-match-the-identify-on-the-gpg-key)
 - [Configure caching of password on GPG agent](#configure-caching-of-password-on-gpg-agent)
    - [Customizing gpg-agent.conf](#customizing-gpg-agentconf)
    - [Restarting GPG-Agent to Ensure gpg-agent.conf is valid](#restarting-gpg-agent-to-ensure-gpg-agentconf-is-valid)
 - [Direct Pinentry to ask via CLI](#direct-pinentry-to-ask-via-cli)
 - [Misc Links](#misc-links)


https://wiki.archlinux.org/title/GnuPG


## Install GPG program
`sudo apt-get -y install gnupg`

## GPG Secret Key Management
### List Keys to get IDs
```bash
 $ gpg --list-keys
/home/(username)/.gnupg/pubring.kbx
-----------------------------
pub   rsa4096 2021-11-23 [SC]
      (KEY_ID)
uid           [ unknown] <git user.name> <git user.email>
sub   rsa4096 2021-11-23 [E]
```

### Create
https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key
```bash
root@ubuntu-s-2vcpu-4gb-120gb-intel-fra1-01:~/test# gpg --full-generate-key
gpg (GnuPG) 2.4.4; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection? 1
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: <Full Name>
Email address: <email>
Comment: 
You selected this USER-ID:
    "<Full Name> <email>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
```

### Export
`gpg --armor --export-secret-keys (KEY_ID) > secret_key.asc`

### import
`gpg --import secret_key.asc`

## Turn on signing for Git
`git config [--global|--local] commit.gpgsign true`

## Direct Git to use gnupg for GPG program
`git config [--global|--local] gpg.program gpg2`

## Setup Git Identity to match the identify on the GPG key
```bash
git config [--global|--local] user.name 'full name'
git config [--global|--local] user.email 'email'
```

## Configure caching of password on GPG agent
### Customizing gpg-agent.conf
`~/.gnupg/gpg-agent.conf`  
https://superuser.com/a/1407685
```bash
default-cache-ttl 0
maximum-cache-ttl 0 || max-cache-ttl 0 # https://superuser.com/a/624488
# pinentry-program /usr/bin/pinentry-curses # make gpg-agent ask for passphrase on terminal, but it also results in the password not being stored in seahorse
# pinentry-program /usr/bin/pinentry-gnome3 # make gpg-agent ask for passphrase in GUI which offers the option to store the password in seahorse
```

### Restarting GPG-Agent to Ensure gpg-agent.conf is valid
#### Option 1. gpgconf command
 `gpgconf --kill gpg-agent`
#### Option 2. sourcing agent [show errors on launch]
https://stackoverflow.com/a/47056403
```bash
pkill -9 gpg-agent
. <(gpg-agent --daemon)
```

## Direct Pinentry to ask via CLI
> place in `~/.bashrc`

`export GPG_TTY=$(tty)`





## Misc Links
 * [How to Export a GPG Private Key and Public Key to a File?](https://itslinuxfoss.com/export-gpg-private-key-and-public-key-file/)
 * [Generating a new GPG key](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key)
 * [pinentry](https://www.gnupg.org/related_software/pinentry/index.html)
 * [How can I retrieve my ssh passphrase from gnome-keyring?](https://superuser.com/questions/482696/how-can-i-retrieve-my-ssh-passphrase-from-gnome-keyring)
 * [Python SecretStorage module](https://pypi.org/project/SecretStorage/)
 * [How to Back Up and Restore Your GPG Keys on Linux](https://www.howtogeek.com/816878/how-to-back-up-and-restore-gpg-keys-on-linux/)
 * [How to Export Private / Secret ASC Key to Decrypt GPG Files](https://stackoverflow.com/a/5588513)
 * [Associating an email with your GPG key](https://docs.github.com/en/authentication/managing-commit-signature-verification/associating-an-email-with-your-gpg-key)
 * [Adding a GPG key to your GitHub account](https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account)
 * [Generating a new GPG key](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key)
