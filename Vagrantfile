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


  # Instala nginx y git, aplica la config y despliega el sitio desde Git
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y nginx git

    # Clonar el repo si no existe; si existe, actualizarlo
    if [ -d /var/www/sitio/.git ]; then
      git -C /var/www/sitio pull
    else
      git clone https://github.com/alemago14/UTN-DevOps-html /var/www/sitio
    fi

    # Aplicar la configuracion de nginx copiada desde el host
    cp /tmp/nginx-default.conf /etc/nginx/sites-available/default
    nginx -t
    systemctl restart nginx
  SHELL
 nginx
  SHELL
end
