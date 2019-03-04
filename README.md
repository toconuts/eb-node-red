# eb-node-red
Node-RED for AWS Elastic Beanstalk w/ or w/o reverse proxy.

## Prerequired
### Doing AWS tutorial
If you have not created EB yet I strongly recommend to do the followings tutorials first. AWS will create some roles include aws-elasticbeanstalk-ec2-role for EB automatically.

[Getting Started Using Elastic Beanstalk - AWS Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/GettingStarted.html)

### Add AWS S3 Full Access Role to aws-elasticbeanstalk-ec2-role
Login to the AWS Console on your browser, select Identity and Access Management (IAM) and add the AmazonS3FullAccess policy to the aws-elasticbeanstalk-ec2-role.

## How to create node-RED using Elastic Beanstalk
### Clone project
First of all, you get eb-node-red project from github in your working directory.
```sh
$ git clone https://github.com/toconuts/eb-node-red.git
$ cd eb-node-red
```

## Initialize git
```sh
$ git init
```

## Setting Up the Application Settings
Defines the options if you required
copy ./settings.js.dist, paste it name as ./settings.js then configure your settings for Node-RED.
```sh
$ cp settings.js.dist settings.js
```
+ awsRegion: region name to create eb-node-red.
+ awsS3Appname: application name usually used the same name as value of "name" key in ./package.json.
+ awsS3Bucket: bucket name to use. node-red-contrib-storage-s3 modules will create application name directory under this bucket.

## Specifying reverse proxy
You can also choose a reverse proxy at ./.ebextensions/proxy.config
```sh
option_settings:
  aws:elasticbeanstalk:container:nodejs:
    ProxyServer: <nginx> | <apache> | <none>
```

## Initialize Elastic Beanstalk environment
```sh
$ eb init
```

## Create Elastic Beanstalk environment
```sh
$ eb create
```

### Create command options
You can create a single instance, with option --single
```sh
$ eb create --single
```
You can create a instance in your vpc
```sh
$eb create --vpc.id vpc-xxxxxx --vpc.ec2subnets subnet-xxxx --vpc.publicip --vpc.elbpublic
```
+ --vpc.publicip: EC2 will be attached to public IP
+ --vpc.elbpublic: without this option, EC2 will be created  internally

## How to check routing
### iptables (PREROUTING)
```sh
$ sudo iptables -t nat -S PREROUTING -v
```

### Nginx configuration
If you choose nginx as proxy
```sh
$ cat /etc/nginx/conf.d/00_elastic_beanstalk_proxy.conf
```

### Network statics
You can see listening ports of node and reverse proxy if you choose.
```sh
$ sudo netstat -anp
```

### Environment variables
PORT variable will show you listening port.
```sh
$ sudo od -S1 -An /proc/$(ps aux | grep ^nodejs | perl -anlE 'say $F[1]' | tail -1)/environ
```
