# IMPLEMENTING LAMP STACK

Before we start we need to have an environemnt to work with. I will we using my AWS account to create an EC2 instance with an Ubuntu Server.

* First thing I'm going to do when I log in to AWS is look for the **EC2** services. There are various methods to navigate to it, here I'm using the **search bar** <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/ec2search.png)

* Once you navigate to the **EC2** page look for a **Launch instance** button <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/launchInstance.png)

* You will then be prompted to pick an OS Image. I will be using **Ubuntu Server 20.04 LTS (HVM), SSD Volume**. Once done click **Select**

* I will pick the **t2.micro** instace type <br /> 
 ![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/t2micro.png)

* I will leave default settings and click **Review and Launch** <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/reviewLaunch.png)

* As you can see we have a Security Group applied to the instance by default which allows SSH connections <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/sshDefault.png)

* After reviewing you can launch your instancing by clicking <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/launch.png)

* We are prompted to create or use an existing Key Pair. I will be creating a new one. I will use this .pem key to SSH into the instance later on. <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/keyPair.png)

* Once you have downloaded your key launch intance <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/LaunchInstances.png)

* To go to the instances dashboard <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/ViewInstances.png)

* If your instance is up and running you will see something like this <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/Running.png)

* To find information on how to connect click on your **Instance ID**

* In the top-right corner you should see the button **Connect**, click on it <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/Connect.png)

* Look for the **SSH client** tab <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/SSHclient.png)

* Under **Example** you'll find an **ssh** command with eveything you need to connect to the instance from a terminal

**Examaple:**
```bash
ssh -i "daro.io.pem" ubuntu@ec2-3-216-90-84.compute-1.amazonaws.com
```
Make sure when you run the command that your current working directory in the terminal is where your KeyPair/.pem is located because in the above example I'm using a relative path to point to my key

* A successful log-in <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/ubunutuLogIn.png)


# STEP1: BACKEND CONFIGURATION
---
First, make sure the system is up to date
```bash
sudo apt update && sudo apt upgrade -y
```
## INSTALL NODE.JS
We get the location of **Node.js** software from [Ubuntu repositories](https://github.com/nodesource/distributions#deb) .

```bash
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
```
Install **Node.js** with the command below
```bash
sudo apt-get install -y nodejs
```
In addition, the command above installs **NPM** a package manager for Node used to install Node modules & packages and to manage dependency conflicts.

Verify the node installation with the command **node -v**
```bash
ubuntu@ip-172-31-59-251:~$ node -v
v12.22.5
```

We can also vertify the installation of Node by checking **NPM** version using command **npm -v**

```bash
ubuntu@ip-172-31-59-251:~$ npm -v
6.14.14
```
Create a new directory for your **To-Do** project and change to it
```bash
mkdir Todo
cd Todo
```

With **Todo** as your current working directory run **npm init** to initialize your project. The initialization process creates **package.json** a file that contains information about your application and dependencies that it needs to run. Follow the prompts after running command and press **Enter** to accepts default values, then accept to write out the package.json file by typing **yes**

```bash
#My output
ubuntu@ip-172-31-59-251:~/Todo$ npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help init` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (todo)
version: (1.0.0)
description: Todo app description
entry point: (index.js)
test command:
git repository:
keywords: todo application
author: Hector Rodriguez
license: (ISC)
About to write to /home/ubuntu/Todo/package.json:

{
  "name": "todo",
  "version": "1.0.0",
  "description": "Todo app description",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "todo",
    "application"
  ],
  "author": "Hector Rodriguez",
  "license": "ISC"
}


Is this OK? (yes)

ubuntu@ip-172-31-59-251:~/Todo$ ls #Checking newly created file
package.json
```

## INSTALL EXPRESSJS
**ExpressJS** (a framework for **Node.js**) simplifies development, and abstracts a lot of low level details. For example, Express helps to define routes of your application based on HTTP methods and URLs.

Install ExpressJS using **npm**:
```bash
npm install express
```
Install ExpressJS using **npm**:
```bash
npm install express
```
Create a file index.js with the command **touch index.js** and run **ls** to confirm file creation
```bash
ubuntu@ip-172-31-59-251:~/Todo$ touch index.js
ubuntu@ip-172-31-59-251:~/Todo$ ls
index.js  node_modules  package-lock.json  package.json
ubuntu@ip-172-31-59-251:~/Todo$
```
Notice there are additional things inside **Todo** after installing **ExpressJS**

Install the **dotenv** module
```bash
npm install dotenv
```
Open **index.js** with your favorite file editor such as nano or vim and copy/paste this code inside.
```javascript
const express = require('express');
require('dotenv').config();
const app = express();
const port = process.env.PORT || 5000;
app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});
app.use((req, res, next) => {
res.send('Welcome to Express');
});
app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
```
Notice that we have specified to use port 5000 in the code. This will be required later when we go on the browser.

Now we start the server(**ExpressJS** + **Node.js**) to see if it works. In the same directory as **index.js** (dir **Todo**) run **node index.js**
```bash
ubuntu@ip-172-31-59-251:~/Todo$ node index.js

Server running on port 5000 #Output if everything is ok
```

* Now we are going to add a rule to our **Security Group** to open **TCP** port **80**
    * Navigate to your intances dashboard and select your instance by cliking the empty box <br /> 
    ![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/checkMark.png)
    * Look for the **Security** tab <br /> 
    ![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/security.png)
    * Click on top of your **Security Group**  <br />
    ![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/securityGroup.png) <br />
    Keep in mind yours might look different
    * Under the tab **Inbound Rules** click **Edit inbound rules** button <br />
    ![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/editInboundRules.png) <br />
    * Click **Add rule** <br />
    ![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/addRule.png) 
    * Pick type **Custom TCP** and source **0.0.0.0/0** meaning all IPs, you can also use drop-down menu **Anywhere-IPv4**
    * Save it <br />
    ![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_STACK/main/images/SaveRules.png) 
## MODELS
## MONGODB DATABASE


# STEP2: FRONTEND CREATION
---
