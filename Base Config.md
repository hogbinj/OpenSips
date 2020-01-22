# SSH to box to setup a clean install
```
sudo su -
apt-get update
apt-get upgrade
apt autoremove
reboot
```

# Re SSH into box & set user as root
```
sudo su -
```

# Install LAMP
```
apt update && sudo apt install lamp-server
```

# Configure MySQL
```
mysql_secure_installation
Password = $PASSWORD$
Remove anonymous users? (Press y|Y for Yes, any other key for No) : Y
Disallow root login remotely? (Press y|Y for Yes, any other key for No) : Y
Remove test database and access to it? (Press y|Y for Yes, any other key for No) : Y
Reload privilege tables now? (Press y|Y for Yes, any other key for No) : Y
```

# Install OpenSips
```
echo "deb https://apt.opensips.org bionic 3.0-releases” >/etc/apt/sources.list.d/opensips.list
apt-get update
apt-get install opensips
```

# Install CLI
```
apt-get install libssl-dev
apt install libncurses5-dev
apt-get install -y m4
apt-get install python3 python3-pip python3-dev gcc default-libmysqlclient-dev
pip3 install mysqlclient sqlalchemy sqlalchemy-utils pyOpenSSL
git clone https://github.com/opensips/opensips-cli ~/src/opensips-cli
cd ~/src/opensips-cli
python3 setup.py install clean
```

# Add DB for OpenSIPS
```
apt install opensips-mysql-module 
opensips-cli -x database create
Please provide the URL of the SQL database: mysql://root:$PASSWORD$localhost
Create [a]ll tables or just the [c]urrently configured ones? (Default value is a): a
```

# Modify mySQL for Opensips Control Panel
```
mysql -u root -p
CREATE USER 'opensips'@'%' IDENTIFIED BY 'opensipsrw';
GRANT ALL PRIVILEGES ON opensips.* TO 'opensips'@'%';
Quit;
```

# Install WebControl Panel
```
cd ~
wget https://github.com/OpenSIPS/opensips-cp/archive/master.zip
apt install unzip
unzip master.zip
mv opensips-cp-master /var/www/html/opensips
chown -R www-data:www-data /var/www/html/opensips
vi /etc/apache2/sites-available/000-default.conf 

  <Directory /var/www/html/opensips/web>
		Options Indexes FollowSymLinks MultiViews
		AllowOverride None
		Require all granted
	</Directory>
	<Directory /var/www/html/opensips>
		Options Indexes FollowSymLinks MultiViews
		AllowOverride None
		Require all denied
	</Directory>
	Alias /cp /var/www/html/opensips/web

	<DirectoryMatch "/var/www/html/opensips/web/tools/.*/.*/(template|custom_actions|lib)/">
		Require all denied
	</DirectoryMatch>

cp /var/www/html/opensips/config/tools/system/smonitor/ /etc/cron.d/
mysql -Dopensips -p < /var/www/html/opensips/config/db_schema.mysql
systemctl restart apache2
```

# Tidy up
```
apt autoremove
```
# Launching OpenSips
Now you can create an OpenSips config and set to autostart.  Remember to install the correct bits of opensips to match your config or you’ll get a service not started 
```
journalctl -xe

-- 
-- Unit opensips.service has finished shutting down.
Jan 22 11:07:23 sbc systemd[1]: opensips.service: Start request repeated too quickly.
Jan 22 11:07:23 sbc systemd[1]: opensips.service: Failed with result 'exit-code'.
Jan 22 11:07:23 sbc systemd[1]: Failed to start OpenSIPS is a very fast and flexible SIP (RFC3261) server.
```

# Some useful commands
 `apt-cache search opensips*` - Lists all the opensips modules you can install
 `dpkg -L opensips` - Lists where all the files are installed
 `/usr/sbin/osipsconfig` - OpenSips Config
