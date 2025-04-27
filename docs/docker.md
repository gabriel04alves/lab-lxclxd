### **Diferenças funcionais**

- O Docker é focado em executar aplicações isoladas (geralmente um processo por container);
- O LXC/LXD é voltado para virtualização de sistemas operacionais completos (similar a VMs, mas mais leve);
- Containers LXD executam um sistema init completo e podem gerenciar múltiplos processos;
- Containers Docker são projetados para executar preferencialmente um único processo;
- Testes de benchmarking mostram que o LXD supera o Docker em aplicações que dependem de largura de banda de memória (Memory Bound);
- Em testes com o DBench, o LXD apresentou desempenho superior em até 85% em comparação ao Docker para acesso simultâneo ao sistema de arquivos.

---

### **Limitações observadas**

- O Docker possui um processo de instalação mais simples, geralmente executando apenas um script;
- O LXD requer um processo de instalação e configuração mais minucioso;
- Para usuários acostumados com Docker, há um período de adaptação para entender o funcionamento e os comandos do LXD;
- A diferença de paradigma (aplicação vs. sistema) também requer uma mudança na forma de pensar a utilização dos contêineres.

---
