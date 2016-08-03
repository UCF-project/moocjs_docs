---
title: "Installation"
slug: "installation.md"
weight: 20
---

These steps will install a development environment. Check
[VISH official doc](https://github.com/ging/vish/wiki/Deployment) for
production deployment.


First make sure you have all the system dependencies.

```bash
# For example in ubuntu

apt-get -y update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
		software-properties-common \
		python-software-properties && \
	apt-add-repository ppa:brightbox/ruby-ng && \
	apt-get -y update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
		make \
		git-core \
		ruby2.2 \
		ruby2.2-dev \
		libxml2-dev \
		libxslt-dev \
		libmagickcore-dev \
		libmagickwand-dev \
		libmysqlclient-dev \
		libsqlite3-dev \
		imagemagick \
		libpq-dev \
		nodejs
```

Clone the UCF MOOC, install gems and copy configuration.

```bash
git clone git@git.rnd.alterway.fr:UCF/vish.git
cd vish
gem install bundler
bundle install
cp config/application_config.yml.example config/application_config.yml
```

Install a database and initiate it.

Note: Other database options can be found in the
[official VISH documentation](https://github.com/ging/vish/wiki/ViSH-Installation-Linux).
However Sphinx will only work with MySQL.

```bash
echo mysql-server mysql-server/root_password password| debconf-set-selections && \
	echo mysql-server mysql-server/root_password_again password| debconf-set-selections && \
	DEBIAN_FRONTEND=noninteractive apt-get install -y mysql-server libdbd-mysql-ruby && \
	service mysql start && \
	mysql -u root -e "create database vish_development;" && \
	cp config/database.yml.example config/database.yml  && \
	bundle exec rake db:schema:load && \
	bundle exec rake db:migrate
```

Install Sphinx, build the index and start service.

```bash
apt-get install -y sphinxsearch && \
	service mysql start && \
	service sphinxsearch start && \
	bundle exec rake ts:index && \
	bundle exec rake ts:config && \
	bundle exec rake ts:rebuild
```

Start database, populate it, rebuild Sphinx indexes.

```bash
service mysql start && \
	bundle exec rake db:install && \
	bundle exec rake db:populate && \
	bundle exec rake ts:rebuild
```

Start rails server.

```bash
rails s
```

Default login is **demo@vishub.org** and password is **demonstration** .
