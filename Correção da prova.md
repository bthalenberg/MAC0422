# Correção da prova

1) A memória virtual funciona por causa da TLB. Ela funciona por causa do princípio da localidade. Existem 3 tipos:
   - TLB usando todo o endereço da página (grande e cara).
   - TLB usando apenas os primeiros bits (pequena, mas limitada).
   - TLB com vários conjuntos, com primeiros bits dizendo qual é (TLB n-associativa).

2) A memória virtual global teria os mesmos problemas de tamanho e dificuldade de alocação da memória real, mas com swapp usando a memória virtual. Poderíamos ter os mesmos problemas de  fragmentação externa/interna. Para lidar com isso, é possível usar duas técnicas:
   - Tabela de páginas com múltiplos níveis.
   - Índice invertido.

3) O índice invertido é uma tabela com o tamanho da memória real dividida pelo tamanho das páginas. Cada uma está associada a uma página. A busca é mais lenta (linear), mas funciona por causa da TLB.

4) A Intel faz uma limitação: usam-se 48 bits para endereçar a memória virtual, mas são aceitos até 52 bits. Assim, seria possível criar páginas de 4 Petabytes, mas a limitação deixa apenas 256 Terabytes.

5) O Working Set assume que um mesmo programa usa várias páginas. Se ele tiver uma janela de certo tamanho fixo, ele observa as n últimas páginas usadas para mantê-las na TLB.

6) O tamanho usado na memória pode, na realidade, depender do caso:
   - A FAT tem entradas para todos os blocos, com uma lista ligada indicando os arquivos.
   - O i-node mantém um certo número de blocos nele, e depois um link para outras tabelas, dependendo do tamanho.

   Se tivermos muitos arquivos abertos, pode ser que haja mais i-nodes que haveria para uma tabela FAT.

   Para acesso aleatório, o i-node é pior, quando temos vários níveis deles.

7) 4 casos de uso de princípio da localidade:
   - Memória virtual
   - i-nodes
   - TLB com múltiplos níveis
   - Caches

8) RAIDs:
   - 0:  para aumentar a capacidade do sistema de arquivos.
   - 1:  para replicar dados.
   - 5:  uma maneira de manter bits de paridade e blocos distribuídos, recuperando um disco em caso de falha.
   - 10: mistura do RAID 0 e 1, aumentando a capacidade do sistema com replicação.

9) Algoritmo do banqueiro:
   Só aloca recursos se, dando-os, houver uma maneira de todos os processos terminarem (caso todos usem o máximo de recursos).

10) Unix:
    Todo programa tem uma área de usuário e uma área de sistemas.
    Fazer uma chamada de sistemas é, apenas, uma chamada de  procedimento, sendo necessário apenas garantir que ela é permitida.

    Minix:
    Temos vários níveis: ler um arquivo exige enviar mensagens para o FS, e depois para o driver, e então para o System Task. A resposta vai no mesmo caminho ao contrário.

11) System Task: serve de interface para mecanismos de dentro do Kernel, comumente usados por processos do sistema que precisam mexer com áreas de fora de seus espaços de memória. (ex: é usado para PM, FS e Kernel manterem suas tabelas sincronizadas, pois só o Kernel pode mexer no FS para o PM, e vice-e-versa).

12) Windows:
    Tem campos na tabela FAT, mais o nome do arquivo, bits de proteção, e outros.

    Linux/Minix:
    Tem apenas o nome simbólico do arquivo e o número do i-node.

13) O software independente de dispositivo é o sistema de arquivos. Suas principais funções são: dar nomes simbólicos para os arquivos, ter independência de disponitivo, permitir segurança e backup, etc.