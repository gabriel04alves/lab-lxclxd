# Lab LXC/LXD

Este repositório contém um laboratório prático sobre Linux Containers (LXC) e Linux Container Daemon (LXD). O objetivo é ensinar, de forma didática, como instalar, configurar e gerenciar containers Linux utilizando o LXD, além de comparar suas funcionalidades com o Docker.

## Conteúdo

- Introdução ao LXC/LXD
- Pré-requisitos de hardware e software
- Instalação do LXD em diferentes distribuições Linux
- Execução e gerenciamento de containers
- Comparação entre LXD e Docker
- Exemplos práticos e casos de uso

## Como rodar localmente

Este projeto utiliza [MkDocs](https://www.mkdocs.org/) com o tema Material para documentação.

### Pré-requisitos

- Python 3.7+
- `pip` instalado

### Instalação

1. Instale o MkDocs e o tema Material:

   ```bash
   pip install mkdocs-material
   ```

2. Clone este repositório:

   ```bash
   git clone https://github.com/gabriel04alves/lab-lxdlxc.git
   cd lxc-lab-lxdlxc
   ```

### Rodando localmente

Execute o servidor de desenvolvimento do MkDocs:

```bash
mkdocs serve
```

Acesse a documentação em [http://localhost:8000](http://localhost:8000) no seu navegador.

### Gerando versão estática

Para gerar os arquivos HTML estáticos:

```bash
mkdocs build
```

Os arquivos serão gerados na pasta `site/`.

---
