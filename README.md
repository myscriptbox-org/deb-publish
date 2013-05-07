# man page for deb-publish.
## NAME 
**deb-publish** manages a publication account on a remote server. The account holds one or more domains. Each domain holds one or more distributions. Each distribution holds one or more releases. Each release consists of one or more packages. Downloading from a distribution can be restricted to registered users only.

## SYNOPSIS
**deb-publish** [objectPath] [ objectId1 ... objectIdN ] -action [ actionArg1 .. actionArgM]

## DESCRIPTION 
Rationale in the data managed by **deb-publish**:**  
An account has a database with keys. Each key has a public and a private part. The account manager uses the private keys to sign packages  
An account has one or more domains.  
A domain has one public key. Users installing from the domain, will download this key.  
A domain has one or more distributions.  
A distribution has a list of registered users.  
A distribution has one or more releases.  
A release has one or more packages.  

### account
Use this object path to perform account-wide operations or to manage the remote gnupg key database. The remote gnupg database must contain the signing private key for verifying the signature of a package.

### account.key
Use this object path to manage a key, both the public and the private part, in the remote gnupg key database.

### account.prikey
Use this object path to manage just the private part of the key in the remote gnupg key database.

### account.pubkey
Use this object path to manage just the public part of the key in the remote gnupg key database.

### account.domain
Use this object path to manage a domain in a remote account.

### account.domain.distrib
Use this object path to manage a distribution in a remote account/domain.

### account.domain.distrib.package
Use this object path to manage a package in a remote account/domain/distribution across releases.

### account.domain.distrib.release
Use this object path to manage a release in a remote account/domain/distribution.

### account.domain.distrib.release.package
Use this object path to manage a package in a remote account/domain/distribution/release.

### account.domain.distrib.release.package.version
Use this object path to manage a package in a particular version in a remote account/domain/distribution/release.

### account.domain.distrib.user
Use this object path to manage registered users in a remote account/domain/distribution.

## OPTIONS

### --help
shows the general help paragraph.

### [objectpath] --help
shows general help for the object path.

### --version
shows the program's version.

### --license
shows the program's license.

### account [obj] -show-keys
This command shows all the keys in the account's key database. The private key is required for the purposes of signing packages. Example:  
**deb-publish** account john@doe.com -show-keys

### account [obj] -upload-prikey [arg]
This command uploads a private key file to the account's key database. Example:  ;
{command} account john@doe.com -upload-prikey mykey.gpg.pri  
The file extension does not matter. You can also supply the key on stdin by using filename '-'. Example:  
cat mykey.gpg.pri | {command} account john@doe.com -upload-prikey -  

### account [obj] -show-domains
This command lists the domains for an account. In each domain, the user can publish a reprepro repository. Example  
**deb-publish** account john@doe.com -show-domains

### account [obj] -fix-keydb-trust
This command sets the trust level of all keys to 6. Below that level, the keys are mostly unusable for signing purposes. Example:  
**deb-publish** account john@doe.com -fix-keydb-trust  

### account [obj] -upload-pubkey [arg]
This command uploads a public key file to the account's key database. Example:  
{command} account john@doe.com -upload-pubkey mykey.gpg.pub  
The file extension does not matter. You can also supply the key on stdin by using filename '-'. Example:  
cat mykey.gpg.pub | {command} account john@doe.com -upload-pubkey -  

### account.domain.arg [obj] -download-pubkey-to-gpg-man
This command downloads downloads a domain-level public key in your local gpg database:  
**deb-publish** account.domain john@doe.com garagesoft.com -download-pubkey-to-gpg-man

### account.domain.arg [obj] -upload-pubkey [args]
This command uploads a public key for a domain for an account. Synopsis:  
**deb-publish** account.domain [user@server] [domain] -upload-pubkey [keyfile] [saveAsFilename]  
Example:  
**deb-publish** account.domain john@doe.com garagesoft.com -upload-pubkey mykey.pub garagesoft.pub  
Note that the file extension does not matter. The _saveAsFilename_ parameter is optional. If it is not supplied, the system wil save the file under the same name as the _keyfile_ itself. You can also upload by supplying a file on stdin by specifying the argument '-' for the _keyfile_ parameter. Example:  
cat mykey.pub | **deb-publish** account.domain john@doe.com garagesoft.com -upload-pubkey - garagesoft.pub  
Again, the _saveAsFilename_ argument is optional. If you do not supply it, the system will save it as 'gpg.key' on the server.

