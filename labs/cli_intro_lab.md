# Introduction to AWS CLI

## Create Access Key

**Introduction**

To use the AWS CLI from your local machine or a server outside the AWS cloud you need to create some Access Keys for
the user in order to authenticate the requests to the CLI.

Access keys consist of an access key ID and secret access key, which are used to sign programmatic requests that are
made to AWS. If you don't have access keys, you can create them by using the AWS Management Console.

**Instructions**

To create an Access Key for your user, from the AWS Management Console click *IAM* under the *Security, Identity &
Compliance* section.

In the IAM console, click on *Users* then click on the user called *student*.

Click on the *Security Credentials* tab then on the *Create access key* button.

That will create a new Access key and open a new pop-up window with the *Access key ID* and the *Secret access key*
displayed. Click *Show*  to expand the *Secret access key* information.

| Access Key ID        | Secret Access key                        |
|----------------------|------------------------------------------|
| AKIAVL6ZT7TO3OJHLQTI | tRa4P/R+GV5OeLCNvlGrRpM9LI4UE2A1XmRI7hd/ |

You will need to save this information somewhere because it is needed in the next step. You have two options: copy
and paste these values into a text file or click on the *Download .csv* file button to download a CSV file with your
credential's information.

## Connecting to an Instance using SSH

**Introduction**

In order to manage a remote Linux server, you must employ an SSH client. Secure Shell (SSH) is a cryptographic
network protocol for securing data communication. It establishes a secure channel over an insecure network.
Common applications include remote command-line login and remote command execution.

Linux distributions and macOS ship with a functional SSH client that accepts standard PEM keys. Windows does not
ship with an SSH client. Therefore, this Lab Step includes instructions for users running Linux/macOS and Windows
on their local host. Only one of them is required depending on your local operating system.

An instance is already available to you, the name is *AWS CLI VM*.

**Windows Instructions**

Windows has no SSH client, so you must install one. This part of the Lab Step will use PuTTY and a previously converted PEM key (converted from PPK using PuTTYgen).

1. Open PuTTY and insert the EC2 instance public IP Address in the *Host Name* field.

2. Navigate to *Connection > SSH > Auth* in the left pane and then select the downloaded private key that you previously converted to PPK format.

3. Login as `ec2-user` and you will see the EC2 server welcome banner and be placed in the Linux shell.

## Configuring the AWS CLI

**Introduction**

Before you can start using the AWS CLI, it needs to know who is running the commands to understand what commands you are authorized to execute. In this Lab Step, you will configure the AWS CLI using the student user access key you recorded in a previous Lab Step.

