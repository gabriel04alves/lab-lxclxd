Através deste laboratório prático, pudemos compreender a tecnologia LXC/LXD e suas particularidades. Aprendemos que o LXD oferece uma abordagem de virtualização mais completa que o Docker, sendo mais próxima de uma máquina virtual tradicional, mas com a leveza dos contêineres.

Os testes práticos nos permitiram verificar que o LXD apresenta vantagens significativas de desempenho em cenários específicos, especialmente em aplicações que dependem intensamente da memória e acesso ao sistema de arquivos. A experiência de criação e gerenciamento de contêineres mostrou a versatilidade da ferramenta, permitindo a execução de sistemas Linux completos de diferentes distribuições.

Também constatamos que, embora tenha uma curva de aprendizado inicial um pouco mais acentuada que o Docker, o LXD oferece recursos poderosos que justificam o investimento no aprendizado, especialmente para casos de uso onde um sistema operacional completo é necessário.

Casos de uso práticos do LXC/LXD:

1. Ambientes de desenvolvimento isolados: O LXD é ideal para criar ambientes de desenvolvimento que reproduzem fielmente o ambiente de produção, sem o overhead de máquinas virtuais tradicionais;

2. Laboratórios didáticos: Como demonstrado no estudo do CTISM, o LXD pode ser uma excelente solução para laboratórios virtuais em instituições de ensino, permitindo aos estudantes trabalharem em ambientes Linux completos de forma segura e isolada;

3. Consolidação de servidores: Em ambientes empresariais, o LXD pode ser usado para consolidar múltiplos servidores físicos em um único host, reduzindo custos de hardware e energia;

4. Testes de sistemas operacionais: A facilidade de criar contêineres com diferentes distribuições Linux faz do LXD uma ferramenta valiosa para testar aplicações em diversos ambientes sem precisar de múltiplas máquinas virtuais;

5. Serviços de acesso simultâneo: Com base nos resultados de benchmarking, o LXD seria a melhor opção para serviços que exigem acesso simultâneo ao sistema de arquivos, como servidores web com alto tráfego ou serviços de banco de dados.

O LXC/LXD representa uma alternativa robusta ao Docker para casos de uso que necessitam de sistemas operacionais completos, oferecendo um equilíbrio entre o isolamento e funcionalidade das máquinas virtuais e a eficiência dos contêineres.

---
