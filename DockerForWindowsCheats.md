Docker Instalation Steps for Windows Production Enviroments

# Firewall Exceptions

All destinatons with 443 port

52.185.224.174
52.241.128.114
54.230.227.117
54.230.227.159
54.230.227.183
104.78.58.218
13.107.4.52
13.65.245.138
13.74.179.117
172.217.30.99
191.232.236.80
23.42.249.5
51.141.32.51
52.84.175.123
52.84.175.15
52.84.175.185
52.84.175.197
54.192.57.233
54.192.57.31
54.192.57.39
54.192.57.97
54.230.227.14
64.4.54.254
65.55.44.108

# Antivirus Exceptions

Program Files\Microsoft Service Fabric
FabricDataRoot (from cluster configuration)
FabricLogRoot (from cluster configuration)
Antivirus Excluded processes
Fabric.exe
FabricHost.exe
FabricInstallerService.exe
FabricSetup.exe
FabricDeployer.exe
ImageBuilder.exe
FabricGateway.exe
FabricDCA.exe
FabricFAS.exe
FabricUOS.exe
FabricRM.exe
FileStoreService.exe

Linux- /var/lib/docker
Windows Server- %ProgramData%\docker
Windows 10- %ProgramData%\docker and Autorun.inf
Mac- $HOME/Library/Containers/com.docker.docker/

# Docker Download

Invoke-WebRequest -UseBasicParsing -OutFile docker-18.09.5.zip https://download.docker.com/components/engine/windows-server/18.09/docker-18.09.5.zip

# Stop Docker service
Stop-Service docker
    
# Extract the archive.
Expand-Archive docker-18.09.5.zip -DestinationPath $Env:ProgramFiles -Force

# Clean up the zip file.
Remove-Item -Force docker-18.09.5.zip

# Install Docker. This requires rebooting.
$null = Install-WindowsFeature containers

# Add Docker to the path for the current session.
$env:path += ";$env:ProgramFiles\docker"

# Optionally, modify PATH to persist across sessions.
$newPath = "$env:ProgramFiles\docker;" +
[Environment]::GetEnvironmentVariable("PATH",
[EnvironmentVariableTarget]::Machine)

[Environment]::SetEnvironmentVariable("PATH", $newPath,
[EnvironmentVariableTarget]::Machine)

# Register the Docker daemon as a service.
dockerd --register-service

# Start the Docker service.
Start-Service docker

#Docker Module

Invoke-WebRequest -UseBasicParsing https://go.microsoft.com/fwlink/?LinkID=627338&clcid=0x409

#Pull Images

docker pull microsoft/dotnet-samples:dotnetapp-nanoserver-sac2016
docker pull microsoft/iis:latest
docker pull mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2016
docker pull microsoft/windowsservercore:latest
docker pull microsoft/aspnet:latest
docker pull microsoft/iis:nanoserver
docker pull mcr.microsoft.com/windows/servercore/iis:nanoserver-sac2016
docker pull microsoft/nanoserver:latest
docker pull microsoft/mssql-server-windows-developer:latest
docker pull microsoft/mssql-server-windows-express:latest

#Images list

microsoft/dotnet-samples                                         dotnetapp-nanoserver-sac2016   afca1083bf22        2 months ago        1.27GB
microsoft/iis                                                    latest                         9581d0bdc68b        3 months ago        11.3GB
mcr.microsoft.com/windows/servercore/iis                         windowsservercore-ltsc2016     9581d0bdc68b        3 months ago        11.3GB
microsoft/windowsservercore                                      latest                         ea9f7aa13d03        3 months ago        11GB
microsoft/aspnet                                                 latest                         ddabd3e10c02        5 months ago        13.7GB
microsoft/iis                                                    nanoserver                     d4d34a16ef9d        5 months ago        1.29GB
mcr.microsoft.com/windows/servercore/iis                         nanoserver-sac2016             d4d34a16ef9d        5 months ago        1.29GB
microsoft/nanoserver                                             latest                         4c872414bf9d        6 months ago        1.17GB
microsoft/mssql-server-windows-developer                         latest                         19873f41b375        15 months ago       15.1GB
microsoft/mssql-server-windows-express                           latest                         1986b8a8f950        15 months ago       13.8GB
