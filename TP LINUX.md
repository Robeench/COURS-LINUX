
INSTALLER  DEBIAN 
CONFIGURER ADRESSE IP : 192.168.1.245
Hosts : LAMPS
CONFIGURER LE SSH

DNS : lamps.batlan
Ajouter le fqdn dans le fichier host de windows : windows/service32/driver/etc/host

![[Pasted image 20230503110611.png]]

# Apache2

Création de la cle SSL via la ligne suivante :
**openssl req $@ -new -x509 -days 3650 -nodes -out /etc/apache2/apache.pem -keyout /etc/apache2/apache.pem**

Configurer les fichier virtualhost pour redirection http=> https


[![[Pasted image 20230503090349.png]]](https://github.com/Robeench/COURS-LINUX/blob/main/IMAGE/Pasted%20image%2020230503090349.png)

[![[Pasted image 20230503090542.png]]](https://github.com/Robeench/COURS-LINUX/blob/main/IMAGE/Pasted%20image%2020230503090542.png)

[![[Pasted image 20230503090832.png]]](https://github.com/Robeench/COURS-LINUX/blob/main/IMAGE/Pasted%20image%2020230503090832.png)

nano /etc/apache2/sites-available/site.conf
[![[Pasted image 20230503094338.png]]](https://github.com/Robeench/COURS-LINUX/blob/main/IMAGE/Pasted%20image%2020230503094338.png)


# Création d'une base de donnée
MYSQL
Installation de mysql8 en lieu et place de maria  
**wget [https://repo.mysql.com/mysql-apt-config_0.8.23-1_all.deb](https://repo.mysql.com/mysql-apt-config_0.8.23-1_all.deb)[  
](https://repo.mysql.com//mysql-apt-config_0.8.22-1_all.deb)dpkg -i mysql-apt-config_0.8.23-1_all.deb   
apt update  
apt upgrade  
apt install mysql-server**

  

_# test de mysql en cli  
_**mysql -u root -p**


Php 7.x
upload max
![[Pasted image 20230503100049.png]]

_# création d’un fichier test dans /home/user/www/  
__# nommé test.php avec le compte user  
_**su user  
nano /home/user/www/test.php**
![[Pasted image 20230503100226.png]]

Connexion via le navigateur :
https://192.168.1.245/phpmyadmin
Identification : root 
Mdp

![[Pasted image 20230503112704.png]]
Ajout Utilisateur et Base de donnée 
![[Pasted image 20230503113119.png]]

![[Pasted image 20230503153113.png]]

droit utilisateur sur base de donnée
![[Pasted image 20230503153210.png]]

Mettre un alias
nano /etc/apache2/conf-available/phpmyadmin.conf
![[Pasted image 20230503110435.png]]

Modifier les droit directement dans la base des données


Se connecter sur navigateur 
![[Pasted image 20230503110450.png]]

# FTP

![[Pasted image 20230503111141.png]]

Filezilla 
Renseigner les champs
Accepter le certificat
![[Pasted image 20230503111455.png]]

# Samba
_# install et config de samba (samba pré-installé en amont)  
_**mv /etc/samba/smb.conf /etc/samba/smb.bak
nano /etc/samba/smb.conf**

[global]
workgroup = BATLAN
netbios name = www
comment = Serveur de fichier de BATMAN pour le lamps
interfaces = ens192
encrypt passwords = true
security = user
smb passwd file = /usr/bin/smbpasswd
passdb backend = tdbsam
obey pam restrictions = yes

[www]
path = /home/tech/www
valid users = tech
writeable = Yes
create mask = 777
directory mask = 777




Pour tester s'il ya des erreurs
testparm
![[Pasted image 20230503114139.png]]









voir PASSWORD sur serrveur crée
cd /etc
cat passwd
![[Pasted image 20230503114027.png]]

cat shadow

Chiffrer le compte utilisateur de la base de compte local pour windows
**smbpasswd -a tech**

![[Pasted image 20230503114445.png]]

![[Pasted image 20230503114633.png]]

Droit utilisateur total
chmod 777 -R /home/tech 
chmod 755 -R /home/tech


# script de sauvegarde des données
backup auto de la base et du www pour le site

cd /root					  
touch backup.sh  
chmod +x backup.sh 		(est actif )			  
nano backup.sh

#!/bin/bash
#Script de backup auto de la base et du www pour le site user
#V1.1 par T.Cherrier sep 2022

clear  
echo Compression :  
 zip -rq /home/user/$(date +%Y%m%d%H%M)_www_backup-fichier.zip  /home/user/www  
echo Dump de la base  
archive en zip  de maniere recursive et silencieuse a la racine de home user nommer dateannée+mois+jour+heur+minute 

mysqldump -u root -p'dadfba16' --databases user > /home/user/$(date +%Y%m%d%H%M)_www_backup-base.sql  
echo Terminé

dadfba= mdp de root à remplacer par le vrai mdp
user : utilisateur créé dans la bas ici TOTO



![[Pasted image 20230503123430.png]]
crontab -e
https://crontab.guru/ pour configurer la sauvegarde
minute heure jour mois weekend
		x    x             x       x         x
![[Pasted image 20230503121900.png]]

![[Pasted image 20230503122921.png]]



# Mettre un certificat sur webadmin
Se connecter sur https://lamps.batlan:10000
![[Pasted image 20230503120804.png]]


# Wordpress

**cd /home/user  
rm -R /home/user/www
wget [https://fr.wordpress.org/latest-fr_FR.zip](https://fr.wordpress.org/latest-fr_FR.zip)  
unzip latest-fr_FR.zip  
mv wordpress www

chown -R www-data:www-data /home/tech/www
chmod -R 775 /home/tech/www


Se connecter via le navigateur sur : https://lamps.batlan/wp-admin

![[Pasted image 20230503124501.png]]


![[Pasted image 20230503151039.png]]

![[Pasted image 20230503124834.png]]

![[Pasted image 20230503151129.png]].


![[Pasted image 20230503153640.png]]]]
