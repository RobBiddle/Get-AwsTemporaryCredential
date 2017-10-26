# Get-AwsTemporaryCredential
PowerShell cmdlet to aquire temporary AWS account credentials via an IAM role

## DESCRIPTION
Retrieves AWS Credentials from a stored profile and uses these to obtain temporary credentials for the specified AWS account, using the specified IAM Role.

If a profile name is not specified you will be prompted via an Out-Grid GUI selection box to pick from a list of your stored profiles.
If no profile is found, instructions for creating one will be printed to the screen.

### Table of Contents
- [How to Install](#Install)
- [How to Use](#Use)
  - [Parameters](#Parameters)
  - [Example](/#Example)
- [Contributing](#Contributing)
- [Support](#Support)
- [License](/LICENSE)


#### Install <a name="Install"></a>
```PowerShell
Install-Module Get-AwsTemporaryCredential
```

#### Use <a name="Use"></a>
```PowerShell
Import-Module Get-AwsTemporaryCredential
```

##### Parameters <a name="Parameters"></a>
- __AccountNumber__ (This is the AWS Account Number)
- __Alias__ (Human friendly name for the account)
- __AwsProfileName__ (Name of the stored AWS Credential Profile to use)
- __Region__ (Region used by accunt.  If more than one Region is in use, run this cmdlet multiple times, once for each account/region pair )
- __IamRole__ (This is the name of the AWS IAM Role to use with this account)
##### Example <a name="Example"></a>
```PowerShell
    $test = Get-AwsTemporaryCredential -AccountNumber 111111111111 -Alias ExampleFriendlyName -Region us-east-1 -IamRole ExampleRole -AwsProfileName ExampleProfileName
```
This above example will return a PSCustomObject with properties as follows:
```PowerShell
$ $test | gm

   TypeName: System.Management.Automation.PSCustomObject

Name          MemberType   Definition
----          ----------   ----------
Equals        Method       bool Equals(System.Object obj)
GetHashCode   Method       int GetHashCode()
GetType       Method       type GetType()
ToString      Method       string ToString()
AccountNumber NoteProperty string AccountNumber=111111111111
Alias         NoteProperty string Alias=ExampleFriendlyName
Credentials   NoteProperty Credentials Credentials=Amazon.SecurityToken.Model.Credentials
Region        NoteProperty string Region=us-east-1
RoleName      NoteProperty string RoleName=ExampleRole
```

The __Credentials__ property contains the _AccessKeyId_, _SecretAccessKey_ and _SessionToken_ which can be used with the AWSPowerShell cmdlets

```PowerShell
$ $test.Credentials | gm


   TypeName: Amazon.SecurityToken.Model.Credentials

Name            MemberType Definition
----            ---------- ----------
Equals          Method     bool Equals(System.Object obj)
GetCredentials  Method     Amazon.Runtime.ImmutableCredentials GetCredentials()
GetHashCode     Method     int GetHashCode()
GetType         Method     type GetType()
ToString        Method     string ToString()
AccessKeyId     Property   string AccessKeyId {get;set;}
Expiration      Property   datetime Expiration {get;set;}
SecretAccessKey Property   string SecretAccessKey {get;set;}
SessionToken    Property   string SessionToken {get;set;}
```

You would then utilize the credentials by referencing the object properties as follows:
```PowerShell
$test.Credentials.AccessKeyId
$test.Credentials.SecretAccessKey
$test.Credentials.SessionToken
```
I usually build these into a hashtable for easy splatting into AWSPowerShell cmdlets:
```PowerShell
$AwsTemporaryCredentialsForSplatting = @{
  AccessKey         = $test.Credentials.AccessKeyId
  SecretAccessKey   = $test.Credentials.SecretAccessKey
  SessionToken      = $test.Credentials.SessionToken
}
```
And splat the hashtable like this:
```PowerShell
Get-EC2Instance -Region us-east-1 @AwsTemporaryCredentialsForSplatting
```

##### Contributing <a name="Contributing"></a>
1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D

##### Support <a name="Support"></a>
- Please Star this repo if you found some of this code useful!
- If you're an unbelievably nice person and want to show your appreciation, I like beer ;-)
  - Send me beer money via LTC: MHJj5jaWFU2VeqEZXnLC4xaZdQ1Nu9NC48
  - Send me beer money via BTC: 38ieXk9rn2LJEsfimFWiyycUZZv5ABJPqM
  - Send me beer money via USD: https://paypal.me/RobertBiddle