*Note*: The virtual machine for this Lab runs the Amazon distribution of Linux, which pre-installs the AWS CLI. The
AWS CLI can easily be installed on many different platforms. If you want to know more about the install options visit
the [documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) page. You can also
configure multiple profiles for the AWS CLI. To learn how refer to the [documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html#cli-config-files).

**Instructions**

1. Enter the following command to check what version is installed: `aws --version`

```sh
[ec2-user@ip-10-0-0-70 ~]$ aws --version
aws-cli/1.11.29 Python/2.7.16 Linux/4.9.20-11.31.amzn1.x86_64 botocore/1.4.86
```

2. Upgrade to the latest version of the AWS CLI: `sudo pip install --upgrade awscli`

pip is the Python package manager that the AWS CLI has been installed with. The AWS CLI can be installed by other methods, so this will only work when it was installed using pip.

3. Re-check the version of the AWS CLI: `aws --version`

```sh
[ec2-user@ip-10-0-0-70 ~]$ aws --version
aws-cli/1.17.10 Python/2.7.16 Linux/4.9.20-11.31.amzn1.x86_64 botocore/1.14.10
```

The version changed from 1.11.29 to 1.17.10 in the screenshot. You may see a newer version.

4. To start configuring enter: `aws configure`

The AWS CLI can be configured in several ways, but you will use the quick configure command provided by the CLI
itself. The command will start a wizard where you can enter key information that will be used as the default for
all commands you issue with the AWS CLI.

5. Enter the following values to the wizard prompts:

- **AWS Access Key ID**: Enter the Access Key ID you recorded in an earlier Lab Step
- **AWS Secret Access Key**: Enter the Secret Access Key you recorded in an earlier Lab Step
- **Default region name**: *us-west-2* (The AWS region your commands will apply to when no region is specified)
- **Default output format**: *text* (The default output format to use when no format is specified. Allowable values
  are text, json, and table)

JSON is best for handling the output programmatically via various languages or jq (a command-line JSON processor).
Text will return some basic text with tab-delimited values. The table format is easy for humans to read, but more
difficult to deal with programmatically. Text format works well with traditional Unix text processing tools, such
as `sed`, `grep`, and `awk`, as well as Windows PowerShell scripts.

6. Test that the AWS CLI is configured by entering the following command: `aws ec2 describe-regions`

```sh
[ec2-user@ip-10-0-0-70 ~]$ aws ec2 describe-regions
REGIONS ec2.eu-north-1.amazonaws.com    opt-in-not-required     eu-north-1
REGIONS ec2.ap-south-1.amazonaws.com    opt-in-not-required     ap-south-1
REGIONS ec2.eu-west-3.amazonaws.com     opt-in-not-required     eu-west-3
REGIONS ec2.eu-west-2.amazonaws.com     opt-in-not-required     eu-west-2
REGIONS ec2.eu-west-1.amazonaws.com     opt-in-not-required     eu-west-1
REGIONS ec2.ap-northeast-2.amazonaws.com        opt-in-not-required     ap-northeast-2
REGIONS ec2.ap-northeast-1.amazonaws.com        opt-in-not-required     ap-northeast-1
REGIONS ec2.sa-east-1.amazonaws.com     opt-in-not-required     sa-east-1
REGIONS ec2.ca-central-1.amazonaws.com  opt-in-not-required     ca-central-1
REGIONS ec2.ap-southeast-1.amazonaws.com        opt-in-not-required     ap-southeast-1
REGIONS ec2.ap-southeast-2.amazonaws.com        opt-in-not-required     ap-southeast-2
REGIONS ec2.eu-central-1.amazonaws.com  opt-in-not-required     eu-central-1
REGIONS ec2.us-east-1.amazonaws.com     opt-in-not-required     us-east-1
REGIONS ec2.us-east-2.amazonaws.com     opt-in-not-required     us-east-2
REGIONS ec2.us-west-1.amazonaws.com     opt-in-not-required     us-west-1
```

Without configuring the AWS CLI, this command would not be allowed because the CLI doesn't know who is running
the command and permission would not be granted.

**Summary**

In this Lab Step, you configured the AWS CLI to run commands as the student user by using the student's access key.
You also assigned a default region and format for AWS CLI commands.

## Working with the AWS CLI

**Introduction**

Amazon provides a lot of excellent documentation for the AWS CLI. You will likely find yourself using one of
the following to help with your learning curve:

- aws help (from the command line)
- [AWS CLI Reference documentation](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-regions.html)

This Lab Step will focus on the command line help.


**Instructions**

1. From your SSH shell, enter the following command:

`aws help`

This is the AWS CLI help page. You can press *enter* to scroll down or *q* to quit.

2. Press *enter* until you can see the *AVAILABLE SERVICES* section:

As you can see, in the `aws help` command there are many services available. Thus, the AWS CLI makes use of a common
pattern for working with each service: `aws <service> <command>`. For example, `aws ec2 describe-regions`.

3. Follow the AWS CLI service pattern to get help on commands available for the EC2 service:

`aws ec2 help`

The service help pages simply give a description and then list all of the available commands for the service.

4. Get the help page for a specific command:

`aws ec2 describe-regions help`

Help pages for service commands provide additional sections for how to use the command:

*SYNOPSIS*: Shows the usage of the command
*OPTIONS*: Describes the options/arguments you can provide on the command-line to modify the behavior of the command
*EXAMPLES*: Provides sample commands along with a text description of what the command accomplishes and the output the
command produces
*OUTPUT*: Describes the format of the output of the command

5. Specify JSON output format appending `--output json`:

```sh
[ec2-user@ip-10-0-0-70 ~]$ aws ec2 describe-regions --output json
{
    "Regions": [
        {
            "OptInStatus": "opt-in-not-required",
            "Endpoint": "ec2.eu-north-1.amazonaws.com",
            "RegionName": "eu-north-1"
        },
        {
            "OptInStatus": "opt-in-not-required",
            "Endpoint": "ec2.ap-south-1.amazonaws.com",
            "RegionName": "ap-south-1"
        },
        {
            "OptInStatus": "opt-in-not-required",
            "Endpoint": "ec2.eu-west-3.amazonaws.com",
            "RegionName": "eu-west-3"
        },
        {
            "OptInStatus": "opt-in-not-required",
            "Endpoint": "ec2.eu-west-2.amazonaws.com",
            "RegionName": "eu-west-2"
        },
        ...
    ]
}
```

Recall earlier that you defined *text* as your default output, so all the commands that support text output should return text. However, you can specify the desired output when entering a command by appending `--output <type>`.

6. Display the output of the command in table format: `aws ec2 describe-regions --output table`


---------------------------------------------------------------------------------
|                                DescribeRegions                                |
+-------------------------------------------------------------------------------+
||                                   Regions                                   ||
|+-----------------------------------+-----------------------+-----------------+|
||             Endpoint              |      OptInStatus      |   RegionName    ||
|+-----------------------------------+-----------------------+-----------------+|
||  ec2.eu-north-1.amazonaws.com     |  opt-in-not-required  |  eu-north-1     ||
||  ec2.ap-south-1.amazonaws.com     |  opt-in-not-required  |  ap-south-1     ||
||  ec2.eu-west-3.amazonaws.com      |  opt-in-not-required  |  eu-west-3      ||
||  ec2.eu-west-2.amazonaws.com      |  opt-in-not-required  |  eu-west-2      ||
||  ec2.eu-west-1.amazonaws.com      |  opt-in-not-required  |  eu-west-1      ||
||  ec2.ap-northeast-2.amazonaws.com |  opt-in-not-required  |  ap-northeast-2 ||
||  ec2.ap-northeast-1.amazonaws.com |  opt-in-not-required  |  ap-northeast-1 ||
||  ec2.sa-east-1.amazonaws.com      |  opt-in-not-required  |  sa-east-1      ||
||  ec2.ca-central-1.amazonaws.com   |  opt-in-not-required  |  ca-central-1   ||
||  ec2.ap-southeast-1.amazonaws.com |  opt-in-not-required  |  ap-southeast-1 ||
||  ec2.ap-southeast-2.amazonaws.com |  opt-in-not-required  |  ap-southeast-2 ||
||  ec2.eu-central-1.amazonaws.com   |  opt-in-not-required  |  eu-central-1   ||
||  ec2.us-east-1.amazonaws.com      |  opt-in-not-required  |  us-east-1      ||
||  ec2.us-east-2.amazonaws.com      |  opt-in-not-required  |  us-east-2      ||
||  ec2.us-west-1.amazonaws.com      |  opt-in-not-required  |  us-west-1      ||
||  ec2.us-west-2.amazonaws.com      |  opt-in-not-required  |  us-west-2      ||
|+-----------------------------------+-----------------------+-----------------+|


7. Display the output in text format:

`aws ec2 describe-regions --output text`

Because you configured text as the default output format, you can omit the `--output option` and achieve the same result.

8. Describe the VPCs in the default region *us-west-2*: `aws ec2 describe-vpcs --output json`

```sh
[ec2-user@ip-10-0-0-70 ~]$ aws ec2 describe-vpcs --output json
{
    "Vpcs": [
        {
            "VpcId": "vpc-09c626abc3d695536",
            "InstanceTenancy": "default",
            "Tags": [
                {
                    "Value": "cloudacademylabs",
                    "Key": "aws:cloudformation:stack-name"
                },
                {
                    "Value": "cloudacademylabs",
                    "Key": "Lab"
                },
                {
                    "Value": "Lab VPC",
                    "Key": "Name"
                },
                {
                    "Value": "arn:aws:cloudformation:us-west-2:418611335002:stack/cloudacademylabs/1188c750-4833-11ea-a025-0ac1c883d400",
                    "Key": "aws:cloudformation:stack-id"
                },
                {
                    "Value": "VPC",
                    "Key": "aws:cloudformation:logical-id"
                }
            ],
            "CidrBlockAssociationSet": [
                {
                    "AssociationId": "vpc-cidr-assoc-07b7fc307545037c3",
                    "CidrBlock": "10.0.0.0/20",
                    "CidrBlockState": {
                        "State": "associated"
                    }
                }
            ],
            "State": "available",
            "DhcpOptionsId": "dopt-0cf7e8b4093ee9cce",
            "OwnerId": "418611335002",
            "CidrBlock": "10.0.0.0/20",
            "IsDefault": false
        },
        ...
    ]
}
```

Notice there are two VPCs in this region--I did not show both in these notes.

9. Specify a different region by appending --region <region name>: `aws ec2 describe-vpcs --region us-east-1`

This time, only one VPC is shown, the default VPC for the region. If you don't specify a region the AWS CLI will use the default region previously configured.

```sh
[ec2-user@ip-10-0-0-70 ~]$ aws ec2 describe-vpcs --region us-east-1
VPCS    172.31.0.0/16   dopt-9936c2fd   default True    418611335002    available       vpc-5719ce30
CIDRBLOCKASSOCIATIONSET vpc-cidr-assoc-f7aa439e 172.31.0.0/16
```

10. Change your default format to JSON by entering `aws configure` by entering *json* for the format prompt and pressing
    enter to keep the existing value for the other options.

**Summary**

In this Lab Step, you learned the basics of entering AWS CLI commands, and how to get more information on
available commands.

Spend a few minutes and play around a bit more with the help tool. You will learn it is a very powerful tool to help
you find useful commands. After using the AWS CLI and the help tool for a while, you will discover the majority of
the commands follow the same pattern. Thus it is possible to guess correct commands based on the action you want
to perform.

For example, using the command line help, try to find the command to describe all the instances in a particular
region or list all S3 buckets that you have in your account.

## Operating S3 with the AWS CLI

**Introduction**

Now it is time to see the AWS CLI in action. At the end of this Lab Step, you will have a new S3 bucket with
static website hosting enabled and several HTML files uploaded to it.

**Instructions**

1. Enter the following command to create a new S3 bucket in the account: `aws s3 mb s3://<UNIQUE_BUCKET_NAME>`
  - where you replace `<UNIQUE_BUCKET_NAME>` with a globally unique name for your bucket:
  - `aws s3 mb s3://lab-bucket-8882`

```sh
[ec2-user@ip-10-0-0-70 ~]$ aws s3 mb s3://lab-bucket-8882828
make_bucket: lab-bucket-8882
```

2. List all the buckets in your account to verify the bucket exists: `aws s3 ls`

```sh
[ec2-user@ip-10-0-0-70 ~]$ aws s3 ls
2020-02-05 16:57:35 lab-bucket-8882
```

3. Download and extract a zip file containing files for a static website: `wget https://github.com/cloudacademy/static-website-example/archive/master.zip`

```sh
[ec2-user@ip-10-0-0-70 ~]$ wget https://github.com/cloudacademy/static-website-example/archive/master.zip
--2020-02-05 16:59:10--  https://github.com/cloudacademy/static-website-example/archive/master.zip
Resolving github.com (github.com)... 192.30.255.112
Connecting to github.com (github.com)|192.30.255.112|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://codeload.github.com/cloudacademy/static-website-example/zip/master [following]
--2020-02-05 16:59:11--  https://codeload.github.com/cloudacademy/static-website-example/zip/master
Resolving codeload.github.com (codeload.github.com)... 192.30.255.120
Connecting to codeload.github.com (codeload.github.com)|192.30.255.120|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [application/zip]
Saving to: ‘master.zip’

master.zip                       [ <=>                                         ] 675.80K  --.-KB/s    in 0.1s

2020-02-05 16:59:11 (6.09 MB/s) - ‘master.zip’ saved [692015]
```

You will use the bundled files to demonstrate uploading a website to S3 from the AWS CLI.

4. Unzip the HTML files: `unzip master.zip`

The files are unzipped into a directory called *static-website-example-master*.

5. Change into website directory: `cd static-website-example-master`

 ```sh
[ec2-user@ip-10-0-0-70 ~]$ cd static-website-example-master
[ec2-user@ip-10-0-0-70 static-website-example-master]$ ls
assets  error  images  index.html  LICENSE.MD  README.MD
```
6. Copy the index.html file to your bucket: `aws s3 cp index.html s3://<UNIQUE_BUCKET_NAME>`
  - where you replace `<UNIQUE_BUCKET_NAME>` with the name of the bucket you created earlier:

```sh
[ec2-user@ip-10-0-0-233 static-website-example-master]$ aws s3 cp index.html s3://lab-bucket-8882
upload: ./index.html to s3://lab-bucket-8882/index.html
```

This command copies files from one place to another. In the above example, it copies files from your local computer
(EC2 Instance) to the specified S3 bucket. You can use this command to copy files from an S3 bucket to your machine,
from your machine to an S3 bucket or between S3 buckets. The usage is as follows:

`aws s3 cp <source> <destination>`

*Note*: Remember to start with `s3://` to indicate a bucket, otherwise the command will look for files on your local
machine.

7. Add the `--recursive` option to recursively copy files in subdirectories, effectively copying all files in the
   directory and subdirectories: `aws s3 cp . s3://<UNIQUE_BUCKET_NAME> --recursive`.

```sh

[ec2-user@ip-10-0-0-233 static-website-example-master]$ aws s3 cp . s3://lab-bucket-8882 --recursive
upload: assets/css/font-awesome.min.css to s3://lab-bucket-8882/assets/css/font-awesome.min.css
upload: ./README.MD to s3://lab-bucket-8882/README.MD
upload: assets/css/main.css to s3://lab-bucket-8882/assets/css/main.css
upload: assets/fonts/fontawesome-webfont.woff to s3://lab-bucket-8882/assets/fonts/fontawesome-webfont.woff
...
```

8. List the files and directories in your s3 bucket by including the bucket name in the ls command: `aws s3 ls s3://<UNIQUE_BUCKET_NAME>`

```sh
[ec2-user@ip-10-0-0-233 static-website-example-master]$ aws s3 ls s3://lab-bucket-8882
                           PRE assets/
                           PRE error/
                           PRE images/
2020-02-05 18:39:21      17128 LICENSE.MD
2020-02-05 18:39:21        648 README.MD
2020-02-05 18:39:21      14522 index.html
```

In the output, *PRE* indicates a prefix. Prefixes are treated as directories, although S3 doesn't really have
directories. Rather, it treats prefixes on object/file names to indicate a directory structure.

9. List the files in a particular directory inside your bucket including the path after the bucket name: `aws s3 ls s3://<UNIQUE_BUCKET_NAME>/assets/`

```sh
[ec2-user@ip-10-0-0-233 static-website-example-master]$ aws s3 ls s3://lab-bucket-8882/assets/
                           PRE css/
                           PRE fonts/
                           PRE js/
                           PRE sass/
```

10. Append the `--recursive` option to the previous command to show all files/folders under a given directory/prefix:
`aws s3 ls s3://<UNIQUE_BUCKET_NAME>/assets/ --recursive`

```sh
[ec2-user@ip-10-0-0-233 static-website-example-master]$ aws s3 ls s3://lab-bucket-8882/assets/ --recursive
2020-02-05 18:39:21      29063 assets/css/font-awesome.min.css
2020-02-05 18:39:21        454 assets/css/ie9.css
2020-02-05 18:39:21      32628 assets/css/main.css
2020-02-05 18:39:21        205 assets/css/noscript.css
2020-02-05 18:39:21     124988 assets/fonts/FontAwesome.otf
2020-02-05 18:39:21      76518 assets/fonts/fontawesome-webfont.eot
2020-02-05 18:39:21     391622 assets/fonts/fontawesome-webfont.svg
2020-02-05 18:39:21     152796 assets/fonts/fontawesome-webfont.ttf
2020-02-05 18:39:21      90412 assets/fonts/fontawesome-webfont.woff
2020-02-05 18:39:21      71896 assets/fonts/fontawesome-webfont.woff2
2020-02-05 18:39:21      95957 assets/js/jquery.min.js
2020-02-05 18:39:21       8801 assets/js/main.js
2020-02-05 18:39:21       9085 assets/js/skel.min.js
2020-02-05 18:39:21      12433 assets/js/util.js
2020-02-05 18:39:21        790 assets/sass/base/_page.scss
2020-02-05 18:39:21       3309 assets/sass/base/_typography.scss
2020-02-05 18:39:21        531 assets/sass/components/_box.scss
2020-02-05 18:39:21       1784 assets/sass/components/_button.scss
2020-02-05 18:39:21       5170 assets/sass/components/_form.scss
2020-02-05 18:39:21        286 assets/sass/components/_icon.scss
2020-02-05 18:39:21       1439 assets/sass/components/_image.scss
2020-02-05 18:39:21       3374 assets/sass/components/_list.scss
2020-02-05 18:39:21       1399 assets/sass/components/_table.scss
2020-02-05 18:39:21        616 assets/sass/ie9.scss
2020-02-05 18:39:21       1616 assets/sass/layout/_bg.scss
2020-02-05 18:39:21        812 assets/sass/layout/_footer.scss
2020-02-05 18:39:21       5134 assets/sass/layout/_header.scss
2020-02-05 18:39:21       2623 assets/sass/layout/_main.scss
2020-02-05 18:39:21        718 assets/sass/layout/_wrapper.scss
2020-02-05 18:39:21        787 assets/sass/libs/_functions.scss
2020-02-05 18:39:21       1801 assets/sass/libs/_mixins.scss
2020-02-05 18:39:21      16468 assets/sass/libs/_skel.scss
2020-02-05 18:39:21        867 assets/sass/libs/_vars.scss
2020-02-05 18:39:21       1122 assets/sass/main.scss
2020-02-05 18:39:21        348 assets/sass/noscript.scss
```

11. Enable the S3 bucket for hosting a static website:
`aws s3 website s3://<UNIQUE_BUCKET_NAME> --index-document index.html --error-document error/index.html`

```sh
[ec2-user@ip-10-0-0-233 static-website-example-master]$ aws s3 website s3://lab-bucket-8882 --index-document index.html --error-document error/index.html
```
If you receive no output from the command it means that it worked.

12. In a new browser tab, navigate to your bucket at the following address: `http://<UNIQUE_BUCKET_NAME>.s3-website-us-west-2.amazonaws.com`
  - `http://lab-bucket-8882.s3-website-us-west-2.amazonaws.com`

Access is denied, unless you explicitly specify all the files you send to a bucket have a public read access.



13. Upload the files again, overwriting the current copies, and set the access control list (ACL) to be public-read:
`aws s3 cp . s3://<UNIQUE_BUCKET_NAME> --recursive --acl public-read`

```sh
[ec2-user@ip-10-0-0-233 static-website-example-master]$ aws s3 cp . s3://lab-bucket-8882 --recursive --acl public-read
upload: ./README.MD to s3://lab-bucket-8882/README.MD
upload: assets/css/main.css to s3://lab-bucket-8882/assets/css/main.css
upload: ./LICENSE.MD to s3://lab-bucket-8882/LICENSE.MD
upload: assets/js/skel.min.js to s3://lab-bucket-8882/assets/js/skel.min.js
upload: assets/js/util.js to s3://lab-bucket-8882/assets/js/util.js
...
```

All the files are copied with a public ACL associated with the files/objects.

14. Refresh your browser tab that is pointing to the bucket.

Now that public read access has been set, the website can load successfully.



**Summary**

In this Lab Step, you used the AWS CLI to work with the Amazon S3 service. You created a bucket, uploaded files, set
the bucket for website hosting, and configured public read access to all the files/objects in the bucket. Because the
AWS CLI follows the same command service patterns, you are well-prepared to use the AWS CLI with any other service.

**Challenge (Optional)**

If you have time remaining in your Lab session, you are encouraged to explore a bit more.  The copy (cp) command you
used in the Lab is very useful, however, there is a better tool for keeping files in sync. Use the help tool with the

aws s3 command and look for the documentation of the sync command and try it out.

`aws s3 sync help`

`aws s3 sync . s3://lab-bucket-8882 --acl public-read`
