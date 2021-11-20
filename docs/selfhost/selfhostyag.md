---
sidebar_position: 1
---

# How to Self Host YAGPDB without Docker

:::caution
This document requires that you have intermediate knowledge of Linux operating system!
:::

:::info
If you want to use Docker instead, click [here](selfhost/docker).
:::

Need help installing your very own instance of Yet Another General Purpose Discord Bot? Welcome to the Unofficial Guide on installing YAGPDB for self hosting.

If you have somehow found this website, and you're unsure what Yet Another General Purpose Discord Bot is, you can find out more information on their Discord.

## Prerequisites

Before we begin to install Yet Another General Discord Bot, or YAGPDB for short, you will need the following prerequisites:

- Server with Root and/or sudo access. Need one? See [here](/resources/hostingproviders)
- Debian 10 or Ubuntu 20.04 (or newer if available)
- Seperate user for the bot
- Git
- Golang (1.17 or later.)
- PostgreSQL (9.6 or later.)
- Redis (V3.x or later.)
- Optional: FFmpeg (for soundboard)

How to Install Git:
```
sudo apt update
sudo apt install git
```

How to Install Golang:
```
wget https://golang.org/dl/go1.17.2.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.17.2.linux-amd64.tar.gz
```

Please append the following line of code to the bottom of your ~/.profile like so:
```
nano ~/.profile
```

Insert the following lines bottom of the file:
```
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
```

Save the file with CTRL+O and close it with CTRL+X.  
Then do the following command:
```
source ~/.profile
```

:::tip
If you'd like to verify that Go has been installed you can run:
```
go version
```
Which should output depending on installed version:

```
go version go1.17.2 linux/amd64
```
:::

How to Install & Configure PostgreSQL:
```
sudo apt install postgresql postgresql-contrib
```

Then to configure and setup your database & user
:::caution
Please make sure you change 'mypass' to your own secure password.
:::
```
sudo -u postgres psql
create database yagpdb;
create user yagpdb with encrypted password 'mypass';
grant all privileges on database yagpdb to yagpdb;
```
:::tip
Make sure you switch back to the root user by running the following command.
```
\q
```
:::

How to Install & Configure Redis:
```
sudo apt install redis-server
```

Now to configure Redis.
```
sudo nano /etc/redis/redis.conf
```

Within this file.. please edit the following value:

- supervised
  * Set to: `systemd`
- maxmemory
  * Set to: `256mb`
- maxmemory-policy
  * Set to: `allkeys-lru`

Save **redis.conf** and then run
```
sudo systemctl restart redis.service
```

If you wish to secure your Redis & PostgreSQL installations, check out these articles:  
Redis: [Click Here](https://www.gspann.com/resources/blogs/best-practices-to-secure-redis-implementation-in-cloud-infrastructure/)  
PostgreSQL: [Click Here](https://www.upguard.com/blog/10-ways-to-bolster-postgresql-security)

## Optional: Install FFmpeg

FFmpeg is an optional requirement for the soundboard feature of YAGPDB. You don't have to install it, but if you wish to use the soundboard it is required to be installed. To install it, all you need to do is run:
```
sudo apt install ffmpeg
```

---

## Configuring YAGPDB

Before downloading Yet Another General Purpose Discord Bot, we need to first configure it. To do this, once again open ~/.profile:

```
nano ~/.profile
```

Then, copy the [env variables](https://raw.githubusercontent.com/botlabs-gg/yagpdb/master/cmd/yagpdb/sampleenvfile) and paste them in.
:::note
You do not have to set each variable. For basic functionality all you need to edit is:

- YAGPDB_CLIENTID - Your Discord Application ID. Found on the [Developer Portal](https://discord.com/developers)
- YAGPDB_CLIENTSECRET - Your Discord Application Client Secret. Found on the [Developer Portal](https://discord.com/developers)
- YAGPDB_BOTTOKEN - Your Discord Application BOT Token. Found on the [Developer Portal](https://discord.com/developers)
- YAGPDB_HOST - The domain of which YAGPDB's panel will be accessible from. Example: `bot.example.com`
- YAGPDB_EMAIL - The email of which your SSL certificate will be assigned to.
- YAGPDB_PQUSERNAME - The username you set when creating your PostGreSQL database user. If following this guide, you most likely set this to 'yagpdb'
- YAGPDB_PQPASSWORD - The password you set when creating your PostGreSQL database user.
- YAGPDB_REDIS - The address for your redis installation. If following this guide, this is most likely set to 'localhost:6379'
:::

Upon pasting the env variables, upon each variable, prefix it with "export". Example:
```
export YAGPDB_OWNER="Insert owner id here"
export YAGPDB_CLIENTID="Insert client id here"
export YAGPDB_CLIENTSECRET="Insert client secret here"
export YAGPDB_BOTTOKEN="Bot token_here"
```

Once you have finished configuring your variables, save the file and then reload the file.
```
source ~/.profile
```

## Downloading YAGPDB

After installing all of the prerequisites, it's time to download and install Yet Another General Purpose Discord Bot.

To download YAGPDB, run the following commands:
```
git clone https://github.com/botlabs-gg/yagpdb
cd yagpdb/cmd/yagpdb
go build 
```
This process may take a while, so go grab a cup of tea.

## Running YAGPDB

Finally, To run YAGPDB all you have to do is run:
```
./yagpdb -all
```

:::note
This will run everything that YAGPDB has to offer.  
If you don't have any plugins setup, they will spit out errors.  
Also, this will run bot in ports 5000 and 5001.  
If you want to run it on ports 80 and 443, add flag -pa
:::

This will run YAGPDB as long as your terminal session remains open. If you'd like to keep YAGPDB and close your session, I recommend using nohup. You can learn how to use nohup [here](https://hexadix.com/use-nohup-execute-commands-background-keep-running-exit-shell-promt/).

---

## Did this guide help?
If you need more support, you may contact us on #self-hosting-discussion channel at [Discord](https://discord.gg/4udtcA5).

