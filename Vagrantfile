Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.define "db" do |db|
    config.vm.network "forwarded_port", guest: 5432, host: 5432
    config.vm.network "private_network", ip: "192.168.1.11"

    config.vm.provider "virtualbox" do |vb|
      vb.name = "dj-database"
      vb.gui = "false"
      vb.memory = "512"
    end

    config.vm.provision "shell", inline: <<-SHELL
      apt-get install -y postgresql
      postgres -c "psql -f /vagrant/db-config.sql"
      postgres -c 'echo "host all all 192.168.1.10/24 trust" >> /etc/postgresql/9.3/main/pg_hba.conf'
      postgres -c "echo listen_addresses=\\'*\\' >> /etc/postgresql/9.3/main/postgresql.conf"
      service postgresql restart
    SHELL
  end

  config.vm.define "web" do |web|
    config.vm.network "forwarded_port", guest: 8000, host: 8000
    config.vm.network "private_network", ip: "192.168.1.10"

    config.vm.provider "virtualbox" do |vb|
      vb.name = "dj"
      vb.gui =  false
      vb.memory = 512
    end

    config.vm.provision "shell", inline: <<-SHELL
      apt-get install -y python-pip python-dev libpq-dev postgresql postgresql-contrib
      pip install django flake8 psycopg2
    SHELL
  end
end