### account.domain.arg [obj] -show-distribs
This command lists the distributions in a domain for an account. Example:  
**deb-publish** account.domain john@doe.com garagesoft.com -show-distribs

### account.domain.arg [obj] -delete-pubkey
This command deletes a domain-level public key. Example:  
**deb-publish** account.domain john@doe.com garagesoft.com -delete-pubkey

### account.domain.arg [obj] -exists
This command checks if a domain exists in the account:  
**deb-publish** account.domain john@doe.com garagesoft.com -exists  
The command outputs a 'yes' if it exists and a 'no' if it doesn't.


### account.domain.arg [obj] -upload-pubkey-from-gpg-man [args]
This command uploads a public key for a domain for an account, straight from your local gpg database. Synopsis:  
**deb-publish** account.domain [user@server] [domain] -upload-pubkey-from-gpg-man key-email saveAsFilename  
Example:  
**deb-publish** account.domain john@doe.com garagesoft.com -upload-pubkey-from-gpg-man packages@garagesoft.com garagesoft.pub  
The _saveAsFilename_ parameter is optional. If it is not supplied, the system wil save the file under the name _gpg.key_. There must be a public key available under the email _key-email_ in the gpg database.

### account.domain.arg [obj] -pubkey-exists
This command checks if a domain-level public key exists. Example:  
**deb-publish** account.domain john@doe.com garagesoft.com -pubkey-exists  
If the public key exists, the command outputs a 'yes'. Otherwise, a 'no'.

### account.domain.arg [obj] -download-pubkey
This command downloads a domain-level public key. Example:  
**deb-publish** account.domain john@doe.com garagesoft.com -download-pubkey

### account.domain.distrib.arg.arg [obj] -htaccess-exists
This command checks if the **.htaccess** file exists for a distribution in a particular account/domain. Example:  
**deb-publish** account.domain.distrib john@doe.com garagesoft.com ubuntu -htaccess-exists  
The command outputs 'yes' if the file exists and 'no', if it doesn't.

### account.domain.distrib.arg.arg [obj] -exists
This command checks if a particular distribution exists in a particular account/domain. Example:  
**deb-publish** account.domain.distrib john@doe.com garagesoft.com ubuntu -exists  
The command returns 'yes', if the distribution exists, and 'no', if it doesn't.

### account.domain.distrib.arg.arg [obj] -add
This command adds a distribution to an account/domain. Synopsis:  
**deb-publish** account.domain.distrib user@server domain distrib -add  
Example:  
**deb-publish** account.domain.distrib john@doe.com garagesoft.com ubuntu -add  

### account.domain.distrib.arg.arg [obj] -show-releases
This command lists the releases for a distribution in a particular account/domain. Example  :
**deb-publish** account.domain.distrib john@doe.com garagesoft.com ubuntu -show-releases

### account.domain.distrib.arg.arg [obj] -delete-users
This command deletes all the users for a particular account/domain. Example:  
**deb-publish** account.domain.distrib john@doe.com garagesoft.com ubuntu -delete-users  

### account.domain.distrib.arg.arg [obj] -restrict
This command restricts a distribution to registered users only for a distribution in a particular account/domain. All releases in that distribution will be restricted too. Example:  
{command} account.domain.distrib john@doe.com garagesoft.com ubuntu -restrict  
Only users with a valid username and password will be able to download from this distribution.

### account.domain.distrib.arg.arg [obj] -delete
This command deletes a distribution from an account/domain. Synopsis:  
**deb-publish** account.domain.distrib user@server domain distrib -delete  
Example:  
**deb-publish** account.domain.distrib john@doe.com garagesoft.com ubuntu -delete  

### account.domain.distrib.arg.arg [obj] -unrestrict
This command lifts the restriction to download from a particular distribution in a particular account/domain. It does not delete the user database. Example:  
**deb-publish** account.domain.distrib john@doe.com garagesoft.com ubuntu -unrestrict  
There is a separate command to delete the user database.

