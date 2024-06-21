### Canvas LMS on Ubuntu 20.04

### Prerequisites,
RAM (Memory): 8 GB RAM
Processor: 4 CPU cores with 2.0 GHz or more
Disk Space: 30GB for storage
OS: Ubuntu 20.04 LTS - This is a MUST

anup@ubuntu-20046:~$ htop
anup@ubuntu-20046:~$ free -h
anup@ubuntu-20046:~$ lscpu
anup@ubuntu-20046:~$ df -h
anup@ubuntu-20046:~$ cat /etc/os-release
anup@ubuntu-20046:~$ sudo apt-get update



### Database installation and configuration,
anup@ubuntu-20046:~$ sudo apt-get install postgresql-12

anup@ubuntu-20046:~$ psql -V
anup@ubuntu-20046:~$ sudo systemctl status postgresql

anup@ubuntu-20046:~$ sudo lsof -i -P -n | grep LISTEN
anup@ubuntu-20046:~$ sudo netstat -plunt | grep postgres


### Set your system username as a postgres superuser,
anup@ubuntu-20046:~$ sudo -u postgres createuser $USER
anup@ubuntu-20046:~$ sudo -u postgres psql -c "alter user $USER with superuser" postgres


### Configuring Postgres
anup@ubuntu-20046:~$ sudo -u postgres createuser canvas --no-createdb \
>    --no-superuser --no-createrole --pwprompt
anup@ubuntu-20046:~$ sudo -u postgres createdb canvas_production --owner=canvas

anup@ubuntu-20046:~$ sudo -i -u postgres
postgres@ubuntu-20046:~$ psql
postgres=# \conninfo
postgres=# \du

postgres=# \list
postgres=# \c canvas_production
canvas_production=# \q
postgres@ubuntu-20046:~$ exit


### Getting the code
anup@ubuntu-20046:~$ sudo apt-get install git-core
anup@ubuntu-20046:~$ git clone https://github.com/instructure/canvas-lms.git canvas
anup@ubuntu-20046:~$ cd canvas
anup@ubuntu-20046:~/canvas$ git checkout prod
anup@ubuntu-20046:~/canvas$ git branch


### Code installation
anup@ubuntu-20046:~/canvas$ sudo mkdir -p /var/canvas
anup@ubuntu-20046:~/canvas$ sudo chown -R anup /var/canvas
anup@ubuntu-20046:~/canvas$ cp -av . /var/canvas
anup@ubuntu-20046:~/canvas$ cd /var/canvas
anup@ubuntu-20046:/var/canvas$ git branch


### Dependency Installation (Ruby Libraries and Packages)
anup@ubuntu-20046:~$ sudo apt-get install software-properties-common
anup@ubuntu-20046:~$ sudo add-apt-repository ppa:instructure/ruby
anup@ubuntu-20046:~$ sudo apt-get update


### Dependency Installation (Install Ruby)
anup@ubuntu-20046:~$ sudo apt-get install ruby3.1 ruby3.1-dev zlib1g-dev libxml2-dev libsqlite3-dev postgresql libpq-dev libxmlsec1-dev libyaml-dev libidn11-dev curl make g++

anup@ubuntu-20046:~$ ruby -v
anup@ubuntu-20046:~$ gem -v
anup@ubuntu-20046:~$ gem query --local
anup@ubuntu-20046:~$ sudo gem list
anup@ubuntu-20046:~$ dpkg -l libsqlite3-dev

anup@ubuntu-20046:~$ sudo apt install sqlite3
anup@ubuntu-20046:~$ sqlite3 --version

### Dependency Installation (Node.js installation)
Node.js installation: https://github.com/nvm-sh/nvm
anup@ubuntu-20046:~$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
anup@ubuntu-20046:~$ sudo nano /home/anup/.bashrc
anup@ubuntu-20046:~$ source ~/.bashrc
anup@ubuntu-20046:~$ command -v nvm
anup@ubuntu-20046:~$ nvm ls
anup@ubuntu-20046:~$ nvm list-remote
anup@ubuntu-20046:~$ nvm install 20
anup@ubuntu-20046:~$ node -v
anup@ubuntu-20046:~$ npm -v


### Dependency Installation (Ruby Gems, Bundler and Canvas dependencies)
anup@ubuntu-20046:~$ cd /var/canvas/
anup@ubuntu-20046:/var/canvas$ sudo gem install bundler --version 2.5.10
anup@ubuntu-20046:/var/canvas$ bundle config set --local path vendor/bundle
anup@ubuntu-20046:/var/canvas$ bundle install


