# -*- mode: ruby -*-"
# vi: set ft=ruby :

box_type1  = "bento/centos-7.2"
box_type2  = "ubuntu/trusty64"

nodes = [
  {
    :name => 'master1',
    :box => box_type1,
    :ip => '192.168.56.20',
    :ram => '1024',
    :cpu => '1',
    :scripts => [
      '/vagrant/bootstrap-salt.sh -P -M git v2015.8.7',
      #'yum -y install salt-cloud sshpass',
      'cp /vagrant/salt/master/master.p* /etc/salt/pki/master/',
      'chown root:root /etc/salt/pki/master/master*',
      'chmod 600 /etc/salt/pki/master/master.pem',
      'chmod 644 /etc/salt/pki/master/master.pub',
      'service salt-master restart',
      'cp /vagrant/salt/minion/minion /etc/salt/minion',
      'service salt-minion restart'
    ]
  },
  {
    :name => 'master2',
    :box => box_type2,
    :ip => '192.168.56.30',
    :ram => '1024',
    :cpu => '1',
    :scripts => [
      '/vagrant/bootstrap-salt.sh -P -M git v2015.8.7',
      #'apt-get -y install salt-cloud sshpass',
      'cp /vagrant/salt/master/master.p* /etc/salt/pki/master/',
      'chown root:root /etc/salt/pki/master/master*',
      'chmod 600 /etc/salt/pki/master/master.pem',
      'chmod 644 /etc/salt/pki/master/master.pub',
      'service salt-master restart',
      'cp /vagrant/salt/minion/minion /etc/salt/minion',
      'service salt-minion restart'
    ]
  },
  {
    :name => 'cweb',
    :box => box_type1,
    :ip => '192.168.56.21',
    :ram => '512',
    :cpu => '1',
    :scripts => [
      '/vagrant/bootstrap-salt.sh -P git v2015.8.7',
      'cp /vagrant/salt/minion/minion /etc/salt/minion',
      'service salt-minion restart'
    ]
  },
  {
    :name => 'sdev',
    :box => box_type1,
    :ip => '192.168.56.22',
    :ram => '512',
    :cpu => '1',
    :scripts => [
      '/vagrant/bootstrap-salt.sh -P git v2015.8.7',
      'cp /vagrant/salt/minion/minion /etc/salt/minion',
      'service salt-minion restart'
    ]
  },
  {
    :name => 'redis',
    :box => box_type2,
    :ip => '192.168.56.23',
    :ram => '512',
    :cpu => '1',
    :scripts => [
      '/vagrant/bootstrap-salt.sh -P git v2015.8.7',
      'cp /vagrant/salt/minion/minion /etc/salt/minion',
      'service salt-minion restart'
    ]
  },
  {
    :name => 'uarchive',
    :box => box_type2,
    :ip => '192.168.56.24',
    :ram => '512',
    :cpu => '1',
    :scripts => [
      '/vagrant/bootstrap-salt.sh -P git v2015.8.7',
      'cp /vagrant/salt/minion/minion /etc/salt/minion',
      'service salt-minion restart'
    ]
  }
]

Vagrant.configure(2) do |config|
  nodes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.box = opts[:box]
      config.vm.hostname = opts[:name]
      config.vbguest.auto_update = false

      config.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--name", opts[:name]]
        v.customize ["modifyvm", :id, "--memory", opts[:ram]]
        v.customize ["modifyvm", :id, "--cpus", opts[:cpu]]
      end

      config.vm.network :private_network, ip: opts[:ip]
      
      opts[:scripts].each do |script|
        config.vm.provision :shell, :inline => script
      end

      folders = opts[:folders]
      if folders
        keys = folders.keys
        keys.each do |key|
          config.vm.synced_folder key, folders[key]
        end
      end
    end
  end
end
