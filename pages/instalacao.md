---
layout: page
title: "Instalação do LXD"
---

> A instalação do LXD pode variar conforme a distribuição do Linux utilizada. Abaixo temos as instruções para as principais distribuições.

---

### Sumário de Distribuições

- [Sumário de Distribuições](#sumário-de-distribuições)
- [**Ubuntu (20.04, 22.04, 24.04)**](#ubuntu-2004-2204-2404)
  - [Instalação via Snap (recomendado)](#instalação-via-snap-recomendado)
- [**Distribuições Baseadas em Debian (ex: Debian 12)**](#distribuições-baseadas-em-debian-ex-debian-12)
  - [Instalação via APT (sem Snap)](#instalação-via-apt-sem-snap)
  - [Instalação via Snap (alternativa)](#instalação-via-snap-alternativa)
- [**Fedora**](#fedora)
- [**CentOS 9**](#centos-9)
- [**Outras Distribuições (Arch, OpenSUSE, etc.)**](#outras-distribuições-arch-opensuse-etc)
- [**Resumo das Formas de Instalação**](#resumo-das-formas-de-instalação)
- [**Notas Importantes**](#notas-importantes)
  - [Inicialização do LXD](#inicialização-do-lxd)
  - [Permissões de usuário](#permissões-de-usuário)
- [Para voltar ao tutorial clique aqui](#para-voltar-ao-tutorial-clique-aqui)

---

### **Ubuntu (20.04, 22.04, 24.04)**

#### Instalação via Snap (recomendado)

1. Atualize o sistema e instale o Snapd (caso ainda não tenha):
   ```bash
   sudo apt update
   sudo apt install snapd -y
   ```
2. **Aguarde de 30 segundos a 2 minutos após instalar o Snapd antes de prosseguir.**
3. Instale o LXD usando o Snap:
   ```bash
   sudo snap install lxd
   ```
4. Inicialize o LXD:
   ```bash
   sudo lxd init
   ```
   > Para gerenciar o LXD sem `sudo`, consulte a seção [permissões de usuário](#permissões-de-usuário).

**❗ Após concluir a instalação e inicialização, siga para [inicialização do LXD](#inicialização-do-lxd).**

---

### **Distribuições Baseadas em Debian (ex: Debian 12)**

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
4. Inicialize o LXD:
   ```bash
   sudo lxd init
   ```
   > Para gerenciar o LXD sem `sudo`, consulte a seção [permissões de usuário](#permissões-de-usuário).

**❗ Após concluir a instalação e inicialização, siga para [inicialização do LXD](#inicialização-do-lxd).**

---

#### Instalação via Snap (alternativa)

1. Instale o Snapd (caso não tenha rodado o passo de APT):
   ```bash
   sudo apt install snapd -y
   ```
2. **Aguarde de 30 segundos a 2 minutos após instalar o Snapd antes de prosseguir.**
3. Instale o LXD via Snap:
   ```bash
   sudo snap install lxd
   ```
4. Inicialize o LXD:
   ```bash
   sudo lxd init
   ```
   > Para gerenciar o LXD sem `sudo`, consulte a seção [permissões de usuário](#permissões-de-usuário).

**❗ Após concluir a instalação e inicialização, siga para [inicialização do LXD](#inicialização-do-lxd).**

---

### **Fedora**

1. Instale o Snapd:
   ```bash
   sudo dnf install snapd -y
   sudo systemctl enable --now snapd.socket
   sudo ln -s /var/lib/snapd/snap /snap
   ```
2. **Aguarde de 30 segundos a 2 minutos após instalar o Snapd antes de prosseguir.**
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

**❗ Após concluir a instalação e inicialização, siga para [inicialização do LXD](#inicialização-do-lxd).**

---

### **CentOS 9**

1. Instale o Snapd:
   ```bash
   sudo dnf update -y
   sudo dnf install snapd -y
   sudo systemctl enable --now snapd.socket
   sudo ln -s /var/lib/snapd/snap /snap
   ```
2. **Aguarde de 30 segundos a 2 minutos após instalar o Snapd antes de prosseguir.**
3. Instale o LXD via Snap:
   ```bash
   sudo snap install lxd
   ```
4. Inicialize o LXD:
   ```bash
   sudo lxd init
   ```
   > Para gerenciar o LXD sem `sudo`, consulte a seção [permissões de usuário](#permissões-de-usuário).

**❗ Após concluir a instalação e inicialização, siga para [inicialização do LXD](#inicialização-do-lxd).**

---

### **Outras Distribuições (Arch, OpenSUSE, etc.)**

- O método universal é instalar o Snapd (seguindo as instruções para sua distribuição) e depois instalar o LXD via Snap:
  ```bash
  sudo snap install lxd
  ```
- **Aguarde de 30 segundos a 2 minutos após instalar o Snapd antes de prosseguir.**
- Consulte a documentação oficial do Snapd para detalhes específicos de cada distribuição.
  > Para gerenciar o LXD sem `sudo`, consulte a seção [permissões de usuário](#permissões-de-usuário).

**❗ Após concluir a instalação e inicialização, siga para [inicialização do LXD](#inicialização-do-lxd).**

---

### **Resumo das Formas de Instalação**

| Distribuição       | Método recomendado | Comando principal              |
| ------------------ | ------------------ | ------------------------------ |
| Ubuntu             | Snap               | `sudo snap install lxd`        |
| Debian             | APT ou Snap        | `sudo apt install lxd` ou Snap |
| Fedora             | Snap               | `sudo snap install lxd`        |
| CentOS             | Snap               | `sudo snap install lxd`        |
| Arch/OpenSUSE/etc. | Snap               | `sudo snap install lxd`        |

---

### **Notas Importantes**

#### Inicialização do LXD

Após concluir a instalação, execute:

```bash
sudo lxd init
```

Durante o assistente, você deverá responder perguntas como:

- **Clustering:**  
  _Would you like to use LXD clustering?_  
  Responda `no` se não for usar múltiplos servidores LXD.

- **Storage pool:**  
  _Do you want to configure a new storage pool?_  
  Responda `yes` para criar um novo pool.  
  _Name of the new storage pool:_  
  Exemplo: `default` ou `rpool`  
  _Name of the storage backend to use (lvm, btrfs, ceph, dir, etc):_  
  Exemplo: `btrfs` (ou `zfs`, `dir`, etc).

- **MAAS:**  
  _Would you like to connect to a MAAS server?_  
  Responda `no` (a menos que use MAAS).

- **Rede:**  
  _Would you like to create a new local network bridge?_  
  Responda `yes` para criar a bridge padrão.  
  _What should the new bridge be called?_  
  Exemplo: `lxdbr0`  
  _What IPv4 address should be used?_  
  Exemplo: `auto`  
  _What IPv6 address should be used?_  
  Exemplo: `auto`

- **Acesso remoto:**  
  _Would you like the LXD server to be available over the network?_  
  Responda `no` (a menos que precise acesso remoto).

- **Atualização de imagens:**  
  _Would you like stale cached images to be updated automatically?_  
  Responda `yes` para manter imagens atualizadas.

- **Preseed YAML:**  
  _Would you like a YAML "lxd init" preseed to be printed?_  
  Responda `no` (a menos que queira automatizar instalações futuras).

#### Permissões de usuário

Para gerenciar contêineres sem usar `sudo`, adicione o seu usuário ao grupo `lxd` e aplique as novas permissões:

```bash
sudo usermod -aG lxd $USER
newgrp lxd
```

- O Snap fornece sempre a versão mais recente do LXD, enquanto os repositórios das distribuições podem oferecer versões LTS ou defasadas.

---

### Para voltar ao tutorial [clique aqui](../index.md)
