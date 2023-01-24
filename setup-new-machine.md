# Setting Up a New Dev Machine
###### tags: `reference`

Sample configuration files [available](https://github.com/santisbon/reference/tree/main/dot-files).

[TOC]

# Do a clean install of the OS

On macOS download the new version from System Preferences, Software Update (or the Mac App Store) and create the bootable media with:
```shell
sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume/
```
using the volume that matches the name of the external drive you are using.  

If you have an Intel-powered Mac here's how to install macOS from a bootable installer:
1. Plug in your bootable media.
2. Turn off your Mac.
3. Press and hold Option/Alt while the Mac starts up - keep pressing the key until you see a screen showing the bootable volume.

If you have an Apple silicon Mac here's how to install macOS from a bootable installer:
1. Plug in your bootable media.
2. Turn off your Mac.
3. Press the power button to turn on the Mac - but keep it pressed until you see the startup options window including your bootable volume.  

Open Finder and show hidden files with Command + Shift + . (period) or with
```shell
defaults write com.apple.finder AppleShowAllFiles TRUE; killall Finder
```


# Package manager 
## macOS
Get [Homebrew](https://brew.sh/), a package manager for macOS.
```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew doctor
```
You can use a more [detailed guide](https://mac.install.guide/homebrew/index.html) if needed.
## Linux
Depending on your distribution, you may have apt (Ubuntu), yum (Red Hat), or zypper (openSUSE).



# Shell
## Bash
If you want to use bash
```shell
brew install bash # get the latest version of bash
chsh -s $(which bash)
nano ~/.bash_profile # and paste from sample dot file
```

## Zsh

```zsh
brew install zsh # get the latest version

# You may need to add the Homebrew version of Zsh to the list on /etc/shells
sudo nano /etc/shells # add the output of "which zsh" to the top of the list, save the file.

where zsh # you might see both the shell that came with your Mac and the latest from Homebrew
which zsh # make sure this returns the one from Homebrew.

chsh -s $(which zsh)
# Install Oh My Zsh, a framework for managing your Zsh configuration with plugins and themes
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

[Oh My Zsh](https://ohmyz.sh/)  
Add any built-in [plugins](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins) you need to your ```~/.zshrc```
```shell
plugins=(git macos python)
```

You can also add custom plugins by cloning the repo into the plugins directory or with Homebrew. 

This plugin auto suggests previous commands. 
```shell
git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
# add zsh-autosuggestions to plugins list in ~/.zshrc and:
source ~/.zshrc
```

This plugin [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) highlights valid commands green and invalid ones red so you don't have to test the command to see if it will work.
```shell
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
Or just use Homebrew:
```shell
brew install zsh-syntax-highlighting
# To activate the syntax highlighting, add the following at the end of your ~/.zshrc:
source $(brew --prefix)/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

Some Oh My Zsh themes like [Spaceship](https://spaceship-prompt.sh/) have requirements like the Powerline Fonts. Get them with:
```shell
git clone https://github.com/powerline/fonts.git --depth=1
./fonts/install.sh
rm -rf fonts
```

You can also install fonts with Homebrew by adding the fonts repository. [Fira Code](https://github.com/tonsky/FiraCode/wiki/VS-Code-Instructions) is a good one for programming and computer science.
```shell
brew tap homebrew/cask-fonts
brew install --cask font-fira-code
```
Install your Oh My Zsh theme e.g. Spaceship
```shell
brew install spaceship
# If the theme is not copied to your themes folder, sim link from the Homebrew dir to your custom themes folder e.g.
ln -sf $(brew --prefix)/Cellar/spaceship/4.4.1/spaceship.zsh $ZSH_CUSTOM/themes/spaceship.zsh-theme

touch ~/.spaceshiprc.zsh
```

Any time you edit your zsh configuration file you can reload it to apply changes.
```shell
source ~/.zshrc
```

## Terminal replacement
You can use [iTerm2](https://iterm2.com/index.html)
```shell
brew install --cask iterm2
```

After you have installed the font(s) required by your Oh My Zsh theme set your iTerm preferences like default shell and font.  
Verify that you're using the shell you want. In the output of the ```env``` command look for something like ```SHELL=/opt/homebrew/bin/zsh```.

Install iTerm 2 color schemes
```shell
git clone https://github.com/mbadolato/iTerm2-Color-Schemes.git
cd iTerm2-Color-Schemes

# Import all color schemes
tools/import-scheme.sh schemes/*
```
Restart iTerm 2 (need to quit iTerm 2 to reload the configuration file).  
iTerm2 > Preferences > Profile > Colors > Color Presets 


# Install utilities
These will vary for each person. Some examples on a Mac:  

```shell
brew install wget
brew install gcc
brew install jq
brew install kompose # translate Docker Compose files to Kubernetes resources
brew install --cask docker
brew install --cask drawio # Diagrams (UML, AWS, etc.)
brew install --cask 1password
brew install --cask nordvpn
brew install --cask google-chrome # or chromium
brew install --cask firefox
brew install --cask visual-studio-code # or vscodium 
brew install --cask kindle
brew install --cask authy
brew install --cask teamviewer
brew install --cask raspberry-pi-imager
brew install --cask balenaetcher # to make bootable media
brew install --cask zoom
brew install --cask spotify
brew install --cask whatsapp
brew install --cask messenger # Facebook messenger
brew install --cask rectangle # to snap windows
brew install --cask avast-security # if it fails, download from website
brew install --cask handbrake # if you need to rip DVDs
brew install --cask virtualbox # currently Intel Macs only. ARM installer in beta.
brew install --cask multipass # run Ubuntu VMs in a local mini-cloud.
brew install --cask steam # if you want games
brew tap homebrew/cask-drivers # add repository of drivers
brew install logitech-options # driver for Logitech mouse
```

On macOS you can also install gcc by installing the xcode developer tools if you're not using Homebrew.
```shell
xcode-select -v # check if tools are installed
xcode-select --install
```

# Code Editor (VSCode/VSCodium)

Q. VSCodium cannot be opened because Apple cannot check it for malicious software
A. You have to "right click" the .app and then hold the alt/option key, while clicking "Open" (only on first launch).

Useful extensions:
- Better TOML
- Compare Folders
- Draw.io Integration

# Git
## Install

macOS  
```shell
brew install git
git --version

# GitHub CLI
brew install gh 
gh auth login
gh config set editor "codium -w" # or code, nano, etc
```
GitHub CLI [reference](https://docs.github.com/en/github-cli/github-cli/github-cli-reference).

Linux
```shell
sudo yum install git # or git-all
git --version
```

## Configure
```shell
git config --global core.editor 'codium --wait'

git config --global diff.tool codium
git config --global difftool.codium.cmd 'codium --wait --diff $LOCAL $REMOTE'

git config --global merge.tool codium
git config --global mergetool.codium.cmd 'codium --wait $MERGED'

# or
codium ~/.gitconfig # and paste from sample dot file. Also: codium, vscode, nano
```

Then you can ```git difftool main feature-branch```.  

If using AWS CodeCommit do this after configuring the AWS CLI:
```shell
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true
```
Troubleshooting CodeCommit  
https://docs.aws.amazon.com/codecommit/latest/userguide/troubleshooting-ch.html#troubleshooting-macoshttps

To create a squash function:
```shell
git config --global alias.squash-all '!f(){ git reset $(git commit-tree "HEAD^{tree}" "$@");};f'
```
:::info
:information_source: 
- Git allows you to escape to a shell (like bash or zsh) using the ! (bang). [Learn more](https://www.atlassian.com/blog/git/advanced-git-aliases).
- [`commit-tree`](https://git-scm.com/docs/git-commit-tree) creates a new commit object based on the provided tree object and emits the new commit object id on stdout.
- [`tree` objects](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects#Tree-Objects) correspond to UNIX directory entries. 
- `reset` resets current HEAD to the specified state e.g a commit.
- The `master^{tree}` syntax specifies the tree object that is pointed to by the last commit on your `master` branch. So `HEAD^{tree}` is the tree object pointed to by the last commit on your current branch.
- If you’re using ZSH, the `^` character is used for globbing, so you have to enclose the whole expression in quotes: `"HEAD^{tree}"`
:::
Then just run:
```shell
git squash-all -m "a brand new start"
git push -f
```

## Set up git autocompletion (bash)

```bash
cd ~
curl -OL https://github.com/git/git/raw/master/contrib/completion/git-completion.bash
mv ~/git-completion.bash ~/.git-completion.bash
```

## Set up SSH key
1. [Check](https://help.github.com/articles/checking-for-existing-ssh-keys/) for existing keys.
2. [Generate](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) a new SSH key and add it to ssh-agent. You may need to set permissions to your key file with ```chmod 600```.
3. [Add](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/) the public key to your GitHub account.

If the terminal is no longer authenticating you:
```shell
git remote -v # is it https or ssh? Should be ssh
git remote remove origin
git remote add origin git@github.com:user/repo.git
```
Still having issues? Start the ssh-agent in the background and add your SSH private key to the ssh-agent.
```shell
ps -ax | grep ssh-agent
eval "$(ssh-agent -s)"
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```
If you need to add your public key to Github again copy and paste it on your Settings page on Github:
```shell
pbcopy < ~/.ssh/id_ed25519.pub
```

# Docker
## Install
### macOS
```shell
brew install --cask docker
```
* You must use the --cask version. Otherwise only the client is included and can't run the Docker daemon. Then open the Docker app and grant privileged access when asked. Only then will you be able to use docker.

### Linux / Raspberry Pi OS:
If you just ran `apt upgrade` on your Raspberry Pi, reboot before installing Docker.  
Follow the appropriate [installation method](https://docs.docker.com/engine/install/debian/#install-using-the-convenience-script).  

If you don't want to have to prefix commands with sudo add your user to the `docker` group. This is equivalent to giving that user root privileges.
```shell
cat /etc/group | grep docker # see if docker group exists
sudo usermod -aG docker $USER
```

Follow [Post-installation steps](https://docs.docker.com/engine/install/linux-postinstall/) to configure log rotation.  

## Use
### Architecture

[Supported Architectures](https://github.com/docker-library/official-images#architectures-other-than-amd64)  
[Platform specifiers](https://github.com/containerd/containerd/blob/v1.4.3/platforms/platforms.go#L63)  

|  Value  | Normalized |          Examples           |
| :-----: | :--------: | :-------------------------: |
| aarch64 | arm64      | Apple M1/M2, Raspberry Pi 4 |
| armhf   | arm        | Raspberry Pi 2              |
| armel   | arm/v6     |                             |
| i386    | 386        |                             |
| x86_64  | amd64      | Intel (Default)             |
| x86-64  | amd64      | Intel (Default)             |

Docker Desktop for Apple silicon can run (or build) an Intel image using emulation.  
The ```--platform``` option sets the platform if server is multi-platform capable.  

Macbook **M1/M2** is based on **ARM**. The Docker host is detected as **linux/arm64/v8** by Docker.  

On a Macbook M1/M2 running a native arm64 linux image instead of an amd64 (x86-64 Intel) image with emulation:  
When installing apps on the container either install the package for the ARM platform if they provide one, or run an amd64 docker image (Docker will use emulation). 

See Docker host info.  
You need to use the real field names (not display names) so we'll grab the info in json to get the real field names and use ```jq``` to display it nicely. Then we'll render them using Go templates.

```shell
docker info --format '{{json .}}' | jq .

docker info --format "{{.Plugins.Volume}}"
docker info --format "{{.OSType}} - {{.Architecture}} CPUs: {{.NCPU}}"
```

With the default images, Docker Desktop for Apple silicon warns us the requested image's platform (linux/amd64) is different from the detected host platform (linux/arm64/v8) and no specific platform was requested.  
```--platform linux/amd64``` is the default and the same as  ```--platform linux/x86_64```. It runs (or builds) an Intel image.  
```--platform linux/arm64``` runs (or builds) an aarch64 (arm64) image. Architecture used by Appple M1/M2.  

```shell
docker run -it --name container1 debian # WARNING. uname -m -> x86_64 (amd64)
docker run -it --platform linux/amd64 --name container1 debian # NO warning. uname -m -> x86_64 (amd64). On Mac it emulates Intel.
docker run -it --platform linux/arm64 --name container1 debian # NO warning. uname -m -> aarch64 (arm64). Mac native.

# On a Mac M2 you can also use this image
docker run -it --name mycontainer arm64v8/debian bash # NO warning. uname -m -> aarch64 (arm64). Mac native.
```

### Examples

Dockerfile instruction to keep a container running
```shell
CMD tail -f /dev/null
```

Running containers
```shell
# sanity check
docker version
docker --help

# "docker run" = "docker container run"

docker run --name container1 debian # created and Exited
docker run -it --name container2 debian # created and Up. Interactive tty
docker run --detach --name container3 debian # created and Exited
docker run -it --detach --name container4 debian # created and Up. Running in background

docker stop container4 # Exited
docker restart container4 # Up
docker attach container4 # Interactive tty

# To have it removed automatically when it's stopped 
docker run -it --rm --name mycontainer debian # created and Up. Interactive tty. 
# Run bash in a new container with interactive tty, based on debian image.
docker run -it --name mycontainer debian bash
# Get the IDs of all stopped containers. Options: all, quiet (only IDs), filter.
docker ps -aq -f status=exited

```

Working with images and volumes
```shell

# Show all images (including intermediate images)
docker image ls -a

# Build an image from a Dockerfile and give it a name (or name:tag).
docker build -f mydockerfile -t santisbon/myimage .
docker builder prune -a		# Remove all unused build cache, not just dangling ones
docker image rm santisbon/myimage

# Volumes
docker volume create my-vol
docker volume ls
docker volume inspect my-vol
docker volume prune
docker volume rm my-vol

# Start a container from an image.
docker run \
--name mycontainer \
--hostname mycontainer \
--mount source=my-vol,target=/data \
santisbon/myimage

# Get rid of all stopped containers. Options: remove **anonymous volumes** associated with the container.
docker rm -v $(docker ps -aq -f status=exited)
# You might want to make an alias:
alias drm='docker rm -v $(docker ps -aq -f status=exited)'
# or
alias drm='sudo docker rm -v $(sudo docker ps -aq -f status=exited)'

# on macOS the volume mountpoint (/var/lib/docker/volumes/) is in the VM that Docker Desktop uses to provide the Linux environment.
# You can read it through a container given extended privileges. 
# Use: 
# - the "host" PID namespace, 
# - an image e.g. debian, 
# - the nsenter command to run a program (sh) in a different namespace
# - the target process with PID 1
# - enter following namspaces of the target process: mount, UTS, network, IPC.
docker run -it --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh
```

If you want to build [multi-platform](https://docs.docker.com/build/building/multi-platform/#getting-started) docker images:
```shell
docker buildx create --name mybuilder --driver docker-container --bootstrap
docker buildx use mybuilder

docker buildx inspect
docker buildx ls
```

Docker Compose. Multi-container deployments in a compose.yaml file.  
[Using Compose in Production](https://docs.docker.com/compose/production/)  

```shell
docker compose -p mastodon-bot-project up --detach
# or
docker compose -p mastodon-bot-project create
docker compose -p mastodon-bot-project start

# connect to a running container
docker exec -it bot-app bash

docker compose -p mastodon-bot-project down
# or
docker compose -p mastodon-bot-project stop
```

Kubernetes

```shell
# Convert the Docker Compose file to k8s files
kompose --file compose.yaml convert

# Make sure the container image is available in a repository. 
# You can build it with ```docker build``` or ```docker compose create``` and push it to a public repository.
docker image push user/repo
# multiple -f filenames or a folder 
kubectl apply -f ./k8s
kubectl get pods
kubectl delete -f ./k8s
```



# Kubernetes
## Kubernetes Essentials
[Interactive Diagram](https://lucid.app/lucidchart/6d5625be-9ef9-411d-8bea-888de55db5cf/view?page=0_0#)  
[Working with k8s objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)  
[Log rotation](https://kubernetes.io/docs/concepts/cluster-administration/logging/#log-rotation)  

In order for Kubernetes to pull your container image you need to first push it to an image repository like Docker Hub.  
To avoid storing your Docker Hub password unencrypted in $HOME/.docker/config.json when you `docker login` to your account, use a [credentials store](https://docs.docker.com/engine/reference/commandline/login/#credentials-store). A helper program lets you interact with such a keychain or external store. 

If you're doing this on your laptop with **Docker Desktop** it **already provides a store**.  
**Otherwise**, **use** one of the stores supported by the [**`docker-credential-helper`**](https://docs.docker.com/engine/reference/commandline/login/#credential-helpers). Now `docker login` on your terminal or on the Docker Desktop app.  


## Designing Applications for Kubernetes
Based on the [12-Factor App Design Methodology](https://12factor.net/)

A Cloud Guru course. It uses Ubuntu 20.04 Focal Fossa LTS and the Calico network plugin instead of Flannel.  
Example with 1 control plane node and 2 worker nodes.

### Building a Kubernetes Cluster

[Reference](https://acloudguru-content-attachment-production.s3-accelerate.amazonaws.com/1658326918182-Building%20a%20Kubernetes%20Cluster.pdf)  
[Installing kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)  
[Creating a cluster with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)  
* kubeadm sometimes doesn't work with the latest and greatest version of docker right away.

`kubeadm` simplifies the process of setting up a k8s cluster.  
`containerd` manages the complete container lifecycle of its host system, from image transfer and storage to container execution and supervision to low-level storage to network attachments.  
`kubelet` handles running containers on a node.  
`kubectl` is a tool for managing the cluster.  

If you wish, you can set an appropriate hostname for each node.  
**On the control plane node**:  
```shell
sudo hostnamectl set-hostname k8s-control
```

**On the first worker node**:  
```shell
sudo hostnamectl set-hostname k8s-worker1
```
**On the second worker node**:  
```shell
sudo hostnamectl set-hostname k8s-worker2
```

**On all nodes**, set up the hosts file to enable all the nodes to reach each other using these hostnames.
```shell
sudo nano /etc/hosts
```

**On all nodes**, add the following at the end of the file. You will need to supply the actual private IP address for each node.
```shell
<control plane node private IP> k8s-control
<worker node 1 private IP> k8s-worker1
<worker node 2 private IP> k8s-worker2
```

Log out of all three servers and log back in to see these changes take effect.  

**On all nodes, set up containerd**. You will need to load some kernel modules and modify some system settings as part of this
process.  
```shell
# Enable them when the server start up
cat << EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF

# Enable them right now without having to restart the server
sudo modprobe overlay
sudo modprobe br_netfilter

# Add network configurations k8s will need
cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

# Apply them immediately
sudo sysctl --system
```

Install and configure containerd.  
```shell
sudo apt-get update && sudo apt-get install -y containerd
sudo mkdir -p /etc/containerd
# Generate the contents of a default config file and save it
sudo containerd config default | sudo tee /etc/containerd/config.toml
# Restart containerd to make sure it's using that configuration
sudo systemctl restart containerd
```

**On all nodes, disable swap**.
```shell
sudo swapoff -a
```
**On all nodes, install kubeadm, kubelet, and kubectl**.
```shell
# Some required packages
sudo apt-get update && sudo apt-get install -y apt-transport-https curl
# Set up the package repo for k8s packages. Download the key for the repo and add it
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
# Configure the repo
cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
sudo apt-get update
# Install the k8s packages. Make sure the versions for all 3 are the same.
sudo apt-get install -y kubelet=1.24.0-00 kubeadm=1.24.0-00 kubectl=1.24.0-00
# Make sure they're not automatically upgraded. Have manual control over when to update k8s
sudo apt-mark hold kubelet kubeadm kubectl
```

**On the control plane node only**, initialize the cluster and set up kubectl access.
```shell
sudo kubeadm init --pod-network-cidr 192.168.0.0/16 --kubernetes-version 1.24.0
# Config File to authenticate and interact with the cluster with kubectl commands
# These are in the output of the previous step
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Verify the cluster is working. It will be in Not Ready status because we haven't configured the networking plugin.
```shell
kubectl get nodes
```

Install the Calico network add-on.
```shell
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

Get the join command (this command is also printed during kubeadm init . Feel free to simply copy it from there).
```shell
kubeadm token create --print-join-command
```

Copy the join command from the control plane node. Run it **on each worker node** as root (i.e. with sudo ).
```shell
sudo kubeadm join ...
```

**On the control plane node**, verify all nodes in your cluster are ready. Note that it may take a few moments for all of the nodes to
enter the READY state.
```shell
kubectl get nodes
```

### Installing Docker

[Reference](https://acloudguru-content-attachment-production.s3-accelerate.amazonaws.com/1631214923454-1082%20-%20S01L03%20Installing%20Docker.pdf)  
[Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)  
[Docker credentials store](https://docs.docker.com/engine/reference/commandline/login/#credentials-store) 
to avoid storing your Docker Hub password unencrypted in `$HOME/.docker/config.json` when you `docker login` and `docker push` your images.  

**On the system that will build Docker images from source code** e.g. a CI server, install and configure Docker.  
For simplicity we'll use the control plane server just so we don't have to create another server for this exercise.  

Create a docker group. Users in this group will have permission to use Docker on the system:
```shell
sudo groupadd docker
```

Install required packages.  
Note: Some of these packages may already be present on the system, but including them here will not cause any problems:
```shell
sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release
```

Set up the Docker GPG key and package repository:
```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Install the Docker Engine:
```shell
sudo apt-get update && sudo apt-get install -y docker-ce docker-ce-cli
# Type N (default) or enter to keep your current containerd configuration
```

Test the Docker setup:
```shell
sudo docker version
```

Add cloud_user to the docker group in order to give cloud_user access to use Docker:
```shell
sudo usermod -aG docker cloud_user
```
Log out of the server and log back in.  
Test your setup:
```shell
docker version
```

### Kubernetes configuration with ConfigMaps and Secrets
III. Config
Store config in the environment

[Reference](https://acloudguru-content-attachment-production.s3-accelerate.amazonaws.com/1631214961641-1082%20-%20S02L03%20III.%20Config%20with%20ConfigMaps%20and%20Secrets.pdf)  

[Encrypting Secret Data at Rest](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)  
[ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/)  
[Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)  

Create a production Namespace:
```shell
kubectl create namespace production
```

Get base64-encoded strings for a db username and password:
```shell
echo -n my_user | base64
echo -n my_password | base64
```

Example: Create a ConfigMap and Secret to configure the backing service connection information for the app, including the base64-encoded credentials:
```shell
cat > my-app-config.yml <<End-of-message 
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-app-config
data:
  mongodb.host: "my-app-mongodb"
  mongodb.port: "27017"

---

apiVersion: v1
kind: Secret
metadata:
  name: my-app-secure-config
type: Opaque
data:
  mongodb.username: dWxvZV91c2Vy
  mongodb.password: SUxvdmVUaGVMaXN0

End-of-message
```

```shell
kubectl apply -f my-app-config.yml -n production
```

Create a temporary Pod to test the configuration setup. Note that you need to supply your Docker Hub username as part of the image name in this file.
This passes configuration data in env variables but you could also do it in files that will show up on the containers filesystem.
```shell
cat > test-pod.yml <<End-of-message

apiVersion: v1
kind: Pod
metadata:
  name: test-pod
spec:
  containers:
  - name: my-app-server
    image: <YOUR_DOCKER_HUB_USERNAME>/my-app-server:0.0.1
    ports:
    - name: web
      containerPort: 3001
      protocol: TCP
    env:
    - name: MONGODB_HOST
      valueFrom:
        configMapKeyRef:
          name: my-app-config
          key: mongodb.host
    - name: MONGODB_PORT
      valueFrom:
        configMapKeyRef:
          name: my-app-config
          key: mongodb.port
    - name: MONGODB_USER
      valueFrom:
        secretKeyRef:
          name: my-app-secure-config
          key: mongodb.username
    - name: MONGODB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: my-app-secure-config
          key: mongodb.password

End-of-message
```

```shell
kubectl apply -f test-pod.yml -n production
```

Check the logs to verify the config data is being passed to the container:
```shell
kubectl logs test-pod -n production
```

Clean up the test pod:
```shell
kubectl delete pod test-pod -n production --force
```

### Build, Release, Run with Docker and Deployments
V. Build, release, run
Strictly separate build and run stages

[Reference](https://acloudguru-content-attachment-production.s3-accelerate.amazonaws.com/1631215185856-1082%20-%20S03L02%20V.%20Build%2C%20Release%2C%20Run%20with%20Docker%20and%20Deployments.pdf)  
[Labels and Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)  
[Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)  

Example: After you `docker build` and `docker push` your image to a repository, create a deployment file for your app.
The `selector` selects pods that have the specified label name and value.  
`template` is the pod template.  
This example puts 2 containers in the same pod for simplicity but in the real world you'll want separate deployments to scale them independently.

```shell
cat > my-app.yml <<End-of-message
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-app-config
data:
  mongodb.host: "my-app-mongodb"
  mongodb.port: "27017"
  .env: |
    REACT_APP_API_PORT="30081"

---

apiVersion: v1
kind: Secret
metadata:
  name: my-app-secure-config
type: Opaque
data:
  mongodb.username: dWxvZV91c2Vy
  mongodb.password: SUxvdmVUaGVMaXN0

---

apiVersion: v1
kind: Service
metadata:
  name: my-app-svc
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
  - name: frontend
    protocol: TCP
    port: 30080
    nodePort: 30080
    targetPort: 5000
  - name: server
    protocol: TCP
    port: 30081
    nodePort: 30081
    targetPort: 3001

---

apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app-server
        image: <Your Docker Hub username>/my-app-server:0.0.1
        ports:
        - name: web
          containerPort: 3001
          protocol: TCP
        env:
        - name: MONGODB_HOST
          valueFrom:
            configMapKeyRef:
              name: my-app-config
              key: mongodb.host
        - name: MONGODB_PORT
          valueFrom:
            configMapKeyRef:
              name: my-app-config
              key: mongodb.port
        - name: MONGODB_USER
          valueFrom:
            secretKeyRef:
              name: my-app-secure-config
              key: mongodb.username
        - name: MONGODB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: my-app-secure-config
              key: mongodb.password
      - name: my-app-frontend
        image: <Your Docker Hub username>/my-app-frontend:0.0.1
        ports:
        - name: web
          containerPort: 5000
          protocol: TCP
        volumeMounts:
        - name: frontend-config
          mountPath: /usr/src/app/.env
          subPath: .env
          readOnly: true
      volumes:
      - name: frontend-config
        configMap:
          name: my-app-config
          items:
          - key: .env
            path: .env
End-of-message
```

Deploy the app.
```shell
kubectl apply -f my-app.yml -n production
```

Create a new container image version to test the rollout process:
```shell
docker tag <Your Docker Hub username>/my-app-frontend:0.0.1 <Your Docker Hub username>/my-app-frontend:0.0.2
docker push <Your Docker Hub username>/my-app-frontend:0.0.2
```

Edit the app manifest `my-app.yml` to use the `0.0.2` image version and then:
```shell
kubectl apply -f my-app.yml -n production
```

Get the list of Pods to see the new version rollout:
```shell
kubectl get pods -n production
```

### Processes with stateless containers
VI. Processes
Execute the app as one or more stateless processes

[Reference](https://acloudguru-content-attachment-production.s3-accelerate.amazonaws.com/1632153847308-1082%20-%20S03L03%20VI.%20Processes%20with%20Stateless%20Containers.pdf)  
[Security Context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)  

Edit the app deployment YAML `my-app.yml`. In the deployment Pod spec, add a new emptyDir volume under volumes:
```yaml
volumes:
- name: added-items-log
  emptyDir: {}
...
```

Mount the volume to the server container:
```yaml
containers:
...
- name: my-app-server
  ...
  volumeMounts:
  - name: added-items-log
    mountPath: /usr/src/app/added_items.log
    subPath: added_items.log
    readOnly: false
  ...
```

Make the container file system read only:
```yaml
containers:
...
- name: my-app-server
  securityContext:
    readOnlyRootFilesystem: true
  ...
```

Deploy the changes:
```shell
kubectl apply -f my-app.yml -n production
```

### Persistent Volumes
[Reference](https://acloudguru-content-attachment-production.s3-accelerate.amazonaws.com/1604349952306-devops-wb002%20-%20S10-L04%20Using%20K8s%20Persistent%20Volumes.pdf)  
[Persistent Volumes (PV)](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)  
[`local` PV](https://kubernetes.io/docs/concepts/storage/volumes/#local)  

Create a `StorageClass` that supports volume expansion as `localdisk-sc.yml`
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: localdisk
provisioner: kubernetes.io/no-provisioner
allowVolumeExpansion: true
```
```shell
kubectl create -f localdisk-sc.yml
```

`persistentVolumeReclaimPolicy` says how storage can be reused when the volume's associated claims are deleted.  
- Retain: Keeps all data. An admin must manually clean up and prepare the resource for reuse.
- Recycle: Automatically deletes all data, allowing  the volume to be reused.
- Delete: Deletes underlying storage resource automatically (applies to cloud only).  

[`accessModes`](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes) can be:
- ReadWriteOnce: The volume can be mounted as read-write by a single node. Still can allow multiple pods to access the volume when the pods are running on the same node.  
- ReadOnlyMany: Can be mounted as read-only by many nodes.
- ReadWriteMany: Can be mounted as read-write by many nodes.
- ReadWriteOncePod: Can be mounted as read-write by a single Pod. Use ReadWriteOncePod access mode if you want to ensure that only one pod across whole cluster can read that PVC or write to it. This is only supported for CSI volumes.

Create a PersistentVolume in `my-pv.yml`.
```yaml
kind: PersistentVolume
apiVersion: v1
metadata:
  name: my-pv
spec:
  storageClassName: localdisk
  persistentVolumeReclaimPolicy: Recycle
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /var/output
```
```shell
kubectl create -f my-pv.yml
```

Check the status of the PersistentVolume.
```shell
kubectl get pv
```

Create a PersistentVolumeClaim that will bind to the PersistentVolume as `my-pvc.yml`
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  storageClassName: localdisk
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 100Mi
```
```shell
kubectl create -f my-pvc.yml
```

Check the status of the PersistentVolume and PersistentVolumeClaim to verify that they have been bound.
```shell
kubectl get pv
kubectl get pvc
```

Create a Pod that uses the PersistentVolumeClaim as `pv-pod.yml`.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pv-pod
spec:
  restartPolicy: Never
  containers:
  - name: busybox
    image: busybox
    command: ['sh', '-c', 'echo Success! > /output/success.txt']
    volumeMounts:
    - name: pv-storage
      mountPath: /output
  volumes:
  - name: pv-storage
    persistentVolumeClaim:
      claimName: my-pvc
```
```shell
kubectl create -f pv-pod.yml
```

Expand the PersistentVolumeClaim and record the process.
```shell
kubectl edit pvc my-pvc --record
```
```yaml
...
spec:
...
  resources:
    requests:
      storage: 200Mi
```

Delete the Pod and the PersistentVolumeClaim.
```shell
kubectl delete pod pv-pod
kubectl delete pvc my-pvc
```

Check the status of the PersistentVolume to verify that it has been successfully recycled and is available again.
```shell
kubectl get pv
```

### Port Binding with Pods
VII. Port binding
Export services via port binding

[Reference](https://acloudguru-content-attachment-production.s3-accelerate.amazonaws.com/1631215167150-1082%20-%20S03L04%20VII.%20Port%20Binding%20with%20Pods.pdf)  
[Cluster Networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/)  

Challenge: only 1 process can listen on a port per host. So how do all apps on the host use a unique port?  
In k8s, each pod has its own network namespace and cluster IP address.  
That IP address is unique within the cluster even if there are multiple worker nodes in the cluster.  
Tht means ports only need to be unique within each pod.  
2 pods can listen on the same port because they each have their own unique IP address within the cluster network.  
The pods can communicate across nodes simply using the unique IPs.

Get a list of Pods in the production namespace:
```shell
kubectl get pods -n production -o wide
```

Copy the name of the IP address of the application Pod.  
Example: Use the IP address to make a request to the port on the Pod that serves the frontend content:
```shell
curl <Pod Cluster IP address>:5000
```

### Concurrency with Containers and Scaling
VIII. Concurrency
Scale out via the process model

[Reference](https://acloudguru-content-attachment-production.s3-accelerate.amazonaws.com/1631215269199-1082%20-%20S04L01%20VIII.%20Concurrency%20with%20Containers%20and%20Scaling.pdf)  
[Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)  

By using `services` to manage access to the app, the service automaticaly picks up the additional pods created during scaling and route traffic to those pods.
When you have `services` alongside `deployments` you can dynamically change the number of replicas that you have and k8s will take care of everything.

Edit the application deployment `my-app.yml`.  
Change the number of replicas to 3:
```yaml
...
replicas: 3
```

Apply the changes:
```shell
kubectl apply -f my-app.yml -n production
```

Get a list of Pods:
```shell
kubectl get pods -n production
```

Scale the deployment up again in `my-app.yml`.  
Change the number of replicas to 5:
```yaml
...
replicas: 5
```

Apply the changes:
```shell
kubectl apply -f my-app .yml -n production
```

Get a list of Pods:
```shell
kubectl get pods -n production
```

### Disposability with Stateless Containers
IX. Disposability
Maximize robustness with fast startup and graceful shutdown

[Reference](https://acloudguru-content-attachment-production.s3-accelerate.amazonaws.com/1631215259805-1082%20-%20S04L02%20IX.%20Disposability%20with%20Stateless%20Containers.pdf)  
[Security Context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)  
[Pod Lifecycle](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/)  

Deployments can be used to maintain a specified number of running replicas automatically replacing pods that fail or are deleted.

Get a list of Pods:
```shell
kubectl get pods -n production
```

Locate one of the Pods from the my-app deployment and copy the Pod's name.  
Delete the Pod using the Pod name:
```shell
kubectl delete pod <Pod name> -n production
```

Get the list of Pods again. You will notice that the deployment is automatically creating a new Pod to replace the one that was deleted:
```shell
kubectl get pods -n production
```

### Dev/Prod Parity with Namespaces
X. Dev/prod parity
Keep development, staging, and production as similar as possible

[Reference](https://acloudguru-content-attachment-production.s3-accelerate.amazonaws.com/1631215371092-1082%20-%20S05L01%20X.%20Dev%3AProd%20Parity%20with%20Namespaces.pdf)
[Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)  

k8s namespaces allow us to have multiple environments in the same cluster. A namespace is like a virtual cluster.

Create a new namespace:
```shell
kubectl create namespace dev
```

Make a copy of the my-app app YAML:
```shell
cp my-app.yml my-app-dev.yml
```

`NodePort` `services` need to be unique within the cluster. We need to choose unique ports so dev doesn't conflict with production.  
Edit the my-app-svc service in the `my-app-dev.yml` file to select different `nodePort`s. You will also need to edit the my-app-config ConfigMap to reflect the new port. 
Set the nodePorts on the service:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app-svc
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
  - name: frontend
    protocol: TCP
    port: 30082
    nodePort: 30082
    targetPort: 5000
  - name: server
    protocol: TCP
    port: 30083
    nodePort: 30083
    targetPort: 3001
```

Update the configured port in the ConfigMap:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-app-config
data:
  mongodb.host: "my-app-mongodb"
  mongodb.port: "27017"
  .env: |
    REACT_APP_API_PORT="30083"
```

Deploy the backing service setup in the new namespace:
```shell
kubectl apply -f k8s-my-app-mongodb.yml -n dev
kubectl apply -f my-app-mongodb.yml -n dev
```

Deploy the app in the new namespace:
```shell
kubectl apply -f my-app-dev.yml -n dev
```
Check the status of the Pods:
```shell
kubectl get pods -n dev
```

Once all the Pods are up and running, you should be able to test the dev environment in a browser at  
```<Control Plane Node Public IP>:30082```.

### Logs with k8s Container Logging
XI. Logs
Treat logs as event streams

[Reference](https://acloudguru-content-attachment-production.s3-accelerate.amazonaws.com/1631215382492-1082%20-%20S05L02%20XI.%20Logs%20with%20k8s%20Container%20Logging.pdf)  
[Logging Architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)  
[Kubectl cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/#interacting-with-running-pods)  

k8s captures log data written to stdout by containers. We can use the k8s API, `kubectl logs` or external tools to interact with container logs.  

Edit the source code for the server e.g. `src/server/index.js`. There is a log function that writes to a file. Change this function to simply write log data to the console:  
```javascript
log = function(data) {
console.log(data);
}
```

Build a new server image because we changed the source code:
```shell
docker build -t <Your Docker Hub username>/my-app-server:0.0.4 --target server .
```

Push the image:
```shell
docker push <Your Docker Hub username>/my-app-server:0.0.4
```

Deploy the new code. Edit `my-app.yml`. Change the image version for the server to the new image:
```yaml
containers:
- name: my-app-server
  image: <Your Docker Hub username>/my-app-server:0.0.4
```

```shell
kubectl apply -f my-app.yml -n production
```

Get a list of Pods:
```shell
kubectl get pods -n production
```

Copy the name of one of the my-app deployment Pods and view its logs specifying the pod, namespace, and container.
```shell
kubectl logs <Pod name> -n production -c my-app-server
```

### Admin Processes with Jobs
XII. Admin processes
Run admin/management tasks as one-off processes

[Reference](https://acloudguru-content-attachment-production.s3-accelerate.amazonaws.com/1631215407613-1082%20-%20S05L03%20XII.%20Admin%20Processes%20with%20Jobs.pdf)  
[Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job/)  

A `Job` e.g. a database migration runs a container until its execution completes. Jobs handle re-trying execution if it fails.  
Jobs have a `restartPolicy` of `Never` because once they complete they don't run again. 
This example adds the administrative job to the server image but you could package it into its own image.  

Edit the source code for the admin process in e.g. `src/jobs/deDeuplicateJob.js`.  
Locate the block of code that begins with // Setup MongoDb backing database .  
Change the code to make the database connection configurable:  

```javascript
// Setup MongoDb backing database
const MongoClient = require('mongodb').MongoClient
// MongoDB credentials
const username = encodeURIComponent(process.env.MONGODB_USER || "my-app_user");
const password = encodeURIComponent(process.env.MONGODB_PASSWORD || "ILoveTheList");
// MongoDB connection info
const mongoPort = process.env.MONGODB_PORT || 27017;
const mongoHost = process.env.MONGODB_HOST || 'localhost';
// MongoDB connection string
const mongoURI = `mongodb://${username}:${password}@${mongoHost}:${mongoPort}/my-app`;
const mongoURISanitized = `mongodb://${username}:****@${mongoHost}:${mongoPort}/my-app`;
console.log("MongoDB connection string %s", mongoURISanitized);
const client = new MongoClient(mongoURI);
```

Edit the `Dockerfile` to include the admin job code in the server image.  
Add the following line after the other COPY directives for the server image:
```dockerfile
...
COPY --from=build /usr/src/app/src/jobs .
```

Build and push the server image:
```shell
docker build -t <Your Docker Hub username>/my-app-server:0.0.5 --target server .
docker push <Your Docker Hub username>/my-app-server:0.0.5
```

Create a Kubernetes Job to run the admin job:
Create `de-duplicate-job.yml`.
Supply your Docker Hub username in the image tag:
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: de-duplicate
spec:
  template:
    spec:
      containers:
      - name: my-app-server
        image: <Your Docker Hub username>/my-app-server:0.0.5
        command: ["node", "deDeuplicateJob.js"]
        env:
        - name: MONGODB_HOST
          valueFrom:
            configMapKeyRef:
              name: my-app-config
              key: mongodb.host
        - name: MONGODB_PORT
          valueFrom:
            configMapKeyRef:
              name: my-app-config
              key: mongodb.port
        - name: MONGODB_USER
          valueFrom:
            secretKeyRef:
              name: my-app-secure-config
              key: mongodb.username
        - name: MONGODB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: my-app-secure-config
              key: mongodb.password
      restartPolicy: Never
  backoffLimit: 4
```

Run the Job:
```shell
kubectl apply -f de-duplicate-job.yml -n production
```

Check the Job status:
```shell
kubectl get jobs -n production
```
Get the name of the Job Pod:
```shell
kubectl get pods -n production
```

Use the Pod name to view the logs for the Job Pod:
```shell
kubectl logs <Pod name> -n production
```

## MicroK8s

[On Raspberry Pi](https://microk8s.io/docs/install-raspberry-pi)  
Note: Your boot parameters might be in `/boot/cmdline.txt`. Add these options at the end of the file, then `sudo reboot`.
```
cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1
```

For Raspberry Pi OS [install](https://snapcraft.io/docs/installing-snap-on-raspbian) `snap` first.
```shell
sudo apt update
sudo apt install snapd
sudo reboot
# ...reconnect after reboot
sudo snap install core
```
Then install MicroK8s.
```shell
pi@raspberrypi4:~ $ sudo snap install microk8s --classic
pi@raspberrypi4:~ $ microk8s status --wait-ready
pi@raspberrypi4:~ $ microk8s kubectl get all --all-namespaces
pi@raspberrypi4:~ $ microk8s enable dns dashboard registry hostpath-storage # or any other addons
pi@raspberrypi4:~ $ alias mkctl="microk8s kubectl"
pi@raspberrypi4:~ $ mkctl create deployment nginx --image nginx
pi@raspberrypi4:~ $ mkctl expose deployment nginx --port 80 --target-port 80 --selector app=nginx --type ClustetIP --name nginx
pi@raspberrypi4:~ $ watch microk8s kubectl get all
pi@raspberrypi4:~ $ microk8s reset
pi@raspberrypi4:~ $ microk8s status
pi@raspberrypi4:~ $ microk8s stop # microk8s start
```

You can update a snap package with `sudo snap refresh`.

Configuration file. These are the arguments you can add regarding log rotation `--container-log-max-files` and `--container-log-max-size`. They have default values. [More info](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/).
```shell
cat /var/snap/microk8s/current/args/kubelet
```

### Registry
[Registry doc](https://microk8s.io/docs/registry-built-in)
```shell
microk8s enable registry
```
The containerd daemon used by MicroK8s is configured to trust this insecure registry. To upload images we have to tag them with `localhost:32000/your-image` before pushing them.

### MicroK8s dashboard
If RBAC is not enabled access the dashboard using the token retrieved with:
```shell
microk8s kubectl describe secret -n kube-system microk8s-dashboard-token
```
Use this token in the https login UI of the `kubernetes-dashboard` service.
In an RBAC enabled setup (`microk8s enable rbac`) you need to create a user with restricted permissions as shown [here](https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md).

To access remotely from anywhere:
```shell
microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443 --address 0.0.0.0
```
You can then access the Dashboard with IP or hostname as in https://raspberrypi4.local:10443/


### Troubleshooting
```shell
microk8s inspect
```
MicroK8s might not recognize that cgroup memory is enabled but you can check with `cat /proc/cgroups`.


## Kubernetes Dashboard

[Documentation](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)
```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.6.1/aio/deploy/recommended.yaml
```
```shell
cat << EOF > dashboard-adminuser.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard

---

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
EOF
```

```shell
kubectl apply -f dashboard-adminuser.yaml

kubectl proxy
# Kubectl will make Dashboard available at 
# http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

kubectl -n kubernetes-dashboard create token admin-user
# Now copy the token and paste it into the Enter token field on the login screen. 
```

# Python

[Packaging Python Projects](https://packaging.python.org/en/latest/tutorials/packaging-projects/)  

## Install Python, the pip Python package installer, and Setuptools
### With Homebrew
Even if you already have Python on your OS it might be an outdated version. This installs python and pip.  
macOS
```shell
brew install python
python3 --version
```
The python formulae install pip (as pip3) and Setuptools. Setuptools can be updated via pip3 and pip3 can be used to upgrade itself.
```shell
python3 -m pip install --upgrade setuptools
python3 -m pip install --upgrade pip
```
Add the unversioned symlinks to your ```$PATH``` by adding this to your ```.zshrc``` file.
```shell
export PATH="$(brew --prefix)/opt/python/libexec/bin:$PATH"
```
See the [Homebrew Python](https://docs.brew.sh/Homebrew-and-Python) documentation for details.  

`site-packages` is here. Example for python 3.10 on Homebrew
```shell
ls -al $(brew --prefix)/lib/python3.10/site-packages
```

### Without Homebrew
Linux (Ubuntu)
```shell
sudo apt install python3
```

macOS  
Download .pkg installer from pythong.org. Then get pip.
```shell
curl -o get-pip.py https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py
```

Linux
```shell
sudo apt install python-pip
```

```shell
pip install --upgrade pip setuptools
```
On Ubuntu, upgrading to pip 10 may break pip. If this happens you need to:
```shell
sudo nano /usr/bin/pip
# change "from pip import main" to "from pip._internal import main"
```

## Get the pep8 python style checker
```shell
# On Linux you may need to use sudo.
pip install pep8
```

## Anaconda
For data science and machine learning. [Anaconda](https://docs.anaconda.com/anaconda/) is a distribution of the Python and R programming languages for scientific computing. See [Getting Started](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html).
```shell
brew install --cask anaconda
```

[Installing in silent mode](https://docs.anaconda.com/anaconda/install/silent-mode/)  

Add anaconda to your ```PATH``` on your ```.zshrc``` and ```source ~/.zshrc``` to apply the changes.
```shell
export PATH="$(brew --prefix)/anaconda3/bin:$PATH"
```
To use zsh:
```shell
conda init zsh
```
If ```conda init zsh``` messed up the ```PATH``` in your ```~/.zshrc``` by adding the ```condabin``` directory instead of ```bin``` you can fix it with a symlink:
```shell
ln -sf $(brew --prefix)/anaconda3/bin/jupyter-lab $(brew --prefix)/anaconda3/condabin/jupyter-lab
ln -sf $(brew --prefix)/anaconda3/bin/jupyter $(brew --prefix)/anaconda3/condabin/jupyter
```

If you don't want to activate the ```base``` environment every time you open your terminal:
```shell
conda config --set auto_activate_base false
```

When creating a conda environment you can use optionally use a configuration file. You can also of course, save a config file from an existing environment to back it up.
```shell
conda env create -f myenv.yml
conda activate myenv

conda env list
conda env remove --name ldm
```

### Notebooks
Fresh installs of Anaconda no longer include notebook extensions in their Jupyter installer. This means that the nb_conda libraries need to be added into your environment separately to access conda environments from your Jupyter notebook. Just run these to add them and you should be able to select your environment as a kernel within a Jupyter notebook.
```shell
conda activate <myenv>
conda install ipykernel
conda install nb_conda_kernels # or defaults::nb_conda_kernels
```

Now you can launch the new JupyterLab or the classic Jupyter Notebook.
```shell
jupyter-lab # or the classic "jupyter notebook"
```

# Raspberry Pi
## Setup

1. Write and preconfigure Raspberry Pi OS on the SD card using Raspberry Pi Imager (```brew install --cask raspberry-pi-imager```). Make sure you use the correct version of the OS (32-bit or 64-bit).
   - Change the default password for the ```pi``` user.
   - Enable SSH (password or ssh keys).
   - Configure WiFi if needed.
   - Take note of the hostname.
2. Insert the SD card in your Raspberry Pi and turn it on. 

If you didn't do so during setup, you can still generate and add an ssh key at any time. Example:
```shell
# on your laptop
ssh-keygen
ssh-copy-id -i ~/.ssh/id_rsa pi@raspberrypi4.local
```
To remove password authentication:
```shell
# on the Pi
sudo nano /etc/ssh/sshd_config
```
and replace `#PasswordAuthentication yes` with `PasswordAuthentication no`.
Test the validity of the config file and restart the service (or reboot).
```shell
sudo sshd -t
sudo service sshd restart
sudo service sshd status
```

## Configuration

Find the IP of your Raspberry Pi using its hostname or find all devices on your network with `arp -a`.
```shell
arp raspberrypi4.local 
```
SSH into it with the ```pi``` user and the IP address or hostname. Examples:
```shell
ssh pi@192.168.xxx.xxx
ssh pi@raspberrypi4.local
# You'll be asked for the password if applicable
```

Configure it.
```shell
pi@raspberrypi4:~ $ sudo raspi-config
# Go to Interface Options, VNC (for graphical remote access)
# Tab to the Finish option and reboot.
```
Update it.  
```upgrade``` is used to install available upgrades of all packages currently installed on the system. New packages will be installed if required to satisfy dependencies, but existing packages will never be removed. If an upgrade for a package requires the removal of an installed package the upgrade for this package isn't performed.  
```full-upgrade``` performs the function of upgrade but will remove currently installed packages if this is needed to upgrade the system as a whole.
```shell
pi@raspberrypi4:~ $ sudo apt update # updates the package list
pi@raspberrypi4:~ $ sudo apt full-upgrade
```

## Remote GUI access

Now you'll need a VNC viewer on your laptop to connect to the Raspberry Pi using the graphical interface.
```shell
brew install --cask vnc-viewer
```

Apparently, on Raspberry Pi pip does not download from the python package index (PyPi), it downloads from PiWheels. PiWheels wheels do not come with pygame's dependencies that are bundled in normal releases.

Install Pygame [dependencies](https://www.piwheels.org/project/pygame/) and Pygame.
```shell
pi@raspberrypi4:~ $ sudo apt install libvorbisenc2 libwayland-server0 libxi6 libfluidsynth2 libgbm1 libxkbcommon0 libopus0 libwayland-cursor0 libsndfile1 libwayland-client0 libportmidi0 libvorbis0a libopusfile0 libmpg123-0 libflac8 libxcursor1 libxinerama1 libasyncns0 libxrandr2 libdrm2 libpulse0 libxfixes3 libvorbisfile3 libmodplug1 libxrender1 libsdl2-2.0-0 libxxf86vm1 libwayland-egl1 libsdl2-ttf-2.0-0 libsdl2-image-2.0-0 libjack0 libsdl2-mixer-2.0-0 libinstpatch-1.0-2 libxss1 libogg0
pi@raspberrypi4:~ $ sudo pip3 install pygame
```

Check that the installation worked by running one of its demos
```shell
pi@raspberrypi4:~ $ python3 -m pygame.examples.aliens
```

## Copy files

Copy files between Pi and local machine. On local machine run:
```shell
scp -r pi@raspberrypi2.local:/home/pi/Documents/ ~/Documents/pidocs
```

## Find info about your Pi

32 or 64-bit kernel?
```shell
getconf LONG_BIT
# or check machine's hardware name: armv7l is 32-bit and aarch64 is 64-bit
pi@raspberrypi4:~ $ uname -m
```

See OS version
```shell
cat /etc/os-release
```

Architecture    
If the following returns a ```Tag_ABI_VFP_args``` tag of ```VFP registers```, it's an ```armhf``` (```arm```) system.  
A blank output means ```armel``` (```arm/v6```).
```shell
pi@raspberrypi2:~ $ readelf -A /proc/self/exe | grep Tag_ABI_VFP_args
```
Or check the architecture with:
```shell
hostnamectl
```

You can find info about the hardware like ports, pins, RAM, SoC, connectivity, etc. with:
```shell
pi@raspberrypi4:~ $ pinout
```

## Learn about electronics

I've added some sample code from the [MagPi Essentials book](https://magpi.raspberrypi.com/books/essentials-gpio-zero-v1).  
[Sample code](https://github.com/santisbon/guides/tree/main/raspberrypi)

### GPIO Header

![](https://i.imgur.com/3Zroadt.jpg)


# Amazon Web Services
## AWS CLI

### Install the AWS command line interface
```shell
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

### Check if the AWS CLI is installed correctly
```shell
# you may need to reload your shell's rc file first.
aws --version
```

### Configure the AWS CLI
```shell
aws configure
```

## Alexa Skills Kit (ASK) CLI
https://developer.amazon.com/docs/smapi/set-up-credentials-for-an-amazon-web-services-account.html  
https://developer.amazon.com/docs/smapi/quick-start-alexa-skills-kit-command-line-interface.html

## AWS Amplify
```shell
npm install -g @aws-amplify/cli
amplify configure
```

# MySQL development

**Install MySQL Community Edition **
Use your OS package manager or download .dmg installer for macOS. Take note of the root password.

**Configure your PATH**
Add the mysql location to your PATH. Typically as part of your ~/.bash_profile
```shell
export PATH=/usr/local/mysql/bin:$PATH
```

**Start the MySQL service**
On macOS
```shell
sudo launchctl load -F /Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist
```

**Verify it's running**
On macOS
```shell
sudo launchctl list | grep mysql
```

**Connect to your MySQL instance**
Use MySQL Workbench or other client tool.

**To stop MySQL**
On macOS
```shell
sudo launchctl unload -F /Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist
```

**Exporting data**
If you need to export data you may need to disable or set a security setting. On macOS:

View the variable using your SQL client. If it's NULL you will be restricted regarding file operations.
```sql
show variables like 'secure_file_priv';
```

Open the configuration file.
```shell
cd /Library/LaunchDaemons
sudo nano com.oracle.oss.mysql.mysqld.plist
```
and set the --secure-file-priv to an empty string (to disable the restriction) or a dir of your choice.
```xml
<key>ProgramArguments</key>
    <array>
        <string>--secure-file-priv=</string>
    </array>
```

Then restart MySQL. Now you can export data:
```sql
SELECT *
INTO OUTFILE 'your_file.csv'
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
FROM `your_db`.`your_table`
```

You can find your exported data:
```shell
sudo find /usr/local/mysql/data -name your_file.csv
```

# Node.js

**Install**

macOS  
Download the Node.js installer from https://nodejs.org/en/download/  
or
```shell
brew install node
```

Linux  
```shell
sudo apt install nodejs
sudo apt install npm
```

[Optional] Install nvm from https://github.com/creationix/nvm to manage multiple versions of node and npm.  

Windows Subsystem for Linux  
On WSL the recommended approach for installing a current version of Node.js is nvm.
```shell
touch ~/.bashrc
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
# Reload your configuration
source ~/.bashrc
nvm install node
# Upgrade npm. You may need to run this twice on WSL.
npm install npm@latest -g
```

**Update npm**
If you installed npm as part of node you may need to update npm.
```shell
sudo npm install -g npm
```

**Initialize a node project by creating a package.json**
```shell
npm init
```

**Installing dependencies examples**
```shell
sudo npm install --save ask-sdk moment
sudo npm install --save-dev mocha chai eslint virtual-alexa
```
**Troubleshooting mocha**
If you get an error running mocha tests e.g. ```node_modules/.bin/mocha``` not having execute permissions or *mocha Error: Cannot find module './options'* delete your node_modules folder and ```npm install```.

**Set up ESLint with a configuration file**
```shell
eslint --init
# you may need to run it as:
# sudo ./node_modules/.bin/eslint --init
```