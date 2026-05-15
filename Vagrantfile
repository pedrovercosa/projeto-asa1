Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"

  # Configurações globais para o Provider VirtualBox
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 512
    vb.linked_clone = true
    vb.check_guest_additions = false
  end

  # Desativa a geração de chaves SSH automática
  config.ssh.insert_key = false

  # --- Servidor de Arquivos (ARQ) ---
  config.vm.define "arq" do |arq|
    arq.vm.hostname = "arq.pedro.vercosa.devops"
    # Adicionado virtualbox__intnet para garantir que todos estejam no mesmo barramento
    arq.vm.network "private_network", ip: "192.168.56.136", adapter: 2, virtualbox__intnet: "rede_projeto"

    arq.vm.provider "virtualbox" do |vb|
      (1..3).each do |i|
        file = "./disk-#{i}.vdi"
        unless File.exist?(file)
          vb.customize ['createmedium', 'disk', '--filename', file, '--size', 10 * 1024]
        end
        # Comando de storageattach completo
        vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', i, '--device', 0, '--type', 'hdd', '--medium', file]
      end
    end
  end

  # --- Servidor de Banco de Dados (DB) ---
  config.vm.define "db" do |db|
    db.vm.hostname = "db.pedro.vercosa.devops"
    db.vm.network "private_network", type: "dhcp", adapter: 2, mac: "080027AAAA01", virtualbox__intnet: "rede_projeto"
  end

  # --- Servidor de Aplicação (APP) ---
  config.vm.define "app" do |app|
    app.vm.hostname = "app.pedro.vercosa.devops"
    app.vm.network "private_network", type: "dhcp", adapter: 2, mac: "080027AAAA02", virtualbox__intnet: "rede_projeto"
  end

  # --- Host Cliente (CLI) ---
  config.vm.define "cli" do |cli|
    cli.vm.hostname = "cli.pedro.vercosa.devops"
    cli.vm.network "private_network", type: "dhcp", adapter: 2, virtualbox__intnet: "rede_projeto"
    cli.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
    end
  end

  # Gatilho para limpar DHCPs antigos (Comando completo)
  config.trigger.before :up do |trigger|
    trigger.info = "Limpando DHCPs antigos do VirtualBox..."
    trigger.run = { inline: "bash -c 'VBoxManage dhcpserver remove --netname HostInterfaceNetworking-vboxnet0 || true'" }
  end
end
