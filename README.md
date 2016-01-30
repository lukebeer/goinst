# goinst
>Install go1.5.3 from source on linux/amd64

_Saved me some time one evening, pull requests welcome_

####Fancy a line?
```bash
wget -q https://raw.githubusercontent.com/lukebeer/goinst/master/goinst.sh -O - | bash
```

####Not great but does the job...
```bash
#!/bin/bash
reset
cd /usr/local
PURPLE='\033[0;35m';
LPURPLE='\033[1;35m';
BLUE='\033[0;32m';
GREEN='\033[0;34m';
NC='\033[0m'
printf "> ${PURPLE}go1.5.3${BLUE} install & setup with cross compilation support on ${PURPLE}linux/amd64${NC}\n"
printf "> ${BLUE}https://github.com/lukebeer/goinst${NC}\n"
printf "\n"

printf "* ${BLUE}Removing any existing golang packages${NC}\n"
apt-get purge golang gccgo-5 &>/dev/null

printf "* ${BLUE}Updating ${PURPLE}/etc/profile${NC}\n"
export GOPATH=$HOME/go
export GOROOT=/usr/local/go
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
export GOROOT_BOOTSTRAP=/usr/local/go1.4.3
sed -i '#export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin#d' /etc/profile
sed -i '/GOPATH=/d' /etc/profile
sed -i '/GOROOT=/d' /etc/profile
sed -i '/GOROOT_BOOTSTRAP=/d' /etc/profile
cat << 'EOFBASH' >> /etc/profile
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
export GOPATH=$HOME/go
export GOROOT=/usr/local/go
export GOROOT_BOOTSTRAP=/usr/local/go1.4.3
EOFBASH

printf "* ${BLUE}Downloading ${PURPLE}go1.4.3.linux-amd64.tar.gz${BLUE} ...${NC}\n"
wget -q https://storage.googleapis.com/golang/go1.4.3.linux-amd64.tar.gz

printf "* ${BLUE}Extracting ${PURPLE}go1.4.3.linux-amd64.tar.gz${BLUE} into ${PURPLE}/usr/local/go1.4.3${BLUE} ...${NC}\n"
rm -rf /usr/local/go1.4.3
mkdir -p /usr/local/go1.4.3
tar --strip=1 -zxf go1.4.3.linux-amd64.tar.gz -C /usr/local/go1.4.3

printf "* ${BLUE}Cloning ${PURPLE}go.googlesource.com/go${BLUE} branch:master ...${NC}\n"
rm -rf /usr/local/go
git clone -b master -q https://go.googlesource.com/go /usr/local/go
cd /usr/local/go/src

printf "* ${BLUE}Building ${PURPLE}go1.5.3${BLUE} ...${NC}\n"
./all.bash >/dev/null

printf "* ${BLUE}Installing gotools${NC}\n"
go get golang.org/x/tools/cmd/...

printf "* ${GREEN}Finished setup, "
go version
printf "${NC}\n"
```
