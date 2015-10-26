# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    config.vm.box = "scotch/box"
    # if we need to drop the php version from 5.6, uncomment below:
    # config.vm.box_version = "1.5"
    
    config.vm.network "private_network", ip: "192.168.33.10"
    config.vm.hostname = "scotchbox" # rename to example.dev (example = client domain name)
    config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]
    
    # Weird SSH issues can occur - workaround:
    #config.ssh.username = "vagrant"
    #config.ssh.password = "vagrant"
    
    # Optional NFS. Make sure to remove other synced_folder line too
    #config.vm.synced_folder ".", "/var/www", :nfs => { :mount_options => ["dmode=777","fmode=666"] }
    
    # vagrant plugin install vagrant-vbguest
    Vagrant::Config.run do |config|
      # set auto_update to false, if you do NOT want to check the correct
      # additions version when booting this machine
      config.vbguest.auto_update = true
      # do NOT download the iso file from a webserver
      config.vbguest.no_remote = true
    end
    
    # Vagrant Triggers
    # Backup MySQL database on halt: :destroy
    if Vagrant.has_plugin? 'vagrant-triggers'
      config.trigger.before :halt, :stdout => true do
        info "Dumping database before 'halt'"
        run  "vagrant ssh -c 'mysqldump -u root scotchbox > /var/www/db/mysqldump_$(date +%Y-%m-%d-%H.%M.%S).sql'"
        # test using wp-cli to dump database
          # wp db export --add-drop-table
          # wp db export --tables=wp_options,wp_users
      end
      config.trigger.before :destroy, :stdout => true do
        info "Dumping database before 'destroy'"
        run  "vagrant ssh -c 'mysqldump -u root scotchbox > /var/www/db/mysqldump_$(date +%Y-%m-%d-%H.%M.%S).sql'"
        # test using wp-cli to dump database
          # wp db export --add-drop-table
          # wp db export --tables=wp_options,wp_users
      end
      config.trigger.before :suspend, :stdout => true do
        info "Dumping database before 'suspend'"
        run  "vagrant ssh -c 'mysqldump -u root scotchbox > /var/www/db/mysqldump_$(date +%Y-%m-%d-%H.%M.%S).sql'"
        # test using wp-cli to dump database
          # wp db export --add-drop-table
          # wp db export --tables=wp_options,wp_users
      end
      config.trigger.before :reload, :stdout => true do
        info "Dumping database before 'reload'"
        run  "vagrant ssh -c 'mysqldump -u root scotchbox > /var/www/db/mysqldump_$(date +%Y-%m-%d-%H.%M.%S).sql'"
        # test using wp-cli to dump database
          # wp db export --add-drop-table
          # wp db export --tables=wp_options,wp_users
      end
    else
      puts 'vagrant-triggers missing, please install the plugin:'
      puts 'vagrant plugin install vagrant-triggers'
    end
    
    # import - create trigger*
    # mysql -u root -p < alldb.sql
    # run "vagrant ssh -c 'mysql -u root scotchbox < mysqldump_DATE'"

    # Extra provisions:
    config.vm.provision "shell", inline: <<-SHELL, privileged: true

      # TODO: Install Mailcatcher

      # Install WP-CLI
      curl -s https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar > wp-cli.phar
      chmod +x wp-cli.phar
      sudo mv wp-cli.phar /usr/local/bin/wp

    SHELL

end
