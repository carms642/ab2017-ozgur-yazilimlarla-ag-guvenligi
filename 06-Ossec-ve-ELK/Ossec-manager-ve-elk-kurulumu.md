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
* Sh dosyasını çalıştırdığınızda hangi dilde kurulum yapmak istediğinizi soracaktır. ENTER tuşuna basarak varsayılan dil İngilizce ile devam ediyoruz.
* Bir sonraki adımda aşağıdaki gibi bir yazı içinde sistem bilgileriniz, kullanıcınız ve makinenin hostname'i yazacaktır. Devam etmek için ENTER tuşuna basınız.

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
```

* Bu adımda kurulum yapılacağı yeri sormaktadır. ENTER tuşuna basarak varsayılan konumu seçiyoruz.

```
2- Setting up the installation environment.

 - Choose where to install the OSSEC HIDS [/var/ossec]: 

```
* Bu adımda e-mail isteyip, istemediğimizi soruyor. Bu kurulumda ben e-mail istemediğim için "n" yazarak devam ediyorum.
```
3- Configuring the OSSEC HIDS.

  3.1- Do you want e-mail notification? (y/n) [y]: n
```
* Bu adımda dosya bütünlük kontrolü yapılacak mı diye sormaktadır. ENTER tuşu ile varsayılan evet ile devam ediyoruz.

```
3.2- Do you want to run the integrity check daemon? (y/n) [y]: 
```
* "Rootkit algılama altyapısını çalıştırmak istiyor musunuz?", sorusuna 'y' yazıp devam ediyoruz.
```
3.3- Do you want to run the rootkit detection engine? (y/n) [y]: 
```
* "Aktif yanıtı etkinleştirmek istiyor musunuz?" sorusuna bu kurulum için "n" yazıp devam ediyoruz.
```
3.4- Active response allows you to execute a specific 
       command based on the events received. For example,
       you can block an IP address or disable access for
       a specific user.  
       More information at:
       http://www.ossec.net/en/manual.html#active-response
       
   - Do you want to enable active response? (y/n) [y]: n
```
* "Uzak syslog'u etkinleştirmek istiyor musunuz?" sorusuna 'y' yazıp devam ediyoruz.
```
  3.5- Do you want to enable remote syslog (port 514 udp)? (y/n) [y]: 
```
* Bu adımda ossec analiz edebileceği log dosyalarını tespit eder. Eğer takip etmesini istediğiniz log dosyası belirtilen listede bulunmuyorsa ossec.conf dosyasına ekleme yapmanız gerekmektedir.(Bknz: ) ENTER ile devam ediyoruz.

```
3.6- Setting the configuration to analyze the following logs:
    -- /var/log/auth.log
    -- /var/log/syslog
    -- /var/log/dpkg.log

 - If you want to monitor any other file, just change 
   the ossec.conf and add a new localfile entry.
   Any questions about the configuration can be answered
   by visiting us online at http://www.ossec.net .
   
   
   --- Press ENTER to continue ---

```
* Eğer aşağıdaki hatayı alıyorsanız, bu hata ```build-essential``` paketi ile ilgilidir. İlk adımda bu paketi kurduğunuzdan eminseniz oturumu kapatıp, yeniden oturum açmayı deneyin. Alternatif çözüm olarak paketi yeniden kurmayı deneyebilirsiniz. 
```
Error 0x5.
 Building error. Unable to finish the installation.
```

* Ossec uygulaması başlatmak için aşağıdaki komutu çalıştırınız.
```
# /etc/init.d/ossec start
```
* Ossec ile ELK uygulamalarını ilişkilendirebilmek için ossec.conf dosyasında json_output'u aktif etmemiz gerekmektedir. Bunun için herhangi bir editörde ```/var/ossec/etc/ossec.conf``` dosyasını açmalıyız.
```
# nano /var/ossec/etc/ossec.conf 

<ossec_config>
  <global>
    <jsonout_output>yes</jsonout_output>
    <email_notification>no</email_notification>
  </global>
  ....
</ossec_config>  

```
* Ossec için yaptığımız config'in geçerli olması için uygulamayı yeniden başlatıyoruz.
```
# /etc/init.d/ossec restart
```

Ossec manager kurulumunu tamamladık. ELK uygulamalarını kurabilmemiz için öncelikle sistemimize Java kurulumu yapmamız gerekmektedir. Eğer sisteminizde Java kurulu ise bu adımı atlayabilirsiniz.

### Java kurulumu

```
# echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main" | tee /etc/apt/sources.list.d/webupd8team-java.list
# echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main" | tee -a /etc/apt/sources.list.d/webupd8team-java.list
# apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886
# apt-get update
# apt-get install oracle-java8-installer
```

## Elasticsearch kurulumu
```
# wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
# echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
# apt-get update
# apt-get install elasticsearch
# /bin/systemctl daemon-reload
# /bin/systemctl enable elasticsearch.service
# service elasticsearch start
```

## Logstash Kurulumu

* Logstash'i tar.gz dosyasını indirip kuracağız. Bunun için aşağıdaki adımları uygulayınız.
```
# wget https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.tar.gz
# tar -xzf logstash-5.2.0.tar.gz 
# cd logstash-5.2.0/
```
* Logstash ile ossec'den verileri alıp Elasticsearch'e yazmak ve bazı alanların isimlerini güncellemek için herhangi bir editör ile siem.conf adında bir dosya oluşturuyoruz.
```
~/logstash-5.2.0# nano siem.conf
```
Dosya içeriği olarak [siem.conf](siem.conf.sample) örneğindeki configler ile doldurunuz.

* Logstash'i siem.conf ile birlikte çalıştırmak için aşağıdaki komutu çalıştırınız.
```
~/logstash-5.2.0# bin/logstash -f siem.conf
```
* Logstash kurulumunu tamamladık. Eğer bu zamana herşey yolunda gitmişse Ossec alert üretmeye başlamış ve Logstash'te bu verileri Elasticsearch'e yazmaktadır.

## Kibana Kurulumu

* Kibana kurulumunu .deb paketi ile yapacağız. Bunun için aşağıdaki adımları uygulayınız.
```
# wget https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-amd64.deb
# dpkg -i kibana-5.2.0-amd64.deb 
```
* Kibana arayüzüne erişebilmemiz için kibana.yml'de server.host değerini makinemizin IP adresi ile güncellemeliyiz.

```
# nano /etc/kibana/kibana.yml


......
# Specifies the address to which the Kibana server will bind. IP addresses and host names are both valid values.
# The default is 'localhost', which usually means remote machines will not be able to connect.
# To allow connections from remote users, set this parameter to a non-loopback address.
server.host: "192.168.82.70"
......

```
Kurulumları tamamladık. Tarayıcıdan http://ossec-makinesini-ip-adresi:5601 adresine gittiğimizde Configure an index pattern sayfası karşılayacaktır. Index name or pattern alanına ```ossec-*``` değerini yazdıp kaydettiğimizde sistemimiz kullanıma hazır hale gelecektir.
