# WSL2 KALI with Systemd/Systemclt

**Packages:**

```shell
wget https://packages.microsoft.com/config/debian/10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb

pushd /tmp
wget --content-disposition \
    "https://github.com/arkane-systems/genie/releases/download/v1.44/systemd-genie_1.44_amd64.deb"

  popd

sudo apt-get update
sudo apt-get install -y apt-transport-https build-essential git kali-win-kex dotnet-sdk-5.0 gdebi lsb-release systemd-container daemonize gawk
sudo rm -f /usr/sbin/daemonize
sudo ln -s /usr/bin/daemonize /usr/sbin/daemonize

wget -O /etc/apt/trusted.gpg.d/wsl-transdebian.gpg https://arkane-systems.github.io/wsl-transdebian/apt/wsl-transdebian.gpg

chmod a+r /etc/apt/trusted.gpg.d/wsl-transdebian.gpg

cat << EOF > /etc/apt/sources.list.d/wsl-transdebian.list
deb https://arkane-systems.github.io/wsl-transdebian/apt/ buster main
deb-src https://arkane-systems.github.io/wsl-transdebian/apt/ buster main
EOF

apt update

sudo apt-get install -y systemd-genie
```

**Config:**

sudo nano /usr/lib/systemd/system/systemd-sysusers.service

```properties
#LoadCredential=passwd.hashed-password.root
#LoadCredential=passwd.plaintext-password.root
#LoadCredential=passwd.shell.root
LoadCredential=
```

Open the file (~ /.zshrc or ~/.bashrc) and paste the code on top

```shell
sudo nano ~/.zshrc
```

```shell
# Start genie botte
if [[ `kex status | grep running` ]]; then
        if [[ `genie -r | grep -v running` ]]; then
                read -q "yn? * Not inside the genie bottle; enter it (Y/N)? " yn
                echo
                if [[ $yn == "y" ]]; then
                        echo "Starting genie:"
                        exec /usr/bin/genie -s
                fi
        fi
fi
```

## Docker

**Install:**

```apache
sudo apt update

sudo apt install -y docker.io nftables

sudo usermod -aG docker $USER

printf '%s\n' "deb https://download.docker.com/linux/debian bullseye stable" | sudo tee /etc/apt/sources.list.d/docker-ce.list

curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker-ce-archive-keyring.gpg

sudo apt update

sudo apt install -y docker-ce docker-ce-cli containerd.io

curl -fsSLO https://gist.github.com/goll/bdd6b43c2023f82d15729e9b0067de60/raw/nftables-docker.sh

sudo bash -x nftables-docker.sh

sudo systemctl enable docker --now

docker volume create portainer_data

docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v portainer_data:/data \
    portainer/portainer-ce:latest

```

# Snapcraft

**Install:**

```shell
sudo apt update
sudo apt install snapd
systemctl enable --now snapd apparmor
```

# VSCODE

**Install:**

```shell
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
rm -f packages.microsoft.gpg

sudo apt install apt-transport-https
sudo apt update
sudo apt install code # or code-insiders
```

# Zsh

**Install:**

```shell
sudo apt install -y zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)
sh -c "$(curl -fsSL https://raw.githubusercontent.com/zdharma/zinit/master/doc/install.sh)"
sh -c "$(curl -fsSL https://git.io/zinit-install)"

mkdir meslo

curl -L https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf > "meslo/MesloLGS NF Regular.ttf"
curl -L https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf > "meslo/MesloLGS NF Bold.ttf"
curl -L https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf > "meslo/MesloLGS NF Italic.ttf"
curl -L https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf > "meslo/MesloLGS NF Bold Italic.ttf"

sudo mkdir /usr/share/fonts/truetype/fonts-meslolgs
sudo -A mv meslo/*.ttf /usr/share/fonts/truetype/fonts-meslolgs
sudo fc-cache -f /usr/share/fonts/truetype

rm -rf meslo
```

```shell

# Plugin history-search-multi-word loaded with investigating.
zinit load zdharma-continuum/history-search-multi-word

# Two regular plugins loaded without investigating.git
zinit light zsh-users/zsh-autosuggestions
zinit light zdharma-continuum/fast-syntax-highlighting

# Load powerlevel10k theme
zinit ice depth"1" # git clone depth
zinit light romkatv/powerlevel10k

```

# Bugs

sudo nano etc/lightdm/lightdm.conf
[LightDM]
logind-check-graphical=true

optional

cp /etc/X11/xorg.conf /etc/X11/xorg_backup.conf
sudo Xorg -configure
cp /root/xorg.conf.new /etc/X11/xorg.conf
startx


