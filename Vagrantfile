Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"

  # Configurações globais para o Provider VirtualBox
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 512
    vb.linked_clone = true
    vb.check_guest_additions = false
  end

  # Desativa a geração de chaves SSH automática (requisito do projeto)
  config.ssh.insert_key = false

  # --- Servidor de Arquivos (ARQ) ---
  config.vm.define "arq" do |arq|
    arq.vm.hostname = "arq.pedro.vercosa.devops"
    arq.vm.network "private_network", ip: "192.168.56.136", adapter: 2
    
    # Adicionando 3 discos de 10GB para o LVM
    arq.vm.provider "virtualbox" do |vb|
      (1..3).each do |i|
        file = "./disk-#{i}.vdi"
        # CORREÇÃO AQUI: de exists? para exist?
        unless File.exist?(file)
          vb.customize ['createmedium', 'disk', '--filename', file, '--size', 10 * 1024]
        end
        vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', i, '--device', 0, '--type', 'hdd', '--medium', file]
      end
    end
  end

  # --- Servidor de Banco de Dados (DB) ---
  config.vm.define "db" do |db|
    db.vm.hostname = "db.pedro.vercosa.devops"
    db.vm.network "private_network", type: "dhcp", adapter: 2, mac: "080027AAAA01"
  end

  # --- Servidor de Aplicação (APP) ---
  config.vm.define "app" do |app|
    app.vm.hostname = "app.pedro.vercosa.devops"
    # CORREÇÃO AQUI: a variável deve ser 'app' e não 'db'
    app.vm.network "private_network", type: "dhcp", adapter: 2, mac: "080027AAAA02"
  end

  # --- Host Cliente (CLI) ---
  config.vm.define "cli" do |cli|
    cli.vm.hostname = "cli.pedro.vercosa.devops"
    cli.vm.network "private_network", type: "dhcp", adapter: 2
    cli.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
    end
  end

# Gatilho corrigido para ignorar erros caso a rede não exista
  config.trigger.before :up do |trigger|
    trigger.info = "Limpando DHCPs antigos do VirtualBox..."
    trigger.run = { inline: "bash -c 'VBoxManage dhcpserver remove --netname HostInterfaceNetworking-vboxnet0 2>/dev/null || true'" }
  end
end
