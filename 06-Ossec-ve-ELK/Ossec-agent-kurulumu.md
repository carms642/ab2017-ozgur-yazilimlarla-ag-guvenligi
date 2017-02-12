# Ossec Agent Kurulumu

Merhabalar, bu dokümanda Ossec Agent kurulumunu ve manager ile bağlantısını anlatmaya çalışacağım. Ossec Agent kurulduğu makinedeki logları takip edip, manager'ın kurulu olduğu makineye gönderir.

Dokümanda işletim sistemi olarak [***Ubuntu Server 16.04.1 LTS***](https://www.ubuntu.com/download/server/thank-you?version=16.04.1&architecture=amd64) sürümünü kullanılmıştır.

_Aşağıdaki tüm kurulum adımları root kullanıcısı ile yapılacaktır. root kullanıcısına geçmek için ```sudo su``` komutunu kullanabilirsiniz._

* Git ve Build Essential paketlerin yüklenmesi
```
# apt-get -y install git build-essential
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
~/ossec-hids# sh install.sh 

  ** Para instalação em português, escolha [br].
  ** 要使用中文进行安装, 请选择 [cn].
  ** Fur eine deutsche Installation wohlen Sie [de].
  ** Για εγκατάσταση στα Ελληνικά, επιλέξτε [el].
  ** For installation in English, choose [en].
  ** Para instalar en Español , eliga [es].
  ** Pour une installation en français, choisissez [fr]
  ** A Magyar nyelvű telepítéshez válassza [hu].
  ** Per l'installazione in Italiano, scegli [it].
  ** 日本語でインストールします．選択して下さい．[jp].
  ** Voor installatie in het Nederlands, kies [nl].
  ** Aby instalować w języku Polskim, wybierz [pl].
  ** Для инструкций по установке на русском ,введите [ru].
  ** Za instalaciju na srpskom, izaberi [sr].
  ** Türkçe kurulum için seçin [tr].
  (en/br/cn/de/el/es/fr/hu/it/jp/nl/pl/ru/sr/tr) [en]: 


 OSSEC HIDS v2.8 Installation Script - http://www.ossec.net
 
 You are about to start the installation process of the OSSEC HIDS.
 You must have a C compiler pre-installed in your system.
 
  - System: Linux ubuntu 4.4.0-31-generic
  - User: root
  - Host: ubuntu


  -- Press ENTER to continue or Ctrl-C to abort. --


1- What kind of installation do you want (server, agent, local, hybrid or help)? agent

  - Agent(client) installation chosen.

2- Setting up the installation environment.

 - Choose where to install the OSSEC HIDS [/var/ossec]: 

    - Installation will be made at  /var/ossec .

3- Configuring the OSSEC HIDS.

  3.1- What's the IP Address or hostname of the OSSEC HIDS server?: 192.168.220.128

   - Adding Server IP 192.168.220.128

  3.2- Do you want to run the integrity check daemon? (y/n) [y]: y

   - Running syscheck (integrity check daemon).

  3.3- Do you want to run the rootkit detection engine? (y/n) [y]: y

   - Running rootcheck (rootkit detection).

  3.4 - Do you want to enable active response? (y/n) [y]: n

   - Active response disabled.

  3.5- Setting the configuration to analyze the following logs:
    -- /var/log/auth.log
    -- /var/log/syslog
    -- /var/log/dpkg.log

 - If you want to monitor any other file, just change 
   the ossec.conf and add a new localfile entry.
   Any questions about the configuration can be answered
   by visiting us online at http://www.ossec.net .
   
   
   --- Press ENTER to continue ---
   5- Installing the system
 - Running the Makefile
    CC external/cJSON/cJSON.o
    LINK libcJSON.a
ar: `u' modifier ignored since `D' is the default (see `U')
    RANLIB libcJSON.a
cd external/zlib-1.2.8/ && ./configure && make libz.a
Checking for gcc...
.............
.............
.............
 - System is Debian (Ubuntu or derivative).
 - Init script modified to start OSSEC HIDS during boot.

 - Configuration finished properly.

 - To start OSSEC HIDS:
      /var/ossec/bin/ossec-control start

 - To stop OSSEC HIDS:
      /var/ossec/bin/ossec-control stop

 - The configuration can be viewed or modified at /var/ossec/etc/ossec.conf


    Thanks for using the OSSEC HIDS.
    If you have any question, suggestion or if you find any bug,
    contact us at contact@ossec.net or using our public maillist at
    ossec-list@ossec.net
    ( http://www.ossec.net/main/support/ ).

    More information can be found at http://www.ossec.net

    ---  Press ENTER to finish (maybe more information below). ---
    


 - You first need to add this agent to the server so they 
   can communicate with each other. When you have done so,
   you can run the 'manage_agents' tool to import the 
   authentication key from the server.
   
   /var/ossec/bin/manage_agents

   More information at: 
   http://www.ossec.net/en/manual.html#ma

```
