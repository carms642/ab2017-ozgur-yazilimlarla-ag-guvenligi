# Ossec Server Kurulumu

Bu dokümanda Ossec Manager uygulaması nasıl kurulur ve bazı konfigürasyon ayarları nasıl yapılırdan bahsedeceğim. Kurulum için **VMware Player** sanallaştırma uygulamasını ve işletim sistemi olarakta [***Ubuntu Server 16.04.1 LTS***](https://www.ubuntu.com/download/server/thank-you?version=16.04.1&architecture=amd64) sürümünü kullanacağım.


Java 8 Kurulumu

su -
echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main" | tee /etc/apt/sources.list.d/webupd8team-java.list
echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main" | tee -a /etc/apt/sources.list.d/webupd8team-java.list
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886
apt-get update
apt-get install oracle-java8-installer
exit
sudo apt-get install oracle-java8-set-default
