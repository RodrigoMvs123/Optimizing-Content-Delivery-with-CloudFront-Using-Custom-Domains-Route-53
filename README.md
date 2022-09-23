# Optimizing-Content-Delivery-with-CloudFront-Using-Custom-Domains-Route-53

https://www.youtube.com/watch?v=pc2o7pkZTQ0
https://github.com/academind/aws-demos

##

##

##


#!/bin/bash
 
# Enable logs
exec > >(tee /var/log/user-data.log|logger -t user-data -s 2>/dev/console) 2>&1
 
# Install Git
echo "Installing Git"
yum update -y
yum install git -y
 
# Install NodeJS
echo "Installing NodeJS"
touch .bashrc
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
. /.nvm/nvm.sh
nvm install --lts
 
# Clone website code
echo "Cloning website"
mkdir -p /demo-website
cd /demo-website
git clone https://github.com/academind/aws-demos.git .
cd dynamic-website-basic
 
# Install dependencies
echo "Installing dependencies"
npm install
 
# Create data directory (later => EBS)
echo "Creating data directory & file"
mkdir -p /demo/data
echo '{"topics": []}' | tee "/demo/data/data-storage.json"
 
# Forward port 80 traffic to port 3000
echo "Forwarding 80 -> 3000"
iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 3000
 
# Install & use pm2 to run Node app in background
echo "Installing & starting pm2"
npm install pm2@latest -g
pm2 start app.js

Dynamic Web Site with Cloudfront
const path = require('node:path');
 
const express = require('express');
 
const app = express();
 
app.use(express.static('public', { maxAge: 10 * 60 * 1000 }));
 
app.get('/', function (req, res) {
  const filePath = path.join(__dirname, 'views', 'index.html');
  res.sendFile(filePath, { maxAge: 2 * 60 * 1000 });
});
 
app.listen(3000);
 

User Data
Web Server
Launch Instance 
#!/bin/bash
 
# Enable logs
exec > >(tee /var/log/user-data.log|logger -t user-data -s 2>/dev/console) 2View Load Balancer 
DNS name - demo1…

CloudFront 
Create a Cloudfront Distribution 
Create Distribution 
Elastic Load Balancer / DNS name - demo1… 
Protocol / http only 
Http Port / 80 
Create Distribution 

CloudFront > Distributions > E1C66OICQQSJ7C

Details 
Distribution Domain Name (d39b…) 
Send a request ( Url ) 

Route 53 Dashboard / Hosted zones
Domain Name ( demo1… ) 
Route Traffic to / Alias to CloudFront Distribution 

Edit Settings 
Alternate domain name ( CNAME ) ( demo1..)
Request public certificate 
Domain names
Fully qualified domain name 
demo1-dummy.com 
Certificates
Certificates ID 
Create records in Route 53 

Custom SSL  Certificate (demo1-dummy.com (SOf9…)  ) 
Save Changes 
Hosted Zone Details 
Edit Record / Route Traffic to (d39bk…) 
/ Browser >&1
 
# Install Git
echo "Installing Git"
yum update -y
yum install git -y
 
# Install NodeJS
echo "Installing NodeJS"
touch .bashrc
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
. /.nvm/nvm.sh
nvm install --lts
 
# Clone website code
echo "Cloning website"
mkdir -p /demo-website
cd /demo-website
git clone https://github.com/academind/aws-demos.git .
cd dynamic-website-with-cloudfront
 
# Install dependencies
echo "Installing dependencies"
npm install
 
# Forward port 80 traffic to port 3000
echo "Forwarding 80 -> 3000"
iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 3000
 
# Install & use pm2 to run Node app in background
echo "Installing & starting pm2"
npm install pm2@latest -g
pm2 start app.js


Firewall - Security Group 
Public IPv4 DNS - Browser path 

Route 53 Dashboard 
Domains 
Registered Domains 
Choose a Domain Name 
Demo 1 

Specify Group Details  
Choose a target type / Target group name ( tg1 ) 

Available Instances 

Create Application Load Balancer 
Basic Configuration 
Load Balancer Name  ( demo 1  ) 
Create Load Balancer 

