# Thanks for trying Chocolatey For Business!

This system has been pre-configured as a fully functioning C4B environment.

<!-- TOC -->

- [Create a license package](#create-a-license-package)
- [Enable Central Management](#enable-central-management)
- [Server Information](#server-information)
  - [Nexus Repository](#nexus-repository)
    - [**Changing the API Key**](#changing-the-api-key)
    - [Choco apikey Command](#choco-apikey-command)
  - [Jenkins](#jenkins)
  - [Chocolatey Central Management](#chocolatey-central-management)
  - [Firewall ports](#firewall-ports)
  - [Browser considerations](#browser-considerations)
  - [SSL Information](#ssl-information)
- [Client installations](#client-installations)
- [Licensing this VM](#licensing-this-vm)
- [Internalization](#internalization)

<!-- /TOC -->

## Create a license package

___

To leverage all of the features of C4B, copy the license file you received via email to C:\ProgramData\chocolatey\license. Make sure the name of the file is exactly `chocolatey.license.xml`

In Powershell execute:

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force ; . 'C:\choco-setup\files\CreateLicensePackage.ps1'
```

This will create the licensed package at C:\choco-setup\packages and push it up to your Nexus repository for use.

## Enable Central Management

___

**NOTE:** This step should _only_ be completed once the license package has been created in the step above. All licensed features are already installed, just unactivated without a valid license file.

Run the following to turn on the Central Management services.

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force ; . 'C:\choco-setup\files\EnableCCM.ps1'
```

## Server Information

___

### Nexus Repository

- Url: [https://chocoserver:8443](https://chocoserver:8443)
- Username: admin
- Password: [REDACTED]
- API Key: [REDACTED]

When you first log in to Nexus, you will immediately be asked you change your password.
You will then be asked if you'd like to enable Anonymous Access to the repositories. We typically recommend doing this, unless security requirements in your organization stipulate that RBAC controls be in place. **NOTE**: If you plan to allow clients to connect in from outside your network (over the internet), please contact support for the right options as there is more work to be done to limit access to specific repositories.

Sources configured in Chocolatey can only read data from their remote endpoints, and cannot delete items. If you need to limit the packages people have access to, control this with separate Hosted and Group repositories. Reach out to Chocolatey Support or consult the Nexus documentation for more information.

#### **Changing the API Key**

You may wish to change the API key before you start using things. To do so, log in to Nexus using the information above, or your new credentials if you have already gone through the first run wizard. Once logged in perform the following steps:

1. Click on your username in the upper right-hand side of the homepage.
2. Select "NuGet API Key" from the left-hand navigation window.
3. Select "Reset API key"
4. Enter your password
5. Take note of the new API key

If you change your API key, you will need to change the key in the Jenkins jobs that are pre-configured for you. See the next section for information on how to connect to Jenkins.

#### Choco apikey Command

To help make pushing packages easier, the `choco apikey` command is available. This will store your API key for a specific source as part of Chocolatey's configuration. This will be encrypted. To setup, do the following:

```powershell
choco apikey add --key="'$YourApiKey'" --source="'https://chocoserver:8443/repository/ChocolateyInternal/'"
```

### Jenkins

- Url: [http://chocoserver:8080](http://chocoserver:8080)
- Username: admin
- Password: [REDACTED]

For using Jenkins, please refer to our documentation here: [https://chocolatey.org/docs/how-to-setup-internal-package-repository](https://chocolatey.org/docs/how-to-setup-internal-package-repository). At most, you will need to login to Jenkins, change the password (`By going to People on the Sidebar > Click on admin > Click Configure on the Sidebar, scroll down to change password section`), and enable the pre-configured jobs to run on the schedule of your choosing. Our documentation can assist with ensuring this is done correctly.

### Chocolatey Central Management

- Url: [https://chocoserver](https://chocoserver)
- Username: ccmadmin
- Password: 123qwe


**NOTE**: You will see 2 packages (aspnetcore-runtimepackagestore and dotnetcore-windowshosting) listed as outdated at version 2.2.7. These packages have been pinned to that version, as they are required at that level for CCM to function.


### Firewall ports

To allow access to all services firewall ports have been opened as follows:

8443: Nexus WebUI
8081: Jenkins WebUI
443: Central Management WebUI
24020: Central Management Service communications for Agent check-in

### Browser considerations

We recommend you use Google Chrome to interact with all Web interfaces for the different services installed. You will find Google Chrome pre-installed in the environment.

### SSL Information

All services have been protected with Self-Signed SSL certificates and are placed in the appropriate stores. Under the following situations you would want to run the script that follows:
* If you want to expose this to the internet so clients can connect from outside your network
* If you change the hostname of this server
* If you add the QDE to a domain
* If you would like to use your own SSL/TLS certificates


```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force ; . C:\choco-setup\files\New-SslCertifcates.ps1
```

> :warning: **WARNING**: This script will seemingly prompt for input, and have other strange output. This is due to poor Java tooling and console output which cannot be suppressed. Just let things happen, as things are working as expected.
Once complete, this script will generate new SSL certificates for all services and move them to the appropriate locations and configure the services to use them.

___

## Client installations

___

To configure a client in your environment run the following in an administrative PowerShell console:

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force ; iex ((New-Object System.Net.WebClient).DownloadString('https://chocoserver:8443/repository/choco-install/ClientSetup.ps1'))
```

This script will :

- Install Chocolatey
- License Chocolatey
- Install the licensed extension (without the PackageBuilder/Internalizer shims)
- Install the licensed agent
- Configure ChocolateyInternal source
- Configure Self-Service mode
- Configure Central Management check-in

## Licensing this VM

___

This VM is running an **UNACTIVATED** Server 2019 Standard Operating System. If you plan to use this box long-term, you _will_ need to apply a license to the VM. If you use a KMS server in your environment, and have it configured on clients via Group Policy, you likely have nothing to do here, but verify.

If you rely on Retail or MAK licensing, you will need to apply the license using the following, replacing the x's with your actual product key:

```powershell
slmgr.vbs /ipk xxxxx-xxxxx-xxxxx-xxxxx
```

## Internalization

___

Chocolatey For Business includes the Package Internalizer feature, which takes a package from the Community Repository and rewrites the package to include all the binaries necessary to complete the installation of the application. You'll find in the C:\choco-setup\files directory a script named `Invoke-ChocolateyInternalizer.ps1` to help you with the process of internalizing additional packages into your environment.

The script accepts an array of Packages, your Local Nexus repository URL, the remote URL to look to for internalization, and your Nexus repository API key.

Example Usage:

```powershell
. C:\choco-setup\files\Invoke-ChocolateyInternalizer.ps1 -Packages adobereader,vlc,vscode -RepositoryUrl https://chocoserver:8443/repository/ChocolateyTest/ -RemoteRepo https://chocolatey.org/api/v2 -LocalRepoApiKey [REDACTED_API_KEY]
```