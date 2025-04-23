---
layout: home
title: "Implementação e Análise de Contêineres LXC/LXD"
---

**Autores:** <a href="">Gabriel Alves</a>, <a href="">Gabriel L. Pereira</a>, <a href="">Maria Eduarda N. Ferreira</a>

> **⚠️ AVISO:**  
> Este guia foi elaborado para sistemas Linux. Em plataformas com outros kernels, é necessário executar o Linux dentro de uma máquina virtual.

## 1. Introdução

O LXC (Linux Containers) é uma tecnologia de virtualização em nível de sistema operacional que permite a execução de múltiplos ambientes Linux isolados em um único host. Diferentemente das máquinas virtuais tradicionais, o LXC compartilha o kernel do sistema hospedeiro, resultando em uma virtualização mais leve e eficiente.

O LXD, por sua vez, é uma camada de gerenciamento desenvolvida pela Canonical (empresa responsável pelo Ubuntu) que aprimora e expande as funcionalidades do LXC. O LXD funciona como um daemon que gerencia os contêineres, enquanto o LXC atua como o cliente. Esta arquitetura torna o sistema mais adequado para ambientes de produção, oferecendo recursos avançados como isolamento de recursos, migração de contêineres, snapshots e backups.

A história do uso de contêineres começa com o conceito de virtualização nos anos 1960, mas foi apenas em 2001 que surgiu a primeira solução robusta de contêiner, o Jails, no FreeBSD. Em 2008, o Linux passou a incluir a tecnologia LXC por padrão no Kernel. Posteriormente, em 2013, foi criado o Docker como uma camada sobre o LXC, e a Canonical lançou o LXD como sua solução própria de contêiner.

## 2. Pré-requisitos

Para garantir que a instalação ocorra corretamente, verifique os itens antes de prosseguir:

### Sistema Operacional

- **Kernel Linux 4.18 ou superior**
  - Versões LTS recomendadas: Ubuntu 18.04+, Debian 10+, Fedora 31+.
- **Arquitetura x86_64** com **VT-x/AMD-V** habilitado no BIOS/UEFI.
- **Outras distribuições**: adapte o gerenciador de pacotes (zypper, pacman etc.).

### Hardware

- **CPU:** ≥ 2 núcleos (≥ 4 núcleos recomendados em ambientes de produção).
- **Memória RAM:** ≥ 2 GB (≥ 4 GB para múltiplos containers).
- **Armazenamento:** ≥ 20 GB livres (reserve espaço extra para imagens, snapshots e volumes).

## 3. Instalação

> #### Para instruções detalhadas de instalação do LXD/LXC em diferentes distribuições Linux, consulte [esse arquivo](/pages/instalacao.md).

## 4. Execução de um Container

### Criação de um container Ubuntu

Para criar um container Ubuntu padrão:

```bash
lxc launch ubuntu: meu-container
```

**Explicação dos parâmetros:**

- `launch`: Comando para criar e iniciar um novo container
- `ubuntu:`: Especifica a imagem a ser usada (neste caso, a versão mais recente do Ubuntu)
- `meu-container`: Nome personalizado para o container

Ao executar este comando pela primeira vez, o sistema baixará a imagem do Ubuntu, o que pode levar alguns minutos dependendo da sua conexão.

### Listar containers

Para visualizar todos os containers criados:

```bash
lxc list
```

**Saída esperada:**

```
+---------------+---------+---------------------+------+------------+-----------+
|     NAME      |  STATE  |        IPV4         | IPV6 |    TYPE    | SNAPSHOTS |
+---------------+---------+---------------------+------+------------+-----------+
| meu-container | RUNNING | 10.103.53.1 (eth0)  |      | PERSISTENT | 0         |
+---------------+---------+---------------------+------+------------+-----------+
```

Esta saída mostra o nome do container, seu estado atual (RUNNING/STOPPED), endereço IP, tipo e informações sobre snapshots.

### Executar comandos em um container

Para executar um comando dentro do container:

```bash
lxc exec meu-container -- [comando]
```

Por exemplo, para verificar a versão do sistema operacional no container:

```bash
lxc exec meu-container -- cat /etc/os-release
```

**Explicação dos parâmetros:**

- `exec`: Executa um comando dentro do container
- `meu-container`: Nome do container alvo
- `--`: Separador que indica o início do comando a ser executado no container
- `[comando]`: O comando que será executado dentro do container

### Acessar o shell do container

Para obter um shell interativo no container:

```bash
lxc exec meu-container -- bash
```

Use `exit` para sair do shell do container.

### Controle do ciclo de vida do container

- **Parar um container:**

```bash
lxc stop meu-container
```

- **Iniciar um container:**

```bash
lxc start meu-container
```

- **Excluir um container:**

```bash
lxc delete meu-container
```

Para excluir um container em execução, adicione a flag `--force`:

```bash
lxc delete meu-container --force
```

### Criar containers de outras distribuições

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

**Explicação dos parâmetros:**

- `images:centos/7`: Especifica a imagem do CentOS 7 do repositório remoto "images"
- `meu-centos`: Nome personalizado para o container

## 5. Comparação com Docker (prática)

### Diferenças funcionais

1. **Foco e propósito:**

   - O Docker é focado em executar aplicações isoladas (geralmente um processo por container)
   - O LXC/LXD é voltado para virtualização de sistemas operacionais completos (similar a VMs, mas mais leve)

