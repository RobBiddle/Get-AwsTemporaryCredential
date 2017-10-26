# Get-AwsTemporaryCredential
PowerShell cmdlet to aquire temporary AWS account credentials via an IAM role

## DESCRIPTION
Retrieves AWS Credentials from a stored profile and uses these to obtain temporary credentials for the specified AWS account, using the specified IAM Role.

If a profile name is not specified you will be prompted via an Out-Grid GUI selection box to pick from a list of your stored profiles.
If no profile is found, instructions for creating one will be printed to the screen.

### Parameters
- __AccountNumber__ (This is the AWS Account Number)
- __Alias__ (Human friendly name for the account)
- __AwsProfileName__ (Name of the stored AWS Credential Profile to use)
- __Region__ (Region used by accunt.  If more than one Region is in use, run this cmdlet multiple times, once for each account/region pair )
- __IamRole__ (This is the name of the AWS IAM Role to use with this account)
### EXAMPLE
```PowerShell
    Get-AwsTemporaryCredential -AccountNumber 111111111111 -Alias ExampleFriendlyName -Region us-east-1 -IamRole ExampleRole -AwsProfileName ExampleProfileName ```
