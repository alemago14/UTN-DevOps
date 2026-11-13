Vagrant.configure("2") do |config|
  # VM Ubuntu
  config.vm.box = "ubuntu/jammy64"
  config.vm.hostname = "utn-devops"

  # Trafico web de la VM (puerto 80) -> puerto 8080 del host
  config.vm.network "forwarded_port", guest: 80, host: 8080

  # Carpeta del proyecto (host) -> /vagrant (VM)
  config.vm.synced_folder ".", "/vagrant"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "utn-devops"
    vb.memory = 1024
    vb.cpus = 1
  end

  # Copia el archivo de configuracion del servidor web desde el host a la VM
  config.vm.provision "file",
    source: "./config/nginx.conf",
    destination: "/tmp/nginx-default.conf"

  # Instala nginx y aplica la configuracion copiada
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y nginx
    cp /tmp/nginx-default.conf /etc/nginx/sites-available/default
    nginx -t
    systemctl restart nginx
  SHELL
end
