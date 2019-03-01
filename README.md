# eb-node-red
Node-RED for AWS Elastic Beanstalk without reverse proxy.

## Deployment
### Clone project
First of all, you get tjsif project from github. In your working directory.
```sh
$ git clone https://github.com/toconuts/eb-node-red.git
```

## Initialize git
```sh
$ git init
```

## Setting Up the Application Settings
Defines the options if you required
Copy ./settings.js.dist, paste it name as ./settings.js then configure your settings for Node-RED.
```sh
$ cp settings.js.dist settings.js
```

## Initialize Elastic Beanstalk environment
```sh
$ eb init
```

## Initialize Elastic Beanstalk environment
```sh
$ eb create
```

### Options
You want to create a single instance, with option --single
```sh
$ eb create --single
```
You want to create in your vpc
$eb create --vpc.id vpc-xxxxxx --vpc.ec2subnets subnet-xxxx --vpc.publicip --vpc.elbpublic
```sh
```
--vpc.publicip: EC2 attached public IP
--vpc.elbpublic: with out this option, EB is created internal

## How to check routing
```sh
$ sudo iptables -t nat -S PREROUTING -v
```

```sh
$ cat /etc/nginx/conf.d/00_elastic_beanstalk_proxy.conf
```

```sh
sudo netstat -anp
```

```sh
$ sudo od -S1 -An /proc/$(ps aux | grep ^nodejs | perl -anlE 'say $F[1]' | tail -1)/environ
```

```sh
option_settings:
  aws:elasticbeanstalk:container:nodejs:
    ProxyServer: <nginx | apache | none>
```

# Prerequired:
If you have not used EB you should do the followings tutorials first. AWS will create some roles for EB automatically.
## Getting Started Using Elastic Beanstalk
https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/GettingStarted.html
