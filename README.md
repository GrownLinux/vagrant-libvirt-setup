# Configuración de Vagrant con Libvirt y Conexión a Visual Studio Code

Esta guía detalla cómo configurar Vagrant con Libvirt y conectar Visual Studio Code a la máquina virtual.

## Requisitos Previos

- Ubuntu/Kubuntu (o cualquier distribución basada en Debian)
- Vagrant
- Libvirt y QEMU/KVM
- Visual Studio Code instalado en tu máquina local

## 1. Instalar Libvirt y QEMU/KVM

Ejecuta los siguientes comandos para instalar Libvirt y QEMU/KVM:
sudo apt-get update
sudo apt-get install -y libvirt-daemon-system libvirt-clients qemu-kvm

##2. Instalar Vagrant

Descarga el paquete de Vagrant:
wget https://releases.hashicorp.com/vagrant/2.3.4/vagrant_2.3.4_linux_amd64.deb

Instala Vagrant:

sudo dpkg -i vagrant_2.3.4_linux_amd64.deb



## 3. Instalar el Plugin de Vagrant para Libvirt

Instala las dependencias de desarrollo de Libvirt:

sudo apt-get install -y libvirt-dev

Instala el plugin de Vagrant para Libvirt:

vagrant plugin install vagrant-libvirt



## 4. Configurar el Vagrantfile

Crea un nuevo directorio para tu proyecto y configura el Vagrantfile:

mkdir ~/my-vagrant-project
cd ~/my-vagrant-project

Crea un archivo Vagrantfile con el siguiente contenido:

# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2004"

  config.vm.provider "libvirt" do |libvirt|
    libvirt.memory = 1024
    libvirt.cpus = 2
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2
  SHELL
end


## 5. Iniciar la Máquina Virtual

Levanta la máquina virtual con Libvirt:

vagrant up --provider=libvirt


## 6. Verificar el Estado de la Máquina Virtual

Verifica que la máquina esté en funcionamiento:

vagrant status

Deberías ver algo similar a:

Current machine states:

default                   running (libvirt)



## 7. Conectarse a la Máquina Virtual

Usa el comando vagrant ssh para conectarte a la máquina virtual:

vagrant ssh



## 8. Configurar SSH para VS Code

Ejecuta el comando vagrant ssh-config para obtener los detalles de configuración SSH:


vagrant ssh-config

Usa la salida de este comando para configurar el archivo SSH. Abre (o crea) el archivo ~/.ssh/config y añade una entrada para tu VM Vagrant. Aquí hay un ejemplo de cómo debería verse (ajusta las rutas y detalles según tu salida de vagrant ssh-config):


Host vagrant-vm
  HostName 127.0.0.1
  User vagrant
  Port 2222
  IdentityFile /path/to/your/private_key
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentitiesOnly yes
  LogLevel FATAL


##9. Conectar VS Code a la VM Vagrant

    Abre Visual Studio Code en tu máquina local.
    Instala la extensión "Remote - SSH" desde la sección de extensiones.
    Presiona F1 (o Ctrl+Shift+P en Windows/Linux, Cmd+Shift+P en macOS) para abrir la paleta de comandos.
    Escribe Remote-SSH: Connect to Host... y selecciona esta opción.
    Selecciona vagrant-vm (o el nombre del host que especificaste en tu configuración SSH).

##10. Abrir una Carpeta en la VM

Una vez conectado, podrás abrir una carpeta en tu VM directamente desde VS Code y trabajar como si estuvieras trabajando en tu máquina local.
