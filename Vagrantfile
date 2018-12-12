nodes = [
  { :hostname => 'k8s1', :ip => '192.168.10.11', :box => 'ubuntu/xenial64', :memory => 2048, :cpus => 2 },
  { :hostname => 'k8s2', :ip => '192.168.10.12', :box => 'ubuntu/xenial64', :memory => 2048, :cpus => 2 },
  { :hostname => 'k8s3', :ip => '192.168.10.13', :box => 'ubuntu/xenial64', :memory => 2048, :cpus => 2 },
]

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.ssh.private_key_path = ["./id_rsa", "~/.vagrant.d/insecure_private_key"]

  nodes.each do |node|
    config.vm.define node[:hostname] do |nodeconfig|
      nodeconfig.vm.box = node[:box]
      nodeconfig.vm.hostname = node[:hostname]
      nodeconfig.vm.network :private_network, ip: node[:ip]

      memory = node[:memory] ? node[:memory] : 1024
      cpus = node[:cpus] ? node[:cpus] : 2
      nodeconfig.vm.provider :virtualbox do |vb|
        vb.memory = memory
        vb.cpus   = cpus
      end

      nodeconfig.vm.provision "file", source: "./id_rsa.pub", destination: "~/.ssh/authorized_keys"

      nodeconfig.vm.provision :shell, path: "bootstrap.sh"
    end
  end
end
