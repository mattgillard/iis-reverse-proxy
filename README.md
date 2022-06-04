# Setting up IIS to be a reverse proxy

## Creating Configuration Files (snapshot of IIS config)
The configuration files administration.config and applicationHost.config are generated as part of the command from a pre-configured IIS server:

`Export-IISConfiguration -PhysicalPath "C:\users\admin\desktop\iis" -DontExportKeys -Force`

## Import IIS config into a new Windows Server

1. Make sure IIS feature is installed:

`Install-WindowsFeature -name Web-Server -IncludeManagementTools`

2. Make sure Application Request Routing (ARR) v3 is installed (https://www.microsoft.com/en-us/download/details.aspx?id=47333). This download includes all dependencies so no need to use the Web Platform Installer which is being [deprecated](https://docs.microsoft.com/en-us/answers/questions/429383/web-platform-installer-end-of-support-and-sunsetti.html). Note: more details here on ARR https://www.iis.net/downloads/microsoft/application-request-routing

3. To import config into IIS:

    1. Open IIS Manager 
    2. Navigate to Shared Configuration
    3. Click Enable Shared Configuration
    4. Enter path to above files
    5. Click Apply

4. Copy web.config to your default website - most likely c:\inetpub\wwwroot
5. Restart IIS

`iisreset /noforce`
