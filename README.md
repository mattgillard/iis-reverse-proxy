# Setting up IIS to be a reverse proxy

This is a basic configuration to setup IIS as a reverse proxy on the IIS Default Web site. All it does it forward all traffic to a downstream host and strip the HTTP_ACCEPT_ENCODING HTTP header to ensure responses are not compressed so all FQDN's in the responses match the proxy host's FQDN so all links work correctly.

This might not suit all requirements and your mileage may vary.  This was designed as a frontend for SSRS hosted on [AWS RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.SQLServer.Options.SSRS.html) to avoid the need for installing RDS Root CA's on client devices to access reports.  AWS do not support custom FQDN's for SSRS as of June 2022 so this works around that limitation.

## Creating Configuration Files (snapshot of IIS config)
The configuration files administration.config and applicationHost.config are generated as part of the command from a pre-configured IIS server:

`Export-IISConfiguration -PhysicalPath "C:\users\admin\desktop\iis" -DontExportKeys -Force`

## Import IIS config into a new Windows Server

1. Make sure IIS feature is installed:

`Install-WindowsFeature -name Web-Server -IncludeManagementTools`

2. Make sure Application Request Routing (ARR) v3 is installed (https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ARRv3_0). This download includes all dependencies.  Note: more details here on ARR https://www.iis.net/downloads/microsoft/application-request-routing

3. To import config into IIS:

    1. Open IIS Manager 
    2. Navigate to Shared Configuration
    3. Click Enable Shared Configuration
    4. Enter path to above files
    5. Click Apply

4. Copy web.config to your default website - most likely c:\inetpub\wwwroot
5. Modify web.config to your needs. You will need to alter at least line 11,20,21 to provide your backend URL + rewrite correctly for your front facing website FQDN.
6. Restart IIS

`iisreset /noforce`

## Reference:

Inspired from: https://techcommunity.microsoft.com/t5/iis-support-blog/setup-iis-with-url-rewrite-as-a-reverse-proxy-for-real-world/ba-p/846222