### Dependency Installation (Yarn Installation)
anup@ubuntu-20046:/var/canvas$ curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
anup@ubuntu-20046:/var/canvas$ echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
anup@ubuntu-20046:/var/canvas$ sudo apt-get update && sudo apt-get install yarn=1.19.1-1
anup@ubuntu-20046:/var/canvas$ npm -g install yarn

npm notice To update run: npm install -g npm@10.8.1


### Install NodeJS dependencies
anup@ubuntu-20046:/var/canvas$ yarn install
anup@ubuntu-20046:/var/canvas$ yarn list
anup@ubuntu-20046:/var/canvas$ npm list
anup@ubuntu-20046:/var/canvas$ npm list --depth=0 (Without dependencies)
anup@ubuntu-20046:/var/canvas$ npm -g list (Global)
anup@ubuntu-20046:/var/canvas$ npm list grunt
anup@ubuntu-20046:/var/canvas$ ruby -v
anup@ubuntu-20046:/var/canvas$ cat /home/anup/.npm/_logs/2024-06-18T06_33_34_518Z-debug-0.log


### Canvas default configuration
anup@ubuntu-20046:/var/canvas$ for config in amazon_s3 database vault_contents \
>   delayed_jobs domain file_store outgoing_mail security external_migration; \
>   do cp config/$config.yml.example config/$config.yml; done  


### Canvas default configuration (Dynamic settings configuration)
anup@ubuntu-20046:/var/canvas$ cp config/dynamic_settings.yml.example config/dynamic_settings.yml
anup@ubuntu-20046:/var/canvas$ ls -ltr config | grep -i "dynamic_settings.yml"
anup@ubuntu-20046:/var/canvas$ nano config/dynamic_settings.yml
Make sure you replace development with production at the top in the dynamic_settings.yml file
production:
  # tree

### Canvas default configuration (Database configuration)
anup@ubuntu-20046:/var/canvas$ cp config/database.yml.example config/database.yml
anup@ubuntu-20046:/var/canvas$ ls -ltr config | grep -i "database.yml"
anup@ubuntu-20046:/var/canvas$ nano config/database.yml
production:
  adapter: postgresql
  encoding: utf8
  database: canvas_production
  host: localhost
  username: canvas
  password: 3ri=rL@4dRL9E
  timeout: 5000


### Canvas default configuration (Outgoing mail configuration)
anup@ubuntu-20046:/var/canvas$ cp config/outgoing_mail.yml.example config/outgoing_mail.yml
anup@ubuntu-20046:/var/canvas$ ls -ltr config | grep -i "outgoing_mail.yml"
anup@ubuntu-20046:/var/canvas$ nano config/outgoing_mail.yml


### Canvas default configuration (URL configuration)
anup@ubuntu-20046:/var/canvas$ cp config/domain.yml.example config/domain.yml
anup@ubuntu-20046:/var/canvas$ ls -ltr config | grep -i "domain.yml"
anup@ubuntu-20046:/var/canvas$ nano config/domain.yml
production:
  domain: 192.168.56.100


### Canvas default configuration (Security configuration)
anup@ubuntu-20046:/var/canvas$ cp config/security.yml.example config/security.yml
anup@ubuntu-20046:/var/canvas$ nano config/security.yml
production: &default
  # replace this with a random string of at least 20 characters
  encryption_key: facdd3a131ddd8988b14f6e4e01039c93cfa0160
  lti_iss: 'https://canvas.instructure.com'


### Database population
anup@ubuntu-20046:/var/canvas$ yarn gulp rev
anup@ubuntu-20046:/var/canvas$ RAILS_ENV=production bundle exec rake db:initial_setup
What email address will the site administrator account use? > uniqs.anup@gmail.com
Please confirm > uniqs.anup@gmail.com
What password will the site administrator use? > 3ri=rL@4dRL9E
Please confirm > 3ri=rL@4dRL9E
What do you want users to see as the account name? This should probably be the name of your organization. > aleph-vitatha

### Generate Assets
anup@ubuntu-20046:/var/canvas$ cd /var/canvas
anup@ubuntu-20046:/var/canvas$ mkdir -p log tmp/pids public/assets app/stylesheets/brandable_css_brands
anup@ubuntu-20046:/var/canvas$ touch app/stylesheets/_brandable_variables_defaults_autogenerated.scss
anup@ubuntu-20046:/var/canvas$ touch Gemfile.lock
anup@ubuntu-20046:/var/canvas$ touch log/production.log
anup@ubuntu-20046:/var/canvas$ sudo chown -R anup config/environment.rb log tmp public/assets \
>                               app/stylesheets/_brandable_variables_defaults_autogenerated.scss \
>                               app/stylesheets/brandable_css_brands Gemfile.lock config.ru


