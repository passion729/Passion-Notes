# Node.js

## NVM

- `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash`
- `nvm install stable`
- `nvm use stable`

## Manually

1. `[https://nodejs.org/en/download](https://nodejs.org/en/download)`
2. `yum install -y xz-devel.x86_64`
3. `xz -d ./node-v16.15.0-linux-x64.tar.xz`
4. `tar -xf ./node-v16.15.0-linux-x64.tar`
5. `mv ./node-v16.15.0-linux-x64 /uar/local/nodejs`
6. `vim /etc/profile`
7. `export NODE_HOME=/usr/local/nodejs`
8. `export PATH=$PATH:$NODE_HOME/bin`
9. `source /etc/profile`
10. `node -v`
11. `npm -v`
12. `npx -v`

