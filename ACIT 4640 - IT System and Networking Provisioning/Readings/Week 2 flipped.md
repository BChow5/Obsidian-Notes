### what is AWS
- AWS CLI is an open source too that enables interacting with AWS services using command line shell
- all IaaS AWS administration, management, and access functions in the AWS management console are available in the AWS API and CLI
- AWS CLI provides direct access to the public API of AWS
- AWS CLI version 2 is only available as a bundled installer

### Built in help command
- aws help
- e.g. aws ec2 help

## Configure the AWS CLI

For general use, the AWS CLI needs the following pieces of information:
- Access key ID
- Secret access key
- AWS Region
- Output format
 - use `aws configure` to set up cli installation 
- can import access keys using .csv file instead of aws configure
- `aws configure import --csv file://credentials.csv`

### IAM User
An **IAM user**:
- Lives **inside a single AWS account**
- Has **credentials** (password and/or access keys)
- Is granted **permissions** to use AWS services
Think of it like a **user account with specific rights** inside your AWS account.

#### What IAM Users Are Used For
IAM users are typically used to:
- Allow **humans** to sign in to the AWS Management Console
- Allow **applications or scripts** to access AWS via API/CLI
- Enforce **least-privilege access** (only what’s needed)

### directly editing config and credentials
To directly edit the `config` and `credentials` files, perform the following.
1. Create or open the shared AWS `credentials` file. This file is `~/.aws/credentials` on Linux and macOS systems, and `%USERPROFILE%\.aws\credentials` on Windows. For more information, see [Configuration and credential file settings in the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html).
2. Add the following text to the shared `credentials` file. Replace the sample values in the `.csv` file that you downloaded earlier and save the file.