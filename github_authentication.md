# Save Github Password to Debian Password Manager [Seahorse]

 - [Setup libsecret so that git can talk to seahorse](#setup-libsecret-so-that-git-can-talk-to-seahorse)
 - [clearing a credential from git-credential-libsecret](#clearing-a-credential-from-git-credential-libsecret)
 - [UNTESTED: disable use of GUI pop-up for credentials prompting](#untested-disable-use-of-gui-pop-up-for-credentials-prompting)
 - [Misc Links](#misc-links)
## Setup libsecret so that git can talk to seahorse
Libsecret: https://gnome.pages.gitlab.gnome.org/libsecret/  
Seahorse: https://wiki.debian.org/Seahorse
> Adapted from https://www.softwaredeveloper.blog/git-credential-storage-libsecret 
```bash
sudo apt-get install libsecret-1-0 libsecret-1-dev make gcc
cd /usr/share/doc/git/contrib/credential/libsecret
sudo make
git config --global credential.helper /usr/share/doc/git/contrib/credential/libsecret/git-credential-libsecret
```
> Sidenote: Setting up Git on WSL to use Windows credential Manager  
> `git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager.exe"`
## clearing a credential from git-credential-libsecret
https://stackoverflow.com/a/65813524
## UNTESTED: disable use of GUI pop-up for credentials prompting
> Adapted from  Section 4.4 of https://www.ece.uvic.ca/~frodo/courses/cpp/documents/github_authentication.pdf
```bash
git config --unset core.askPass
git config --global --unset core.askPass
sudo git config --system --unset core.askPass
unset GIT_ASKPASS
unset SSH_ASKPASS
```
## Misc Links
 * [Safely storing git credentials ](https://my-take-on.tech/2019/08/23/safely-storing-git-credentials/)
 * [Libsecret - remember Git credentials in Linux Mint and Ubuntu securely](https://www.softwaredeveloper.blog/git-credential-storage-libsecret)
