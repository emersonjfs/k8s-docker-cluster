# Vagrantfile para Kubernetes Cluster
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Definindo o box de sistema operacional
  config.vm.box = "ubuntu/focal64"

  # Desabilitar a sincronização de diretório padrão
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # Configuração do nó mestre
  config.vm.define "k8s-master" do |master|
    master.vm.hostname = "k8s-master"
    master.vm.network "private_network", ip: "192.168.56.10"
    master.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
    end
    master.vm.provision "shell", path: "bootstrap.sh", args: ["master"]
  end

  # Configuração dos nós de trabalho
  (1..2).each do |i|
    config.vm.define "k8s-worker#{i}" do |worker|
      worker.vm.hostname = "k8s-worker#{i}"
      worker.vm.network "private_network", ip: "192.168.56.1#{i+1}"
      worker.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 2
      end
      worker.vm.provision "shell", path: "bootstrap.sh", args: ["worker"]
    end
  end
end
