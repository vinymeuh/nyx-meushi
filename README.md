# nyx-meushi

## How to use

### Inital install

1. Install [Arch Linux](https://github.com/vinymeuh/nyx-meushi/blob/master/notes/INSTALL-ARCHLINUX.md)

2. Install **pyenv** using [pyenv installer](https://github.com/pyenv/pyenv-installer)

3. Create Python **virtualenv**:

```shell
pyenv virtualenv system nyx-meushi
```

4. Install **nyx-meushi**

```shell
cd $HOME
git clone https://github.com/vinymeuh/nyx-meushi
cd nyx-meushi
pyenv activate nyx-meushi
pip install -r requirements.txt
ansible-galaxy install -r requirements.yml
```

### Ansible playbooks

```shell
cd ~/nyx-meushi
# Base system
ansible-playbook setup-root-base.yml --check -K
ansible-playbook setup-root-base.yml -K
# User applications
ansible-playbook setup-root-userapps.yml --check -K
ansible-playbook setup-root-userapps.yml -K
# User setup
ansible-playbook setup-user.yml --check
ansible-playbook setup-user.yml
```

### AURs packages

AURs packages are manually managed using **aur_builder** user:

```shell
sudo su - aur_builder
git clone https://aur.archlinux.org/<package_name>.git
cd <package_name>
makepkg -si
```

AUR packages installed:

* https://aur.archlinux.org/timeshift.git 
* https://aur.archlinux.org/timeshift-autosnap.git 
* https://aur.archlinux.org/art-rawconverter.git

## Pacman notes & tips

Package cache is cleanup periodically using the systemd timer **paccache.timer**.

* To display a summary of installed packages: ```pacreport```
* See Pacman log: ```paclog```
* List explicitly installed packages: ```pacman -Qe```
* List orphans packages: ```pacman -Qtdq```
* Purge Pacman cache (**/var/cache/pacman/pkg**): ```paccache -r```
* List of install packages sorted by install date: ```expac --timefmt='%F %T' '%l %n' | sort -n```
* Detailed information about a package: ```pacinfo <pkg>```
