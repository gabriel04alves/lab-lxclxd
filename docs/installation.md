Focaremos apenas na instalação do **LXD**, que o orquestrador de containers. Embora o cliente de linha de comando se chame `lxc`, o que realmente gerenciamos e instalamos aqui é o **servidor LXD**.

Durante o uso do LXD, você irá utilizar a ferramenta de linha de comando chamada `lxc`.  
Apesar do nome, neste contexto o `lxc` é apenas o **cliente que se comunica com o servidor LXD**, e **não** o conjunto de ferramentas LXC tradicional usado diretamente.

> **Observação:** O **LXC** continua presente nos bastidores, é ele que, junto com o kernel do Linux, realiza a criação e o gerenciamento real dos contêineres.  
> O **LXD** apenas facilita a interação, adicionando funcionalidades avançadas como clustering, redes virtuais, armazenamento em pools e APIs REST.

<img src="/images/example-cli.png" alt="CLI layers" style="max-width: 15vw; display: flex; justify-self: center;">

---

### **Ubuntu**

1. Atualize o sistema e instale o Snapd (caso ainda não tenha):
   ```bash
   sudo apt update
   sudo apt install snapd -y
   ```
2. Aguarde de 30 segundos a 2 minutos após instalar o Snapd antes de prosseguir.
3. Instale o LXD usando o Snap:
   ```bash
   sudo snap install lxd
   ```
4. Siga para [inicialização do LXD](#inicialização-do-lxd).

---

### **Debian**

#### Instalação via APT (sem Snap)

1. Atualize o sistema:
   ```bash
   sudo apt update
   sudo apt upgrade -y
   ```
2. Instale o LXD:
   ```bash
   sudo apt install lxd -y
   ```
3. (Opcional) Instale suporte a ZFS, caso queira usar esse backend de armazenamento:
   - Edite o `/etc/apt/sources.list` para incluir `contrib` nos repositórios.
   - Atualize a lista de pacotes e instale o ZFS:
     ```bash
     sudo apt update
     sudo apt install zfs-dkms -y
     sudo reboot
     ```
4. Siga para [inicialização do LXD](#inicialização-do-lxd).

---

#### Instalação via Snap (alternativa)

1. Instale o Snapd (caso não tenha rodado o passo de APT):
   ```bash
   sudo apt install snapd -y
   ```
2. Aguarde de 30 segundos a 2 minutos após instalar o Snapd antes de prosseguir.
3. Instale o LXD via Snap:
   ```bash
   sudo snap install lxd
   ```
4. Siga para [inicialização do LXD](#inicialização-do-lxd).

---

### **Fedora**

1. Instale o Snapd:
   ```bash
   sudo dnf install snapd -y
   sudo systemctl enable --now snapd.socket
   sudo ln -s /var/lib/snapd/snap /snap
   ```
2. Aguarde de 30 segundos a 2 minutos após instalar o Snapd antes de prosseguir.
3. Instale o LXD via Snap:
   ```bash
   sudo snap install lxd
   ```
4. (Opcional) Verifique o status dos serviços do LXD:
   ```bash
   sudo snap services lxd
   ```
5. Inicialize o LXD:
   ```bash
   lxd init
   ```
   > Para gerenciar o LXD sem `sudo`, consulte a seção [permissões de usuário](#permissões-de-usuário).
6. Siga para [inicialização do LXD](#inicialização-do-lxd).

---

### **Manjaro**

1. Atualize o sistema e sincronize os repositórios:
   ```bash
   sudo pacman -Syu
   ```
2. Instale o LXD:
   ```bash
   sudo pacman -S lxd
   ```
3. (Opcional) Instale suporte a ZFS, caso queira usar esse backend:
   ```bash
   sudo pacman -S zfs-utils
   sudo systemctl enable --now zfs-import-cache zfs-mount
   ```
4. Habilite e inicie o serviço do LXD:
   ```bash
   sudo systemctl enable --now lxd.service lxd.socket
   ```
5. Siga para: [Inicialização do LXD](#inicialização-do-lxd).

---

### **CentOS**

1. Instale o Snapd:
   ```bash
   sudo dnf update -y
   sudo dnf install snapd -y
   sudo systemctl enable --now snapd.socket
   sudo ln -s /var/lib/snapd/snap /snap
   ```
2. Aguarde de 30 segundos a 2 minutos após instalar o Snapd antes de prosseguir.
3. Instale o LXD via Snap:
   ```bash
   sudo snap install lxd
   ```
4. Siga para [inicialização do LXD](#inicialização-do-lxd).

---

### **Inicialização do LXD**

1. Execute:

```bash
sudo lxd init
```

> Para gerenciar o LXD sem sudo, consulte a seção [Permissões de usuário](#permissões-de-usuário).

Durante a instalação, responda:

- **Would you like to use LXD clustering?** (yes/no) [default=no]  
  Responda `no` se não for usar múltiplos servidores LXD.

- **Do you want to configure a new storage pool?** (yes/no) [default=yes]  
  Responda `yes` para criar um novo pool.  
  _Name of the new storage pool [default=default]:_  
  Exemplo: `default` ou `rpool`  
  _Name of the storage backend to use (lvm, btrfs, ceph, dir, etc) [default=btrfs]:_  
  Exemplo: `btrfs` (ou `zfs`, `dir`, etc)

- **Would you like to connect to a MAAS server?** (yes/no) [default=no]  
  Responda `no`.

- **Would you like to create a new local network bridge?** (yes/no) [default=yes]  
  Responda `yes`.  
  _What should the new bridge be called? [default=lxdbr0]:_  
  Exemplo: `lxdbr0`  
  _What IPv4 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]:_  
  Responda `auto`  
  _What IPv6 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]:_  
  Responda `none`

- **Would you like the LXD server to be available over the network?** (yes/no) [default=no]  
  Responda `no`.

- **Would you like stale cached images to be updated automatically?** (yes/no) [default=yes]  
  Responda `yes`.

- **Would you like a YAML "lxd init" preseed to be printed?** (yes/no) [default=no]  
  Responda `no`.

---

### **Permissões de usuário**

Para gerenciar contêineres sem usar `sudo`, adicione o seu usuário ao grupo `lxd` e aplique as novas permissões:

```bash
sudo usermod -aG lxd $USER
newgrp lxd
```

---
