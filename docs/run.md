Vamos explorar as operações básicas de criação, gerenciamento e configuração de containers. Cobriremos desde a instalação de um container Ubuntu até o controle do seu ciclo de vida, execução de comandos, testes com uma aplicação web simples e utilização de imagens de diferentes fontes.

---

### **Criação de um container Ubuntu**

Para criar um container Ubuntu padrão:

```bash
lxc launch ubuntu: meu-container
```

> - `launch`: Comando para criar e iniciar um novo container
> - `ubuntu:`: Especifica a imagem a ser usada (neste caso, a versão mais recente do Ubuntu)
> - `meu-container`: Nome personalizado para o container

Ao executar este comando pela primeira vez, o sistema baixará a imagem do Ubuntu, o que pode levar alguns minutos dependendo da sua conexão.

---

### **Listar containers**

Para visualizar todos os containers criados:

```bash
lxc list
```

Saída esperada:

```
+---------------+---------+---------------------+------+------------+-----------+
|     NAME      |  STATE  |        IPV4         | IPV6 |    TYPE    | SNAPSHOTS |
+---------------+---------+---------------------+------+------------+-----------+
| meu-container | RUNNING | 10.103.53.1 (eth0)  |      | PERSISTENT | 0         |
+---------------+---------+---------------------+------+------------+-----------+
```

> Esta saída mostra o nome do container, seu estado atual, endereço IP, tipo e informações sobre snapshots.

---

### **Executar comandos em um container**

Para executar um comando dentro do container:

```bash
lxc exec meu-container -- [comando]
```

Por exemplo, para verificar a versão do sistema operacional no container:

```bash
lxc exec meu-container -- cat /etc/os-release
```

> - `exec`: Executa um comando dentro do container
> - `meu-container`: Nome do container alvo
> - `--`: Separador que indica o início do comando a ser executado no container
> - `[comando]`: O comando que será executado dentro do container

### **Acessar o shell do container**

Para obter acesso shell do container:

```bash
lxc exec meu-container -- bash
```

> Use `exit` para sair do shell do container.

---

### **Controle do ciclo de vida do container**

- Parar um container:

```bash
lxc stop meu-container
```

- Iniciar um container:

```bash
lxc start meu-container
```

- Excluir um container:

```bash
lxc delete meu-container
```

Para excluir um container em execução, adicione a flag `--force`:

```bash
lxc delete meu-container --force
```

---

### **Testando com uma aplicação web simples**

1. Atualizar repositórios e instalar o Nginx
   ```bash
   lxc exec meu-container -- apt update
   lxc exec meu-container -- apt install -y nginx
   ```
2. Configurar o DNS do servidor lxd:
   ```bash
    lxc exec meu-container -- sh -c 'echo "nameserver 8.8.8.8" > /etc/resolv.conf'
   ```
3. Verificar serviço no container
   ```bash
   lxc exec meu-container -- systemctl status nginx
   ```
   Saída esperada:
   ```
   nginx.service - A high performance web server and a reverse proxy server
      Loaded: loaded (/lib/systemd/system/nginx.service; enabled)
      Active: active (running) since ...
   ```
4. Testar localmente dentro do container
   ```bash
   lxc exec meu-container -- curl -I http://localhost
   ```
   Saída esperada:
   ```
   HTTP/1.1 200 OK
   Server: nginx/1.18.0 (Ubuntu)
   ```
5. Descobrir o IP do container
   ```bash
   lxc list meu-container -c4 --format=csv
   ```
   Isso retorna algo como `10.103.53.1`.
6. Expor o servidor Nginx
   ```bash
   lxc config device add meu-container nginx-port80 proxy listen=tcp:0.0.0.0:8081 connect=tcp:127.0.0.1:80
   ```
7. Acessar o servidor web a partir do host  
   No seu host, abra um navegador ou use `curl`:
   ```bash
   curl http://localhost:8081
   ```
   Você deverá ver o HTML padrão do Nginx (“Welcome to nginx!”).

### **Criar containers de outras distribuições**

Para listar as imagens disponíveis no servidor oficial:

```bash
lxc image list images:
```

Para filtrar por uma distribuição específica:

```bash
lxc image list images: | grep centos
```

Para criar um container CentOS 7:

```bash
lxc launch images:centos/7 meu-centos
```

> - `images:centos/7`: Especifica a imagem do CentOS 7 do repositório remoto "images"
> - `meu-centos`: Nome personalizado para o container

### **Outras Fontes de Imagens LXC/LXD**

Além do repositório padrão `ubuntu:` e `images:`, você pode usar outras fontes de imagens:

- **suse:** imagens oficiais da SUSE Enterprise

  ```bash
  lxc image list images:suse/15.2
  lxc launch images:suse/15.2 meu-suse
  ```

- **snapshots e mirrors privados:** você mesmo pode hospedar um LXD Image Server e adicionar como remoto

  ```bash
  lxc remote add meu-servidor https://meu-lxd-images.local
  lxc image list meu-servidor:
  ```

- **Registro Docker Hub (via importação):** caso queira converter uma imagem Docker em LXD
  ```bash
  lxc image import docker://nginx --alias nginx-docker
  lxc launch nginx-docker meu-nginx-docker
  ```

---
