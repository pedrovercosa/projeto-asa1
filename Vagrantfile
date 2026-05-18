Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"

  # Configurações globais para o Provider VirtualBox
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 512
    vb.linked_clone = true
    vb.check_guest_additions = false
  end

  # Desativa a geração de chaves SSH automática (Exigência do PDF)
  config.ssh.insert_key = false
  config.ssh.keys_only = true

  # --- Servidor de Arquivos (ARQ) ---
  # IP fixo configurado com XX = 36 (192.168.56.136)
  config.vm.define "arq" do |arq|
    arq.vm.hostname = "arq.pedro.vercosa.devops"
    arq.vm.network "private_network", ip: "192.168.56.136", adapter: 2, virtualbox__intnet: "rede_projeto"

    arq.vm.provider "virtualbox" do |vb|
      (1..3).each do |i|
        file = "./disk-#{i}.vdi"
        unless File.exist?(file)
          vb.customize ['createmedium', 'disk', '--filename', file, '--size', 10 * 1024]
        end
        # Comando de storageattach para os 3 discos do LVM
        vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', i, '--device', 0, '--type', 'hdd', '--medium', file]
      end
    end
  end

  # --- Servidor de Banco de Dados (DB) ---
  # Configurado via DHCP com o endereço MAC estático exigido
  config.vm.define "db" do |db|
    db.vm.hostname = "db.pedro.vercosa.devops"
    db.vm.network "private_network", type: "dhcp", adapter: 2, mac: "080027AAAA01", virtualbox__intnet: "rede_projeto"
  end

  # --- Servidor de Aplicação (APP) ---
  # Configurado via DHCP com o endereço MAC estático exigido
  config.vm.define "app" do |app|
    app.vm.hostname = "app.pedro.vercosa.devops"
    app.vm.network "private_network", type: "dhcp", adapter: 2, mac: "080027AAAA02", virtualbox__intnet: "rede_projeto"
  end

  # --- Host Cliente (CLI) ---
  # Hardware diferenciado conforme solicitado (1024 MB de RAM)
  config.vm.define "cli" do |cli|
    cli.vm.hostname = "cli.pedro.vercosa.devops"
    cli.vm.network "private_network", type: "dhcp", adapter: 2, virtualbox__intnet: "rede_projeto"
    cli.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
    end
  end

  # Gatilho para desabilitar o servidor DHCP padrão do VirtualBox (Evita conflitos com o ARQ)
  config.trigger.before :up do |trigger|
    trigger.info = "Garantindo que o DHCP nativo do VirtualBox está desativado..."
    trigger.run = { inline: "bash -c 'VBoxManage dhcpserver remove --netname HostInterfaceNetworking-vboxnet0 || true; VBoxManage dhcpserver remove --netname rede_projeto || true'" }
  end
end
