# Kontinuum

A set of services and tools to help with deployment.

## Prereq

Buy domain name on aws route53, which will automatically create the hosted zone for you.

## Usage

#### Deploy a static site to aws

1. Uses ACM to create certs.
2. Creates buckets in s3.
3. Creates a cloudfront distribution.
4. Creates A records in route53.

`yarn add kontinuum-push --dev`

```json
{
	"scripts": {
		"deploy": "./node_modules/kontinuum-push/push.sh --domain foo.example.com --root example.com --source ./build"
	}
}
```


## Setup Environment

**Important:*s Do this in the environment before running the script.

These scripts are just wrappers around aws-cli so first you need the cli. You will need python and pip because awscli is a python package. Also requires jq.

```
brew install python jq
pip install awscli
```

### Setup Env Vars (Also Needed For CI)

Each script will reference the following env vars.

```
AWS_DEFAULT_REGION
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
```

#### How to get these values?

`AWS_DEFAULT_REGION`: [Region Default List](http://docs.aws.amazon.com/general/latest/gr/rande.html) eg. `us-west-2`

[AWS Best Practices](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html?icmpid=docs_iam_console)

*TL;DR*

1. [Create New Group](https://console.aws.amazon.com/iam/home?region=us-west-2#/groups), attach the following policies.

	- AmazonS3FullAccess
	- AmazonRoute53FullAccess

2. [Create IAM User](https://console.aws.amazon.com/iam/home?region=us-west-2#/users), select `programmatic access` for access type. Add user to the group you just created.
3. 	You can download the csv and save it in a safe location. This file contains your `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.

#### Configure awscli

`aws configure`

You will be prompted for `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.