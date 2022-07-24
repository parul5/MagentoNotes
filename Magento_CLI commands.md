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

ssh web.eu-west-1.prd.sparta.ceng.magento.com
```

## Taking dumps

- mkdir <directory_name>
- cd <directory_name>
- cloud-teleport <project_ssh_address> dump
- m2install.sh (--source composer --version 2.4.1 --b2b --ee --sample-data yes --php php74 --es-host services --es-port 9200)
- 
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

## Check versions before sending upgrade tickets to platform

magento version

```
php bin/magento --version
```

elasticsearch version

```
php bin/magento config:show catalog/search/engine
curl -s -XGET 'http://localhost:9200'
```

mysql version

```
mysql -V
```

## Check top IPs for high usage

```
cat /var/log/platform/gm64lkzs2f6jy/access.log | awk '{print $1}'| sort | uniq -c | sort -nr | head -20
```

<!-- change head value to change number of IPs -->

## Get list of environments

magento-cloud env:list -p <projectID>

## Get admin uri

bin/magento info:adminuri

## Use merchant's DB

export DB_NAME=$(grep [\']db[\'] -A 20 app/etc/env.php | grep dbname | head -n1 | sed "s/.*[=][>][ ]*[']//" | sed "s/['][,]//");
export MYSQL_HOST=$(grep [\']db[\'] -A 20 app/etc/env.php | grep host | head -n1 | sed "s/.*[=][>][ ]*[']//" | sed "s/['][,]//");
export DB_USER=$(grep [\']db[\'] -A 20 app/etc/env.php | grep username | head -n1 | sed "s/.*[=][>][ ]*[']//" | sed "s/['][,]//");
export MYSQL_PWD=$(grep [\']db[\'] -A 20 app/etc/env.php | grep password | head -n1 | sed "s/.*[=][>][ ]*[']//" | sed "s/[']$//" | sed "s/['][,]//");
mysql -h $MYSQL_HOST -u $DB_USER --password=$MYSQL_PWD $DB_NAME

## check disk space for mysql

df -h

## Check fastly service ID

cat /mnt/shared/fastly_tokens.txt |grep ID
php bin/magento config:show system/full_page_cache/fastly/fastly_service_id

## check service relationships

### SSH environment
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp

### Direct through magento-cloud
```
magento-cloud repo:cat .magento/services.yaml -p <projectID> -e <env>
```

# Version check
curl localhost:5672
dpkg -s rabbitmq-server | grep Version
 
 ## command test
/usr/lib/rabbitmq/bin/rabbitmqctl list_queues
## プラグインの確認
/usr/lib/rabbitmq/bin/rabbitmq-plugins list  
 
## Tunnel SSH from Local.
# They can access the RabbitMQ WebUI by creating the following tunnel
ssh -N -L localhost:8008:localhost:15672 <ssh_url> -v
 
## RabbitMQのトンネルSSH接続
Here is the example for accessing the RabbitMQ management web interface.
Then you can browse to RabbitMQ panel from your local PC.
### 本番ステージングの場合
$ ssh -N -L localhost:8008:localhost:15672 1.ent-xdcriuipudt3u-staging-5em2ouy@ssh.us-a1.magento.cloud -v
### 開発環境の場合
$ ssh -N -L localhost:8008:rabbitmq.internal:15672 43q6nqpwpga3g-integration2-hohc4oi--mymagento@ssh.us-5.magento.cloud -v
### ポート指定でブラウズ
http://localhost:8008/
 
 
# log check
kz3c4dqu3ohd4@i-04fbfc58f215c7b43:/var/log/rabbitmq$ ls -al
total 5784
drwxr-xr-x  3 rabbitmq adm     4096 Jan 23 00:25 .
drwxr-xr-x 27 root     root    4096 Jan 23 03:25 ..
drwxr-sr-x  2 rabbitmq adm     4096 Jan 23 00:00 log
-rw-r--r--  1 rabbitmq adm  3792574 Jan 23 06:04 rabbit@host1.log
 
# CRON設定確認
$ bin/magento queue:consumers:list
vendor/magento/module-message-queue/etc/crontab.xml
<group id="consumers">
    <job name="consumers_runner" instance="Magento\MessageQueue\Model\Cron\ConsumersRunner" method="run">
        <schedule>* * * * *</schedule>
    </job>
</group>

## Check Indexer status
bin/magento indexer:status

## check redis memory
redis-cli -p 6370 info | egrep --color "(role|used_memory_peak_human|maxmemory_human|used_memory_peak_perc|evicted_keys|uptime_in_days)"


ssh -L 5672:localhost:5672 ssh 1.ent-qzbampqu5rxv2-staging-5em2ouy@ssh.eu-3.magento.cloud

## disable third party modules
php72 bin/magento module:status | grep -v Magento | grep -v List | grep -v None | grep -v -e '^$'| xargs php72 bin/magento module:disable
m2modoff