2. **Sistema Init e múltiplos processos:**

   - Containers LXD executam um sistema init completo e podem gerenciar múltiplos processos
   - Containers Docker são projetados para executar preferencialmente um único processo

3. **Performance em testes específicos:**
   - Testes de benchmarking mostram que o LXD supera o Docker em aplicações que dependem de largura de banda de memória (Memory Bound)
   - Em testes com o DBench, o LXD apresentou desempenho superior em até 85% em comparação ao Docker para acesso simultâneo ao sistema de arquivos

### Limitações observadas

1. **Compatibilidade com Sistemas Operacionais:**

   - O Docker é compatível com diversos sistemas operacionais populares para servidores
   - O LXD é principalmente compatível com distribuições baseadas em Debian, o que limita seu uso em outros ambientes

2. **Facilidade de uso:**

   - O Docker possui um processo de instalação mais simples, geralmente executando apenas um script
   - O LXD requer um processo de instalação e configuração mais minucioso

3. **Curva de aprendizado:**
   - Para usuários acostumados com Docker, há um período de adaptação para entender o funcionamento e os comandos do LXD
   - A diferença de paradigma (aplicação vs. sistema) também requer uma mudança na forma de pensar a utilização dos contêineres

## 6. Conclusão

### O que aprendemos com o laboratório

Através deste laboratório prático, pudemos compreender a tecnologia LXC/LXD e suas particularidades. Aprendemos que o LXD oferece uma abordagem de virtualização mais completa que o Docker, sendo mais próxima de uma máquina virtual tradicional, mas com a leveza dos contêineres.

Os testes práticos nos permitiram verificar que o LXD apresenta vantagens significativas de desempenho em cenários específicos, especialmente em aplicações que dependem intensamente da memória e acesso ao sistema de arquivos. A experiência de criação e gerenciamento de contêineres mostrou a versatilidade da ferramenta, permitindo a execução de sistemas Linux completos de diferentes distribuições.

Também constatamos que, embora tenha uma curva de aprendizado inicial um pouco mais acentuada que o Docker, o LXD oferece recursos poderosos que justificam o investimento no aprendizado, especialmente para casos de uso onde um sistema operacional completo é necessário.

### Sugestões de uso futuro

1. **Ambientes de desenvolvimento isolados**: O LXD é ideal para criar ambientes de desenvolvimento que reproduzem fielmente o ambiente de produção, sem o overhead de máquinas virtuais tradicionais.

2. **Laboratórios didáticos**: Como demonstrado no trabalho do CTISM, o LXD pode ser uma excelente solução para laboratórios virtuais em instituições de ensino, permitindo aos estudantes trabalharem em ambientes Linux completos de forma segura e isolada.

3. **Consolidação de servidores**: Em ambientes empresariais, o LXD pode ser usado para consolidar múltiplos servidores físicos em um único host, reduzindo custos de hardware e energia.

4. **Testes de sistemas operacionais**: A facilidade de criar contêineres com diferentes distribuições Linux faz do LXD uma ferramenta valiosa para testar aplicações em diversos ambientes sem precisar de múltiplas máquinas virtuais.

5. **Serviços de acesso simultâneo**: Com base nos resultados de benchmarking, o LXD seria a melhor opção para serviços que exigem acesso simultâneo ao sistema de arquivos, como servidores web com alto tráfego ou serviços de banco de dados.

O LXC/LXD representa uma alternativa robusta ao Docker para casos de uso que necessitam de sistemas operacionais completos, oferecendo um equilíbrio entre o isolamento e funcionalidade das máquinas virtuais e a eficiência dos contêineres.

## 7. Referências:

- https://sol.sbc.org.br/index.php/erbase/article/download/8990/8891/
- https://tech.4u.pt/lxc-criar-o-primeiro-container/
- https://www.tabnews.com.br/gustavohenrique/lxd-linux-container-como-alternativa-ao-docker
- https://napoleon.com.br/glossario/o-que-e-lxd-lxc-com-suporte-estendido/
- https://documentation.ubuntu.com/lxd/en/latest/tutorial/first_steps/
- https://sol.sbc.org.br/index.php/errc/article/view/26006
- https://prof.valiante.info/aulas/arquitetura-de-computadores/m%C3%A1quinas-virtuais-e-containers/microtutorial-de-containers-lxclxd
- https://luizsouza.com/2018/12/08/iniciando-os-trabalhos-com-lxd/
- https://www3.livrariacultura.com.br/practical-lxc-and-lxd-111620395/p
- https://www.reddit.com/r/linux4noobs/comments/kf4cxz/lxdlxc_is_such_a_great_technology_why_it_isnt/?tl=pt-br
- https://www.hostnet.com.br/blog/linux-containers-lxc-o-que-e-e-quais-as-suas-vantagens/
- https://ubuntu.com/blog/lxd-vs-docker
- https://www.ufsm.br/app/uploads/sites/495/2019/05/2015-Bruno-Rizzetti.pdf
- https://www.ic.unicamp.br/~william/howto/lxd.html
- https://www.zabbix.com/br/integrations/lxc
- https://pt.ubunlog.com/Introdu%C3%A7%C3%A3o-%C3%A0-instala%C3%A7%C3%A3o-de-cont%C3%AAineres-lxd/
- https://sol.sbc.org.br/index.php/erbase/article/view/8990/8891
- https://www.funtoo.org/LXD/pt-br
- https://ubuntu.com/tutorials/how-to-setup-a-basic-lxd-cluster
- https://blog.purestorage.com/purely-educational/lxc-vs-lxd-linux-containers-demystified/
