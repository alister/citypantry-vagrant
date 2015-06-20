Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_check_update = false
  config.vm.hostname = "citypantry.dev"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  # Folder shares.
  # Use NFS for Puppet, because it does not need a specific owner.
  # Use rsync for the application, because those folders need to be owned by the 'citypantry'
  # user, and NFS does not support changing the owner of a synced folder.
  config.vm.synced_folder "./citypantry-3-frontend", "/home/citypantry/project/frontend",
    type: "rsync",
    create: true,
    owner: "citypantry",
    group: "citypantry",
    rsync__auto: true,
    rsync__exclude: ["app/cache/", "app/logs/"]
  config.vm.synced_folder "./citypantry-3-api", "/home/citypantry/project/api",
    type: "rsync",
    create: true,
    owner: "citypantry",
    group: "citypantry",
    rsync__auto: true,
    rsync__exclude: ["app/cache/", "app/logs/"]
  config.vm.synced_folder "./citypantry-2-puppet", "/home/vagrant/puppet",
    type: "nfs"

  # Provider-specific configuration.
  config.vm.provider "virtualbox" do |vb|
    vb.name = "citypantry_dev"
  end

  config.vm.provision "shell", inline: <<-SHELL
    echo 'Setting up system dependencies.'
    add-apt-repository ppa:nginx/development
    apt-get update
    [ -f /usr/bin/git ] || apt-get install git -y
    [ -f /usr/bin/puppet ] || apt-get install puppet -y
    echo 'Setting up Node dependencies that are not yet install via Puppet.'
    apt-get install npm -y
    npm install -g grunt-cli
    ln -s /usr/bin/nodejs /usr/bin/node
    echo 'Starting Puppet.'
    cd /home/vagrant/puppet
    git checkout master
    ./papply
    echo 'Stopping Puppet agent because the machine has been provisioned via Puppet apply.'
    service puppet stop
    echo 'Finished.'
  SHELL

  config.vm.post_up_message = <<-MESSAGE
    Your City Pantry development VM has been created!

    You can SSH into the VM with `vagrant ssh`, or `ssh USERNAME@192.168.33.10 -p 2223`

    If everything else is set up correctly, you can see the website in your browser at:
    http://order.citypantry.dev/

    Now start syncing the API and frontend folders: `nohup vagrant rsync-auto &`
  MESSAGE
end