anup@ubuntu-20046:/var/canvas$ RAILS_ENV=production bundle exec rake canvas:compile_assets
anup@ubuntu-20046:/var/canvas$ chown -R anup public/dist/brandable_css


### Canvas ownership ()
anup@ubuntu-20046:/var/canvas$ sudo adduser --disabled-password --gecos canvas anup
anup@ubuntu-20046:/var/canvas$ sudo chown anup config/*.yml
anup@ubuntu-20046:/var/canvas$ sudo chmod 400 config/*.yml


### Apache configuration (Installation)
anup@ubuntu-20046:/var/canvas$ sudo apt-get install apache2

anup@ubuntu-20046:/var/canvas$ sudo apt-get install -y dirmngr gnupg apt-transport-https ca-certificates
anup@ubuntu-20046:/var/canvas$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7
anup@ubuntu-20046:/var/canvas$ sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger focal main > /etc/apt/sources.list.d/passenger.list'
anup@ubuntu-20046:/var/canvas$ sudo apt-get update
anup@ubuntu-20046:/var/canvas$ sudo apt-get install -y libapache2-mod-passenger
anup@ubuntu-20046:/var/canvas$ sudo apt-get install -y passenger
anup@ubuntu-20046:/var/canvas$ sudo a2enmod rewrite

<!-- anup@ubuntu-20046:/var/canvas$ sudo systemctl restart apache2
anup@ubuntu-20046:/var/canvas$ sudo systemctl status apache2 -->

anup@ubuntu-20046:/var/canvas$ passenger -v
anup@ubuntu-20046:/var/canvas$ apache2 -v
anup@ubuntu-20046:/var/canvas$ sudo /usr/bin/passenger-config validate-install
anup@ubuntu-20046:/var/canvas$ sudo /usr/sbin/passenger-memory-stats


### Apache configuration (Configure Passenger with Apache)
anup@ubuntu-20046:/var/canvas$ sudo a2enmod passenger

anup@ubuntu-20046:/var/canvas$ ls -ltr /usr/lib/apache2/modules/mod_passenger.so
anup@ubuntu-20046:/var/canvas$ sudo nano /etc/apache2/mods-enabled/passenger.conf
<IfModule mod_passenger.c>
  PassengerRoot /usr/lib/ruby/vendor_ruby/phusion_passenger/locations.ini
  PassengerDefaultRuby /usr/bin/passenger_free_ruby

  PassengerInstanceRegistryDir /var/run/passenger-instreg

  LoadModule passenger_module /usr/lib/apache2/modules/mod_passenger.so
  PassengerRoot /usr
  PassengerRuby /usr/bin/ruby

  PassengerDefaultUser anup

</IfModule>

anup@ubuntu-20046:/var/canvas$ sudo nano /etc/apache2/mods-enabled/passenger.load
LoadModule passenger_module /usr/lib/apache2/modules/mod_passenger.so

anup@ubuntu-20046:/var/canvas$ sudo apachectl configtest
anup@ubuntu-20046:/var/canvas$ passenger-config --version


### Apache configuration (Configure SSL with Apache)
anup@ubuntu-20046:/var/canvas$ sudo a2enmod ssl

<!-- anup@ubuntu-20046:/var/canvas$ systemctl restart apache2 -->


### Apache configuration (Configure Canvas with Apache) 
anup@ubuntu-20046:/var/canvas$ sudo unlink /etc/apache2/sites-enabled/000-default.conf
anup@ubuntu-20046:/var/canvas$ sudo nano /etc/apache2/sites-available/canvas.conf

anup@ubuntu-20046:/var/canvas$ cd /etc/apache2/sites-enabled/
anup@ubuntu-20046:/etc/apache2/sites-enabled$ ls -ltr
anup@ubuntu-20046:/etc/apache2/sites-enabled$ sudo a2ensite canvas
anup@ubuntu-20046:/etc/apache2/sites-enabled$ ls -ltr

anup@ubuntu-20046:/etc/apache2/sites-enabled$ sudo apachectl configtest
anup@ubuntu-20046:/etc/apache2/sites-enabled$ sudo systemctl reload apache2
anup@ubuntu-20046:/etc/apache2/sites-enabled$ sudo systemctl restart apache2


### Optimizing File Downloads
anup@ubuntu-20046:/var/canvas$ sudo apt-get install libapache2-mod-xsendfile  
anup@ubuntu-20046:/var/canvas$ sudo apachectl -M | sort

anup@ubuntu-20046:/var/canvas$ cd config/environments/
anup@ubuntu-20046:/var/canvas/config/environments$ cp production.rb production-local.rb
anup@ubuntu-20046:/var/canvas/config/environments$ ls -ltr
anup@ubuntu-20046:/var/canvas/config/environments$ nano production-local.rb
anup@ubuntu-20046:/var/canvas/config/environments$ nano /etc/apache2/sites-available/canvas.conf
    XSendFile On
    XSendFilePath /var/canvas


### Cache configuration
anup@ubuntu-20046:/var/canvas$ sudo add-apt-repository ppa:chris-lea/redis-server
anup@ubuntu-20046:/var/canvas$ sudo apt-get update
anup@ubuntu-20046:/var/canvas$ sudo apt-get install redis-server

anup@ubuntu-20046:/var/canvas$ cd /var/canvas/
anup@ubuntu-20046:/var/canvas$ sudo cp config/cache_store.yml.example config/cache_store.yml
anup@ubuntu-20046:/var/canvas$ sudo nano config/cache_store.yml
anup@ubuntu-20046:/var/canvas$ sudo chown anup config/cache_store.yml
anup@ubuntu-20046:/var/canvas$ sudo chmod 400 config/cache_store.yml

anup@ubuntu-20046:/var/canvas$ cd /var/canvas/
anup@ubuntu-20046:/var/canvas$ sudo cp config/redis.yml.example config/redis.yml
anup@ubuntu-20046:/var/canvas$ sudo nano config/redis.yml
anup@ubuntu-20046:/var/canvas$ sudo chown anup config/redis.yml
anup@ubuntu-20046:/var/canvas$ sudo chmod 400 config/redis.yml

anup@ubuntu-20046:/var/canvas$ ps aux | grep redis-server
anup@ubuntu-20046:/var/canvas$ redis-server --daemonize yes

### Python QTI Migration Tool
anup@ubuntu-20046:/var/canvas$ sudo apt install python3-pip
anup@ubuntu-20046:/var/canvas$ pip3 install lxml
anup@ubuntu-20046:/var/canvas$ cd /var/canvas/vendor
anup@ubuntu-20046:/var/canvas/vendor$ git clone https://github.com/instructure/QTIMigrationTool.git QTIMigrationTool
anup@ubuntu-20046:/var/canvas/vendor$ cd QTIMigrationTool
anup@ubuntu-20046:/var/canvas/vendor/QTIMigrationTool$ chmod +x migrate.py

anup@ubuntu-20046:/var/canvas/vendor/QTIMigrationTool$ cd /var/canvas/
anup@ubuntu-20046:/var/canvas$ script/delayed_job restart


### Automated jobs (Installation)
anup@ubuntu-20046:/var/canvas$ sudo ln -s /var/canvas/script/canvas_init /etc/init.d/canvas_init
anup@ubuntu-20046:/var/canvas$ sudo update-rc.d canvas_init defaults
anup@ubuntu-20046:/var/canvas$ sudo /etc/init.d/canvas_init start

anup@ubuntu-20046:/var/canvas$ sudo /etc/init.d/apache2 restart
anup@ubuntu-20046:/var/canvas$ sudo systemctl status apache2.service
anup@ubuntu-20046:/var/canvas$ sudo /etc/init.d/canvas_init restart
anup@ubuntu-20046:/var/canvas$ sudo /etc/init.d/canvas_init status


https://elearningevolve.com/blog/install-canvas-lms/


### Logs
anup@ubuntu-20046:/var/log/apache2$ /usr/bin/passenger-config compile-agent
anup@ubuntu-20046:/var/log/apache2$ /usr/bin/passenger-config compile-agent --force parameter
anup@ubuntu-20046:/var/log/apache2$ passenger-config download-agent
anup@ubuntu-20046:/var/log/apache2$ passenger-config download-agent --force parameter
anup@ubuntu-20046:/var/canvas/log$ multitail production.log delayed_job.log

### Firewall
anup@ubuntu-20046:/var/canvas$ sudo ufw status
anup@ubuntu-20046:/var/canvas$ sudo ufw enable
anup@ubuntu-20046:/var/canvas$ sudo ufw allow 22
anup@ubuntu-20046:/var/canvas$ sudo ufw allow 22/tcp
anup@ubuntu-20046:/var/canvas$ sudo ufw allow 80
anup@ubuntu-20046:/var/canvas$ sudo ufw allow 80/tcp
anup@ubuntu-20046:/var/canvas$ sudo ufw allow 443
anup@ubuntu-20046:/var/canvas$ sudo ufw allow 443/tcp
anup@ubuntu-20046:/var/canvas$ sudo ufw allow 6379
anup@ubuntu-20046:/var/canvas$ sudo ufw allow 6379/tcp

