# easy-mock-deployment
Deploying easy-mock in the intranet server

- 环境搭建：nodejs-->v8.9.0,mongodb-->v3.4,redis-->v4.0.2 


# install nodejs
> wget https://nodejs.org/dist/v8.9.0/node-v8.9.0-linux-x64.tar.gz

> tar xf node-v8.9.0-linux-x64.tar.gz -C /usr/local/

> cd /usr/local/

> mv node-v8.9.0-linux-x64/ nodejs

- create symbolic link 
> ln -s /usr/local/nodejs/bin/node /usr/local/bin

> ln -s /usr/local/nodejs/bin/npm /usr/local/bin

- test 
> node -v

> npm -v


# install mongodb
- in case of errors during the installation,update your yum,it will take a long time 
> yum update

- yum source with mongodb addition 
> cd /etc/yum.repos.d/

> vim mongodb-3.4.repo
- copy the following words to mongodb-3.4.repo,you can change gpgcheck to 0 to avoid gpg validation 
>[mongodb-org-3.4] 

>name=MongoDB Repository 

>baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/ 

>gpgcheck=1 

>enabled=1 

>gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc

- install 
> yum install -y mongodb-org

- create database storage and log folder 
> mkdir /data

> mkdir /data/db

> mkdir /data/logs

> mkdir /data/logs/mongodb

> mkdir /data/logs/mongodb/logs

- start mongodb 
> mongod --fork --logpath=/data/logs/mongodb/logs/mogondb.log

- create database
> mongo

> use easymockdb

# Abnormal stop can cause startup error
- repair：delete mongod.lock in /data/db and logs,then start mongodb in --repair way [😳failed,actually i don't know how to repair it]
- uninstall-->reinstall 
> delete mongod.lock in /data/db and logs
> yum list installed | grep mongo [find installation packages related to mongodb]

> yum erase XXX [delete related packages]

> reinstall

# install redis
> wget http://download.redis.io/releases/redis-4.0.2.tar.gz

> tar xzf redis-4.0.2.tar.gz

> cd redis-4.0.2

> make

> make install

> cd /usr/local/bin

> redis-server   //startup

# deploy easy-mock

- download project 
> git clone https://github.com/easy-mock/easy-mock.git

> npm install

- configure 
> cd easy-mock

> cp config/default.json config/local.json

> vi config/local.json

"db": "mongodb://localhost:27017/easymockdb",

- build 
> npm run build

- startup 
> npm run dev

DONE Compiled successfully in 476ms [start successfully]


# Using PM2 to ensure continuous working of the project

- install 
> npm i -g pm2

- create symbolic link or it will prompt that the PM2 command does not exist[maybe😀] 
> ln   -s   xxx[path of pm2]   /usr/local/bin

> pm2 start app.js [start]

> pm2 stop app.js [stop]


** if you want to deploy it on virtual machine,some configurations is needed **

[VMware]Edit->Virtual Network Editor->NAT（VMnet8）->Edit->Port Forwarding,add mapping host port：80，virtual machine IP address:xxx，Port：80
