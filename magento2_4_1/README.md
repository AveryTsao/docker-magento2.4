# docker containers for magento 2.4.1 (open source)
### Docker containers for Magento 2.4.1 development including :

* averytsaowork/magento:2.4.1 ( php:7.4-apache / composer 1.10.22 )  
  
* averytsaowork/elasticsearch:7.7.1  
  
* PhpMyAdmin  
  
* mariadb:10.4.20  
  
* mailhog  
  
  
# Installation
### 1.install docker ( https://www.docker.com/ )
### 2.download this folder
### 3.you can edit config or not
* ./env  
  　　Environment Variables  
* ./magento2/auth.json  
  　　use mine key or yoursalf , see https://devdocs.magento.com/guides/v2.4/install-gde/prereq/connect-auth.html  
### 4.open a command line and cd to this folder path
### 5.Type the following commands in order on command line (This step may take a long time)
  * docker-compose build  
  
  * docker-compose up -d  
  
  * docker exec -it <averytsaowork/magento:2.4.1 container name> install-magento  
  
  * docker exec -it <averytsaowork/magento:2.4.1 container name> install-sampledata  
  
  * docker exec -it <averytsaowork/magento:2.4.1 container name> install-upgrade  
  
### 6.Disable 2FA ( if you don't want to do two factor auth )
  * docker exec -it <averytsaowork/magento:2.4.1 container name> bin/magento module:disable Magento_TwoFactorAuth  
  
### Sample bash shell
  * docker exec -it magento2_4_1_magento2_1 install-magento  
  
  * docker exec -it magento2_4_1_magento2_1 install-sampledata  
  
  * docker exec -it magento2_4_1_magento2_1 install-upgrade  
  
  * docker exec -it magento2_4_1_magento2_1 bin/magento module:disable Magento_TwoFactorAuth  
  
  
# Note (default .env)
### magento home page : http://127.0.0.1
### magento admin page : http://127.0.0.1/admin
  　　Username : admin / Password : magento2  
### mail : http://127.0.0.1:8025
  　　can accept magento admin auth mail when login magento admin page
### PhpMyAdmin : http://127.0.0.1:8080
  　　Username : root / Password : magento  
  
  　　Username : magento / Password : magento
