# -*- mode: ruby -*-"
# vi: set ft=ruby :

box_type1  = "bento/centos-7.1"
box_type2  = "ubuntu/trusty64"

nodes = [
  {
    :vm => 'master',
    :hostname => 'master',
    :box => box_type2,
    :ip => '192.168.56.20',
    :ram => '1024',
    :cpu => '1',
    :scripts => [
      '/vagrant/bootstrap-salt.sh -P -M'
    ]
  },
  {
    :vm => 'cweb',
    :hostname => 'cweb',
    :box => box_type1,
    :ip => '192.168.56.21',
    :ram => '512',
    :cpu => '1',
    :scripts => [
      '/vagrant/bootstrap-salt.sh -P',
      'echo "master: 192.168.56.20" >> /etc/salt/minion',
      'echo "id: cweb" >> /etc/salt/minion',
      'service salt-minion restart'
    ]
  },
  {
    :vm => 'sdev',
    :hostname => 'sdev',
    :box => box_type1,
    :ip => '192.168.56.22',
    :ram => '512',
    :cpu => '1',
    :scripts => [
      '/vagrant/bootstrap-salt.sh -P',
      'echo "master: 192.168.56.20" >> /etc/salt/minion',
      'echo "id: sdev" >> /etc/salt/minion',
      'service salt-minion restart'
    ]
  },
  {
    :vm => 'redis',
    :hostname => 'redis',
    :box => box_type1,
    :ip => '192.168.56.23',
    :ram => '512',
    :cpu => '1',
    :scripts => [
      '/vagrant/bootstrap-salt.sh -P',
      'echo "master: 192.168.56.20" >> /etc/salt/minion',
      'echo "id: redis" >> /etc/salt/minion',
      'service salt-minion restart'
    ]
  },
  {
    :vm => 'uarchive',
    :hostname => 'uarchive',
    :box => box_type2,
    :ip => '192.168.56.24',
    :ram => '512',
    :cpu => '1',
    :scripts => [
      '/vagrant/bootstrap-salt.sh -P',
      'echo "master: 192.168.56.20" >> /etc/salt/minion',
      'echo "id: uarchive" >> /etc/salt/minion',
      'service salt-minion restart'
    ]
  }
]

Vagrant.configure(2) do |config|
  nodes.each do |node|
    config.vm.define node[:vm] do |node_config|
      config.vm.provider 'virtualbox' do |vb|
        vb.name = node[:hostname]
        vb.memory = node[:ram]
        vb.cpus = node[:cpu]
      end
      folders = node[:folders]
      if folders
        keys = folders.keys
        keys.each do |key|
          node_config.vm.synced_folder key, folders[key]
        end
      end
      config.vbguest.auto_update = false
      node_config.vm.box = node[:box]
      node_config.vm.hostname = node[:hostname]
      node_config.vm.network :private_network, ip: node[:ip]
      node[:scripts].each do |script|
        node_config.vm.provision :shell, :inline => script
      end
    end
  end
end
