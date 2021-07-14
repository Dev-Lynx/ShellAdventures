## Setting up Modoboa

14th July, 2021 12:23PM 		Debugging Dovecot

Configuration can be found at ` /etc/dovecot/dovecot.conf`

```bash
dovecot -n # Combine and print entire configuration
# https://stackoverflow.com/a/18104947/8058709
```



Splitting a large file:

```bash
split --help

# Splitting by lines
split -l 200000 large-file.txt
# https://stackoverflow.com/a/2016918/8058709

# Relevant application
cd /var/log
split -l 200000 mail.log
```



#### DNS Settings

MX RECORD: Hostname = hostname Value = mail.hostname

#### Investigating Used Ports

```bash
netstat -a | egrep 'Proto|LISTEN' 

# Preferred command
lsof -i -n | egrep 'COMMAND|LISTEN' 
```



#### Removing APT-Packages

```bash
sudo apt-get remove --purge php-fpm
```

It appears that the installation of `iredmail` and `modoboa` on the same server causes a configuration clash. 

* `php-form` was running on port 9999 which was specified by dovecot for `smtpd_recipient_restrictions`
* `opendkim` might needed ssl reconfiguration
* `dovecot` needed extra permissions to create mail folders at `/srv/mail`. `/srv` had to be `chmod -R g+rw /srv`
* `postfix` needed some reconfiguration
* `nginx` needed to be reconfigured too. (PS: Backup your configuration files (`/etc/nginx/sites-available`) somewhere)