### account.domain.distrib.arg.arg [obj] -htpasswd-exists
This command checks if a **htpasswd** file exists for a particular distribution for a particular account/domain. Example:  
**deb-publish** account.domain.distrib john@doe.com garagesoft.com ubuntu -htpasswd-exists  
The command outputs 'yes' if the file exists and 'no', if it doesn't.
### account.domain.distrib.arg.arg [obj] -generate-reprepro-conf
This command regenerates the 'distributions' and 'override' configuration files for a distribution in a particular account/domain. This command is called automatically after adding or removing a release. Example:  
**deb-publish** account.domain.distrib john@doe.com garagesoft.com ubuntu -generate-reprepro-conf


### account.domain.distrib.arg.arg [obj] -show-users
This command lists the users registered for a particular distribution in a particular account/domain. Example:  
**deb-publish** account.domain.distrib john@doe.com garagesoft.com ubuntu -show-users

### account.domain.distrib.package.arg.arg.arg [obj] -show-releases
### account.domain.distrib.release.arg.arg.arg [obj] -control [arg]
This command updates the **releases** file in the **conf** folder in the reprepro distribution folder. This command is called automatically when adding or removing a release. Example:  
**deb-publish** account.domain.distrib.release john@doe.com garagesoft.com ubuntu precise -control add**
The **-control** action takes the argument **add** or **delete**. This step is will prepare the regeneration of the **conf/distribution** and **conf/override.*** files.

### account.domain.distrib.release.arg.arg.arg [obj] -show-packages
This command shows the packages in a particular account/domain/distribution/release. Example:  
**deb-publish** account.domain.distrib.release john@doe.com garagesoft.com ubuntu precise -show-packages
### account.domain.distrib.release.arg.arg.arg [obj] -delete
This command deletes a particular release in a particular account/domain/distribution. Example  :
**deb-publish** account.domain.distrib.release john@doe.com garagesoft.com ubuntu precise -delete

### account.domain.distrib.release.arg.arg.arg [obj] -exists
This command checks if a particular release exists in an account/domain/distribution. Example:  
**deb-publish** account.domain.distrib.release john@doe.com garagesoft.com ubuntu precise -exists  
If the release exists, the command outputs a 'yes'. Otherwise, a 'no'.

### account.domain.distrib.release.arg.arg.arg [obj] -add
This command adds a release to a particular account/domain/distribution. Synopsis:  
**deb-publish** account.domain.distrib.release user@server domain distrib release -add  
Example:  
**deb-publish** account.domain.distrib.release john@doe.com garagesoft.com ubuntu raring -add  
This example adds the 'raring' release to the 'ubuntu' distribution.


### account.domain.distrib.release.package.arg.arg.arg.arg [obj] -delete
This command deletes a package in an account/domain/distribution/release. Example:  
**deb-publish** account.domain.distrib.release.package john@doe.com garagesoft.com ubuntu precise mechanic-viewer -delete

### account.domain.distrib.release.package.arg.arg.arg.arg [obj] -exists
This command checks if a particular package exists in an account/domain/distribution/release. Example:  
**deb-publish** account.domain.distrib.release.package john@doe.com garagesoft.com ubuntu precise mechanics-viewer -exists  
The command returns 'yes' if the package exists in the account/domain/distribution/release or 'no', if it doesn't.

### account.domain.distrib.release.package.arg.arg.arg.arg [obj] -show
This command shows the distribution details for a package in a particular account/domain/distribution/release. Example:  
**deb-publish** account.domain.distrib.release.package john@doe.com garagesoft.com ubuntu precise mechanics-viewer -show  
Typical output:  
precise|main|i386: mechanics-viewer 1.0.3  
precise|main|amd64: mechanics-viewer 1.0.3  
precise|main|source: mechanics-viewer 1.0.3


### account.domain.distrib.release.package.version.arg.arg.arg.arg.arg [obj] -add [arg]
This command uploads a package to an account/domain/distribution/release for a particular version. Synopsis:  
**deb-publish** account.domain.distrib.release.package.version \
         user@server domain distribution release package version -add folder  
Example:  
**deb-publish** account.domain.distrib.release.package.version \
         john@doe.com garagesoft.com ubuntu raring mechanics-viewer 0.7.3 -add myfolder  
Use '.' as folder name for the uploading the current working folder:  
**deb-publish** account.domain.distrib.release.package.version \
         john@doe.com garagesoft.com ubuntu raring mechanics-viewer 0.7.3 -add .  
