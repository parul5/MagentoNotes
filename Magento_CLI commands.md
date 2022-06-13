source ~/.bashrc (Update bashrc)
magento-cloud {
project:info
project:clear-build-cache //clear project build cache
project:get //clone project
project:info //read or set properties for project
project:list
project:set-remote
db:sql //To load connection details
local:build
local:clean
}

## connect to sparta server

```
eval "$(ssh-agent)" && ssh-add ~/.ssh/id_rsa && ssh-add -l && ssh -L 5000:web.us-west-2.prd.sparta.ceng.magento.com:22 web.us-west-2.prd.sparta.ceng.magento.com
```

## Taking dumps

- mkdir <directory_name>
- cd <directory_name>
- cloud-teleport <project_ssh_address> dump
- m2install.sh

## temporary fix for wsl2 network problem in ssh

```
Get-NetAdapter | Where-Object {$_.InterfaceDescription -Match "Hyper"} | Set-NetIPInterface -InterfaceMetric 1
Get-NetAdapter | Where-Object {$_.InterfaceDescription -Match "PANGP"} | Set-NetIPInterface -InterfaceMetric 6000Get-NetIPInterface -InterfaceAlias "Wi-Fi" | Set-NetIPInterface -InterfaceMetric 8000
Get-NetIPInterface -InterfaceAlias "Ethernet" | Set-NetIPInterface -InterfaceMetric 8500
```

## docker commands

- docker pull <repo_name>
- docker run -t -d --name <container_name_you_want_to_give> <image_name_that_you_pulled> (-d runs container in detached which kind of means in background)
- docker ps (for list of running containers)
- docker exec -it <container_name> <command>
- docker stop <container_name_OR_container_id>
- docker rm <container_name>
- docker images
-

## fix wsl logon failure

Run in powershell(Administrator)

> Get-Service vmcompute | Restart-Service

## See applied patches

php ./vendor/bin/ece-patches status
