# Example usage of the Moonshot Library

In this example we are going to use the resources of the sample directo
This example assumes you have access to an Amazon AWS account and have sufficient permissions to create roles and resources.

## So, what's in it for me?

After setup, you will be able to repeatedly deploy a PHP app to one or more
isolated environments on AWS by running the following command:

```shell
bundle exec bin/environment deploy-code
```

You will also be able to update the OS and supporting software by pulling the
latest changes in this repository and updating the stack with following command:

```shell
bundle exec bin/environment update
```

Lastly, you get a disposable, light-weight application that can be used to learn
and hack on Moonshot without having to muck with applications that actually
matter.

## Setup

### Create a service role for Code Deploy in your AWS account.

Create a role called CodeDeployRole with the AWSCodeDeployRole policy

```bash
aws iam create-role --role-name CodeDeployRole --assume-role-policy-document '{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": [
          "codedeploy.amazonaws.com"
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}'
aws iam attach-role-policy --role-name CodeDeployRole --policy-arn arn:aws:iam::aws:policy/service-role/AWSCodeDeployRole
```

If you wish to do this manually, follow the
[Create a Service Role for AWS CodeDeploy](http://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-create-service-role.html)
documentation, and make sure to name the role `CodeDeployRole` as that is
what Moonshot expects.

### Install Moonshot and it's dependencies.

This step assumes that you have [Bundler](http://bundler.io/) and a modern version of Ruby installed on your system. See Moonshot's [requirements](index.md#requirements)

```shell
bundle install
```

## Usage of the CLI

Run the following commands to create your environment and deploy code to it.
Note that you will have to set the `AWS_REGION` environment variable prior to running these commands. If it's not set, it will use the default AWS region which at the time of this writing is us-east-1.

A detailed explanation of [all the CLI commands can be found in the User Guide](user-guide/cli.md)

You can now deploy your software to a new stack with:

```shell
$ ./bin/environment create
```

By default, you'll get a development environment named `moonshot-sample-app`. If you want to provision test or production
named environment, use:

```shell
$ ./bin/environment create -n my-service-staging
$ ./bin/environment create -n my-service-production
```

By default, create launches the stack and deploys code. If you want to only
create the stack and not deploy code, use:

```shell
$ ./bin/environment create --no-deploy
```

If you make changes to your application and want to release a development build
to your stack, run:

```shell
$ ./bin/environment deploy-code
```

To build a "named build" for releasing through test and production environments,
use:

```shell
$ ./bin/environment build-version v0.1.0
$ ./bin/environment deploy-version v0.1.0 -n <environment-name>
```

To see the outputs of the stack you just spun up:

```shell
$ ./bin/environment build-version v0.1.0
$ ./bin/environment deploy-version v0.1.0 -n <environment-name>
```

Tear down your stack by running the following command:

```shell
bundle exec bin/environment delete
```