Vagrant.configure("2") do |config|
  # Usamos la imagen oficial de Ubuntu 22.04

config.vm.box = "generic/ubuntu2204"
  # Configuración de red (opcional: permite acceder a la VM vía IP estática)
  # config.vm.network "private_network", ip: "192.168.56.10"

  # Aprovisionamiento mediante Shell script
  config.vm.provision "shell", inline: <<-SHELL
    echo "Actualizando el sistema e instalando dependencias..."
    apt-get update -y
    apt-get install -y ca-certificates curl gnupg lsb-release

    echo "Añadiendo la clave GPG oficial de Docker..."
    mkdir -m 0755 -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg

    echo "Configurando el repositorio de Docker..."
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null

    echo "Instalando Docker Engine..."
    apt-get update -y
    apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

    echo "Configurando permisos para el usuario vagrant..."
    # Esto permite ejecutar comandos docker sin usar 'sudo'
    usermod -aG docker vagrant
config.vm.synced_folder "/home/vboxuser/Desktop/tarea_ampliacion", "/home/vagrant/tarea_compartida"


    echo "Instalación completada. Docker está listo."
  SHELL
  # Configuración de recursos de la VM (ajusta según necesites)
  config.vm.provider :libvirt do |libvirt|
    libvirt.driver = "qemu"
# Redirige el puerto 8080 de tu PC al puerto 80 de la VM
  config.vm.network "forwarded_port", guest: 80, host: 8080

  end
end