The folder should contain 4 files:  
**.deb**: This file is the binary package. The filename should be: package_version-release_all.deb  
**.changes**: This file is the binary control package, describing the binary package. The filename should be: package_version-release_amd64.dsc  
**.tar.gz**: This file is the source package. The filename should be: package_version-release.tar.gz  
**.dsc**: This file is the source control package, describing the source package. The filename should be: package_version-release.dsc  

### account.domain.distrib.user.arg.arg.arg [obj] -delete
This command deletes a user from an account/domain/distribution. Example:  
**deb-publish** account.domain.distrib.user john@doe.com garagesoft.com ubuntu carl -delete

### account.domain.distrib.user.arg.arg.arg [obj] -set-pwd [arg]
This command adds a user or updates his password in an account/domain/distribution. Example:  
**deb-publish** account.domain.distrib.user john@doe.com garagesoft.com carl -set-passwd 'adf34#$'  

### account.domain.distrib.user.arg.arg.arg [obj] -exists
This command checks if a user exists in an account/domain/distribution. Example:  
**deb-publish** account.domain.distrib.user john@doe.com garagesoft.com ubuntu carl -exists  
The command returns 'yes' if the user existes and 'no' if he doesn't.
### account.key.arg [obj] -download-to-gpg-man
This command downloads a key, both public and private parts, from the account's gnugpg database into the local gnupg key database. Example:  
**deb-publish** account.key john@doe.com info@garagesoft.com -download-to-gpg-man  
The key is represented by the second email address.

### account.key.arg [obj] -delete
This command deletes a key, both public and private parts, from the account's key database. Example:  
**deb-publish** key john@doe.com info@garagesoft.com -delete  
The key is represented by the second email address.

### account.key.arg [obj] -upload-from-gpg-man
This command uploads a key, both private and public parts, from the local gnupg key database into the remote account's database. Example:  
**deb-publish** account.key john@doe.com info@garagesoft.com -upload-from-gpg-man  
The key is represented by the second email address.

### account.prikey.arg [obj] -download
This command downloads a private key from a remote account's key database. Example:  
**deb-publish** account.prikey john@doe.com info@garagesoft.com -download > mykeyfile.pri  
You can choose any name for the 'mykeyfile.pri' argument.

### account.prikey.arg [obj] -exists
This command checks if a private key exists in a remote account's key database. Example:  
**deb-publish** account.prikey john@doe.com garagesoft.com -exists  
The command outputs 'yes' if the private key exists and 'no' if it doesn't.
The key is represented by the second email address.
### account.pubkey.arg [obj] -download
This command downloads a public key from the remote account's key database. Example:  
**deb-publish** account.pubkey john@doe.com info@garagesoft.com -download > myfile.pub  
You can choose any name for the 'myfile.pub' argument.

### account.pubkey.arg [obj] -exists
This command checks if a public key exists in a remote account's key database. Example:  
**deb-publish** account.pubkey john@doe.com garagesoft.com -exists  
The command outputs 'yes' if the public key exists and 'no' if it doesn't.
The key is represented by the second email address.

## ENVIRONMENT 
### GPG
By default, **deb-publish** will use the **gpg** command to manage the local and remote key databases. Use the _GPG_ environment variable to change this default. Example:  
GPG=altgpg **deb-publish** ...  

## EXIT-STATUS 
### 0
A zero exit code means that everything went ok.
### 1
A one exit code is usually an error generated by **deb-publish** itself.
### other
Another exit code is always an error generated by one of the underlying programs invoked by **deb-publish**.

## FILES 
### /etc/deb-publish-root.d/templates/distributions
This file is the template configuration file for a reprepro 'conf/distributions' file. In the lingo of **deb-publish** this file contains the 'releases'. It has the following template variables: {domain}, {distrib}, and {release}. The template is repeated for each release in the reprepro repository.

### /etc/deb-publish-root.d/templates/options
This file is the template configuration file for a reprepro 'conf/options' file. It has the following template variable: {rootpath}.

### /etc/deb-publish-root.d/templates/htaccess
This file the template apache configuration file for a restricted distribution. It has the following template configuration variables: {user}, {domain}, and {distrib}. In this context, 'user' means 'account'.

## AUTHOR
Erik Poupaert <erik@sankuru.biz>

## REPORTING-BUGS
Report bugs to: erik@sankuru.biz

# COPYRIGHT
Licensed under GPL
