
# CONFIGURE APACHE AS A LOAD BALANCER

## Web servers Public IP                      Private IP
Webserver 1   http://18.133.254.250/       172.31.32.210
http://13.41.76.49/                        172.31.33.235

### Launch Ubuntu Instance
### Connect to Load balancer Instance
### Port 80 opened on ALB security group

## Install Apache Load Balancer on Proj8LB server and configure it to point traffic coming to LB to both Web Servers:

### Install apache2
sudo apt update
sudo apt install apache2 -y
sudo apt-get install libxml2-dev

### Enable following modules:

sudo a2enmod rewrite
sudo a2enmod proxy
sudo a2enmod proxy_balancer
sudo a2enmod proxy_http
sudo a2enmod headers
sudo a2enmod lbmethod_bytraffic

### Restart apache2 service
sudo systemctl restart apache2

### Check if apache2 is up and running

sudo systemctl status apache2



# Configure load balancing

sudo vi /etc/apache2/sites-available/000-default.conf

## Add this configuration into this section <VirtualHost *:80>  </VirtualHost>

<Proxy "balancer://mycluster">
               BalancerMember http://172.31.32.210:80 loadfactor=5 timeout=1
               BalancerMember http://172.31.33.235:80 loadfactor=5 timeout=1
               ProxySet lbmethod=bytraffic
               # ProxySet lbmethod=byrequests
        </Proxy>

        ProxyPreserveHost On
        ProxyPass / balancer://mycluster/
        ProxyPassReverse / balancer://mycluster/

### Restart apache server

sudo systemctl restart apache2

## To verify that our configuration works – 
## We access the LB’s public IP address or Public DNS name from your browser:
http://18.130.145.4/index.php

## Succesfully configured Apache ALB



