On linux and macOS, the credentials file is located at ~/.aws/credentials.
On windows the file is located at C:\Users\USERNAME\.aws\credentials.
cat ~/.aws/credentials
[default]
aws_access_key_id=*****
aws_secret_access_key=*******
aws_session_token=***********************
cat ~/.aws/config
[default]
region = us-east-1
output = json


###################
View the current configuration
aws configure list 
View all existing profile names
aws configure list-profiles
In case, you want to change the region in a given profile
aws configure set <parameter> <value>  --profile <profile-name>
aws configure set region us-east-1  

###################
Let the system know that your sensitive information is residing in the .aws folder
export AWS_CONFIG_FILE=~/.aws/config
export AWS_SHARED_CREDENTIALS_FILE=~/.aws/credentials

##################
Check the successful configuration of the AWS CLI, by running either of the following AWS command:
# If you've just one profile set locally
aws iam list-users
# If you've multiple profiles set locally
aws iam list-users --profile <profile-name>

####################
In the future, you can set a single value, by using the command, such as:

# Syntax
# aws configure set <varname> <value> [--profile profile-name]
 aws configure set default.region us-east-2


 ########################
 Azure portal issue:
 https://community.spiceworks.com/t/azure-ad-admin-center-invalid-link/950514
 