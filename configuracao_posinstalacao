#!/bin/bash

#script criado por: Sérgio ricardo
#data: 07/10/2021 --- baseado na aula do curso do diolinuxplay

#adiciona o repositorio flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

#adiciona o ppa do lutris
PPA_LUTRIS="ppa:lutris-team/lutris"

# variavel para instalar pacote Deb
PROGRAMAS_PARA_INSTALAR_DEB=(
  https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
  https://github.com/Automattic/simplenote-electron/releases/download/v1.8.0/Simplenote-linux-1.8.0-amd64.deb
  https://d2t3ff60b2tol4.cloudfront.net/builds/insync_3.0.20.40428-bionic_amd64.deb
  https://atom.io/download/deb
  https://download.teamviewer.com/download/linux/teamviewer_amd64.deb
)

# Variavel para criação de Diretorio

DIRETORIO_PROGRAMAS="$HOME/Downloads/Baixados"

#INSTALA PROGRAMA APT
PROGRAMAS_PARA_INSTALAR_APT=(
snapd
virtualbox
flatpak
)

#INSTALA PROGRAMA SNAP
PROGRAMA_SNAP=(
spotify
vlc
telegram-desktop
notepad-plus-plus

)

#INSTALA PROGRAMA FLATPAK
PROGRAMA_FLATPAK=(
Discord
inkscape
AnyDesk
OBS Studio
)

VERMELHO='\e[1;91m'
VERDE='\e[1;92m'
SEM_COR='\e[0m'


atualizar_sistema () {
  sudo apt update -y # processo de atualização
}

instalar_pacote_net-tools () {
  sudo apt install net-tools #instala o comando ifconfig
}
instalar_openssh(){
  sudo apt install openssh-server #instala o comando openssh
}

remover_locks () {
  sudo rm /var/lib/dpkg/lock-frontend
  sudo rm /var/cache/apt/archives/lock #remove os lock
}

Adicionar_arquiterura_32bits () {
  sudo dpkg --add-architecture i386 #libera a arquiterura 32bits
}

atualizar_repositorios () {
  echo -e "${VERDE}[INFO] - Atualizando repositórios...${SEM_COR}"
  sudo apt update &> /dev/null
}

adicionar_ppas () {
  echo -e "${VERDE}[INFO] - Adicionando PPAs...${SEM_COR}"
  sudo add-apt-repository "$PPA_LUTRIS" -y &> /dev/null
  atualizar_repositorios
}

lista_atualizacao () {
  sudo apt list --upgradable #lista os pacotes a serem atualizados
}

baixar_pacotes_deb () {
  [[ ! -d "$DIRETORIO_PROGRAMAS" ]] && mkdir "$DIRETORIO_PROGRAMAS" #cria o diretorio caso ele não exixta !!!
for url in ${PROGRAMAS_PARA_INSTALAR_DEB[@]};do #vai verificar a lista de url para baixar
  url_extraida=$(echo -e ${url##*/} | sed 's/-/_/g' | cut -d _ -f 1) #quebra o link da url para somente o nome do pacote e converte traços dos nomes para anderline
 if ! dpkg -l | grep -iq $url_extraida; then # lista os pacotes baixados
   wget -c "$url" -P "$DIRETORIO_PROGRAMAS" # confere se ja foi baixado  e se sim nao faz o download
   sudo dpkg -i $DIRETORIO_PROGRAMAS/${url##*/} # faz uma nova verificação de pacotes no diretorio
 else
   echo "[INFO] O programa $url_extraida já está instalado." #informa de o programa foi instalado
   fi
 done
 sudo apt -f install -y #instala as dependencias que falta no pacote
}


instalar_pacote_apt () {
for programa in ${PROGRAMAS_PARA_INSTALAR_APT[@]}; do
    if ! dpkg -l | grep -q $programa; then
    sudo apt install $programa -y
  else echo "[INFO] o pacote $programa já está instalado."
  fi
  done
}

instalar_snap (){
  for programa in ${PROGRAMA_SNAP[@]}; do
      if ! snap list | grep -q $programa; then
      sudo snap install $programa
    else echo "[INFO] o pacote $programa já está instalado."
    fi
    done

}

instalar_flatpak(){
  for programa in ${PROGRAMA_FLATPAK[@]}; do
      if ! flatpak list | grep -q $programa; then
      sudo flatpak install flathub $programa
    else echo "[INFO] o pacote $programa já está instalado."
    fi
    done
}


atua_upgrade (){
  sudo apt upgrade -y # faz a atualização do sistema
  sudo apt autoclean -y # faz a limpeza do sistema
  sudo apt autoremove -y # faz a limpeza de pacote
}

atualizar_sistema
instalar_openssh
instalar_pacote_net-tools
remover_locks
Adicionar_arquiterura_32bits
atualizar_repositorios
adicionar_ppas
lista_atualizacao
baixar_pacotes_deb
instalar_pacote_apt
instalar_snap
instalar_flatpak
atua_upgrade
