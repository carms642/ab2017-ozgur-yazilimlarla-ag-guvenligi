# Ossec Manager ve ELK Kurulumu

Merhabalar, dokümanda Ossec Manager, Elasticsearch, Logstash ve Kibana kurulumlarını anlatmaya çalışacağım. Bu dokümanda bu uygulamaların hepsi tek bir makineye kurulacaktır. Production ortamına kurulum yapıyorsanız performans açısından uygulamaları farklı makinelere kurmanınızı öneririm.  

Dokümanda işletim sistemi olarak [***Ubuntu Server 16.04.1 LTS***](https://www.ubuntu.com/download/server/thank-you?version=16.04.1&architecture=amd64) sürümünü kullanılmıştır.

* Git ve Build Essential paketlerin yüklenmesi
```
sudo apt-get -y install git build-essential
```
_Paketler yüklendikten sonra aktif olan oturumunuzu kapatıp yeniden oturum açınız._

* Ossec uygulamasının Github'dan stable versiyonunu çekmek

Aşağıdaki işlemleri root kullanıcısı ile yapınız. root kullanıcısına geçmek için: ```sudo su``` komutunu kullanabilirsiniz.

```
# git clone https://github.com/ossec/ossec-hids -b stable
```
* Ossec kurulumu için sh dosyasını çalıştırma

```
# cd ossec-hids/
~/ossec-hids# bash install.sh 
```
* Sh dosyasını çalıştırdığınızda aşağıdaki gibi bir yazı içinde sistem bilgileriniz, kullanıcınız ve makinenin hostname'i yazacaktır. Devam etmek için ENTER tuşuna basınız.

```
 OSSEC HIDS v2.8 Installation Script - http://www.ossec.net
 
 You are about to start the installation process of the OSSEC HIDS.
 You must have a C compiler pre-installed in your system.
 
  - System: Linux ossec-manager 4.4.0-31-generic
  - User: root
  - Host: ossec-manager


  -- Press ENTER to continue or Ctrl-C to abort. --
```
* Bu dokümanda kuracağımız ossec bizim için manager uygulama olacağı için kurulum türü olarak _server_ yazıyoruz. 
```
1- What kind of installation do you want (server, agent, local, hybrid or help)? server



root@ossec-manager:~/ossec-hids# nano /var/ossec/etc/ossec.conf 

    <jsonout_output>yes</jsonout_output>


root@ossec-manager:~/ossec-hids# /etc/init.d/ossec start



```
## Java kurulumu

```
echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main" | tee /etc/apt/sources.list.d/webupd8team-java.list
echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main" | tee -a /etc/apt/sources.list.d/webupd8team-java.list
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886
apt-get update
apt-get install oracle-java8-installer
```

## Elasticsearch kurulumu
```

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
sudo apt-get update
sudo apt-get install elasticsearch
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable elasticsearch.service
sudo service elasticsearch start
```

## Logstash Kurulumu

```
root@ossec-manager:~$ wget https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.tar.gz
root@ossec-manager:~$ tar -xzf logstash-5.2.0.tar.gz 
root@ossec-manager:~$ cd logstash-5.2.0/
root@ossec-manager:~/logstash-5.2.0$ nano siem.conf
root@ossec-manager:~/logstash-5.2.0# bin/logstash -f siem.conf
```



## Kibana Kurulumu

```
wget https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-amd64.deb
sinan@ossec-manager:~$ sudo dpkg -i kibana-5.2.0-amd64.deb 
nano /etc/kibana/kibana.yml


```
