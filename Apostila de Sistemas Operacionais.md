# Apostila de Sistemas Operacionais

Esse documento é apenas uma reformatação das anotações de aula do Renato Cordeiro Ferreira (@[renatocf](https://github.com/renatocf/MAC0422)).

## Informações

### Professor
Alan
aland@usp.br
sala 201 - Bloco C

### Composição da média

3 provas:

| 15% P1  |  15% P2  | 70% P3 |
| ----- | ------- | ------- | 
| (opcional) | (opcional) | (obrigatória) |
| (data) |     (data)  |    (data) |

Não fazer a prova opcional aumenta o peso da última prova.

Média: Média = (2MP + NEP)/3

3 eps - escalonamento, memória, entrada/saída

### Bibliografia

* Operating	Systems: Design	and	Implementation.	A. Tanembaum, A. Woodhul. (3a	edição)	
* Sistemas	Operacionais. A. Silberschatz, P. Galvin --	ótimo livro de	referência.	
* Operating	Systems. H.M. Deitel – livro muito fácil de	ler	e muito	claro; a primeira e segunda	edição tem estudos de caso de	sistemas antigos que valem a pena serem lidos.	


-------------------------------------------------

## História dos Sistemas Operacionais

De forma geral, a Computação repete invenções antigas. No caso dos SOs, houve a ascensção dos servidores, a substituição por computadores pessoais e, agora, a volta dos servidores com sistemas de nuvens.

### Linha do tempo:

* Babbage (1792-1871): Máquina de calcular mecânica
* Compudator moderno (1940): 
    - Cálculos para construir a primeira bomba atômica
    - John von Neumann (Princeton/Harvard)
    - Não havia ainda sistema operacional (programação era feita redirecionando os circuítos entre portas lógicas do hardware)
    - Programação = "Redrawing"
* Transistores (1960)
    - Transistores substituiíram as válvulas, o que permitiu construir sistemas mais complexos e programáveis.
    - Programação em binário.
    - Começou a fazer sentido criar um conjunto de rotinas que automatizassem comandos comuns de entrada e saída. Essa agregação gerou os primeiros SOs (que eram bibliotecas).
    - O primeiro SO eram o dos batches, que incluíam um sistema de controle pararodar os processos.
    - As informações eram guardadas em fitas magnéticas, e vários computadores eram responsáveis por cada função (entrada, saída).
    - A saída padrão eram impressoras ou fitas magnéticas.
    - A entrada padrão era o teclado, mas mais para o operador.
    - Quando criou-se o sistema batch, ele funcionava como um aviso para os programadores.
    - Depois, descobriu-se que a maiorai dos programas perdiam mais tempo em entrada e saída. Por isso, surgiram os computadores multiprogramados - que aceitavam vários programas, cada um fazendo alguma modificação.
    - Surgiram linguagens de alto nível, que convertiam para assembly: LISP (1958), FORTRAN (1954), ALGOL (1958).
    - 2 tipos de computadores: 
        -> Científicos: com Fortran, voltados a processamento
        -> Comerciais: com Cobol, voltados à entrada e saída  
* Circuitos integrados
    - A IBM criou a série IBM/360 - uma série de computadores com circuítos integrados que tinham compatibilidade na linguagem de máquina: um programa compilado em um rodaria em outro.
    - Multics: sistema operacional que permitiria atender vários usuários ao mesmo tempo (processamento científico, iterativo, comercial, etc). Ele criou um sistema de prioridades que permitia distribuir o usoda CPU entre os computadores. O nome Unix veio de uma brincadeia com o nome Multics.
    - O ideal, nesse momento, é que a CPU trabalhasse 100% a todo o momento.
    - Criou-se um sistema que permitia memória virtual - o usuário não precisava olhar mais para um endereço no disco diretamente, pois quem fazia isso era o hardware.
    - Key Thompson criou o UNICS, uma versão simplificada do MULTICS.
    - David Ritchie cria a linguagem C e rescreve o UNICS na sua linguagem: nasce o UNIX.
    - Steve Jobs e Steve Wozniack criaram uma plataforma de slots de tamanho padronizado, que permitiam fazer substituições. Esse foi o lançamento da Apple, o Apple I.
    - Steve Jobs criou o Lisa, de arquitetura fechada. A IBM criou uma arquiteturade 16 bits aberta, e o SO para ela foi o MS-DOS, futuramente o Windows.
    - Os computadores carregavam seus programas usando disquetes.    
* Sistemas distribuídos
    - Acreditou-se que os servidores morreriam, substituídos por vários computadores em rede integrados.
    - O conjunto de instruções crescia cada vez mais - CISC
    - A Sun fez, então, um computador que tinha o contrário: reduziu o conjunto de instruções (RISC), com várias instruções pequenas e de rápida execução, facilmente otimizáveis pelos compiladores.
    - O sistema distribuído era uma forma de associar em que era  transparente a existência dos computadores.
    - A área de compiladores e bancos de dados se separa sistemas operacionais, e se dessenvolveram independentemente.
* MINIX: 
    - primeira versão do UNIX para PC;
    - gratuíto;
    - voltado para o ensino de sistemas operacionais;
    - em 1957, a AT&T fechou o código do UNIX. O MINIX foi criado com a mesma interface do UNIX para ajudar no ensino de computação;
    - muito modular (drivers rodam fora do kernel);
    - foi feito para ser lido, não para ser eficiente;
    - compatível com a versão 7 do UNIX.

-------------------------------------------------------

## Arquitetura do computador

O computador modero é composto de várias partes:

* **Barramento:** Canal de comunicação entre componentes do computador
* **CPU:** Processamento e controle entre componentes
* **Memória:** Memória secundária não durável
* **Controlador de Vídeo:** Faz a interface com o monitor (foi introduzido pela Xerox na década de 1980)
* **Controlador de Teclado:** Componente para entrada
* **Controlador do HD:** Disco Rígido durável

A velocidade de acesso entre memórias é bem diferente. A memória interna (cache), muito rápido e em vários níveis, a memória RAM e o disco rígido (muito lento)

* Barramento
    - Trilhas comunicando uma ou mais dispositivos apenas uma mensagem de cada vez
* Processador
    - Contador, pilha, PSW (prog. Status word), modo usuário e privilegiado (algumas instruções só podem ser executadas no modo de supervisão), multicore x singlecore (um core apenas será usado para a maioria dos algoritmos)
    - Trap (interrupção): sinal eletrônico que faz o proessador entrar em modo privilegiado e muda o programa para um endereço pré-
      especificado. Cada interrupção faz com que seja realizada alguma ação, sendo que elas estão armazenadas em um vetor de 
      interrupções. Essas rotinas e o vetor são carregados assim que acontece o boot do SO.
* Memória
    - Proteção e troca de contexto: memória virtual, código realocável
    - A memória virtual é uma abstração: ela é, de fato, separada usando partes. Porém, existe um hardware específico que permite mudar a memória.
    - O hardware tem um registro das regiões que cada programa pode acessar. Se ele sair da região, é dado um sinal específico que "mata" o programa (falha de segmentação)
* Discos
    - Prato, superfície, cilindros, trilhas, setores, cabeças.
    - A taxa de transferência para o disco é muito lento, porque  a rotação do disco demora em taxa de milissegundos

    ```
    
                                        .------ cabeça para
            .----------------------.    '------ gravação
           / .--- trilha ---------. \    --.
          / /                      \ \     | cilindro
         / '------------------------' \  --'
        '------------------------------' --.
        '------------------------------'   | pratos
        '------------------------------'   | 
        '------------------------------' --'
        Setor: mínimo de bytes que podem ser escritos na memória 
    ```
    

### Outros dispositivos de I/O:

*  Discos, vídeo, teclado, mouse, interfaces de rede, etc.
* CPU não se comunica com dispositos (em geral, analódigos), mas com o circuíto didital de controle (controlador)
* Controladores são conectados ao barramento, via alguns registradores mapenados na CPU
* Cada controlador está associado a um "device driver" (interfaces padrão para acesso aos sistemas, convertidos via programas específicos em assembly)
* Quando acabou? "bus waiting" ou interrupção.

### Processos

* Um programa sendo executado
* Linha de processamento, registradores (pilha, etc.) e estado da memória.
* 5 estados possíveis: rodando, pronto, bloqueado, suspenso (bloqueado e pronto)

    ```
    
                         .-------.
                         |Running| ---> Terminate
                         '-------'
                preempt / ^       \ block
                       ^ / Run     ^ 
                  .-----.        .-------.
       Create --->|Ready|<-------|blocked|
                  '-----'        '-------'
                    | ^    unblock  | ^
                    | |             | |
            Suspend | | Resume      | | Resume
                    | |      Suspend| |
                    ^ |             ^ |
              .---------.          .---------.
              |  Ready  | <------- | Blocked |
              |Suspended|  unblock |Suspended|
              '---------'          '---------' 
    ```
    
* Estados:
    - **Blocked**: Esperando pelo resultado de uma chamada ao sistema
    - **Ready**:   Pronto para rodar, mas não necessariamente rodando (outro programa de maior prioridade pode estar ocupando a CPU. Nesse estado, ele só precisa ocupar a CPU)
    - **Blocked Suspended**: Se muitos programas estão rodando ao mesmo tempo, um programa pode ser suspenso, e ficar aguardando até que o sistema se desocupe. Nesse estado, ele ainda está aguardando por alguma chamada ao sistema.
    - **Ready Suspended**: Quando o programa recebe sua requisição ao SO, fica Ready, mas ainda suspenso.
    - **Running**: O programa ocupando a CPU.
    
* Multiprogramação
    - CPU deve ser otimizada com relação à execução (geralmente, programas com muito IO tem prioridade, porque eles usam a CPU e ficam bloqueados enquanto esperam as (demoradas) ações de IO).
    - O sistema mostrado acima é o pseudo-paralelismo da CPU.
    - **Threads (processos leves):** linha de processamento distinta, no contexto de registradores, com mesmo estado de memória. Usar threads é diferente de criar processos filhos, porque neste caso cada um tem seu espaço de memória, e naquele, a memória é compartilhada dentro do processo.
    - **Tabela de processos:** guarda quais processos estão rodando, e a ordem em que são trocados.
    - Processos não podem assumir nada em relação a tempo real  estão ou não executando).

### Minix

* Anos 80: A AT&T fecha o código do sistema Unix (S7) e proíbe seu ensino em universidades (para lucrar)
* Dificuldade de construção de SO leva muitos cursos a se tornarem teóricas ou a propor apenas pequenas simulações.
* Tanembaum se propõe a construir Minix:
* Compatível com a versão 7 do Unix (mesmas chamadas de sistema e escrito para ser lido, com fácil modificação):
    - Código C
    - Extremamente modularizado
    - Forte uso de comentários (original com 11.000 linhas de código e 3.000 linhas de comentários)
        - Fácil modificação
        - Fontes disponíveis
* SO "de verdade"

#### Processos (Minix)

* Processos podem se comunicar
* Organização hierárquica (Minix/Unix/Linux)
    - UID (User ID)
    - Obs: instrução 'fork' cria novo processo (clonado a partir do processo-pai, com registradores, processos, pilha, etc. todos iguais). O retorno do UID é 0 para o filho e o UID do filho para o pai.
    - Os filhos só podem saber quem são seus pais usando de comunicação:
        - Mensagens: Entre processos
        - Arquivos: Escrevendo entre um e outro
        - Pipes (|): Muito mais rápido que usar os arquivos, porque cria-se um buffer na memória, em vez de um arquivo no disco.
        - Sinais (signal): espécie de sinal assíncrono que faz que o receptor tenha código desviado para endereço pré-determinado. Os sinais fazem com que o outro programa tenha alguma ação parecida com uma interrupção. O destinatário pode ou não tratá-lo. Se tratar, a ação é parecida com a de interrupção. Porém, em vez de ir para um local pré-determinado no hardware, pode ir para outro.

#### Arquivos Minix

* Organização hierárquica
    - Diretório root (começando com /)
    - Caminhos absolutos e relativos (o diretório atual é guardado na tabela de processos)
* Proteção 8 bits (rwx)
* Arquivo especial:
    - Acesso transparente ao I/O
    - Bloco (disco): lê no apontador, e permite acesso aleatório. Ele pode ser alterado usando "seek" (para mudar o local) ou ler (com "read"). 
    - Caracter (teclado, tela, etc.): só podem ser usados para ler ou escrever diretamente (read/write), pois não é possível "voltar" para o passado.
* Todos os arquivos tÊm um DESCRITOR DE ARQUIVO.
* Standard input/output: descritores de arquivo 0 e 1 (geralmente, são o teclado e a tela)
* Montar/desmontar (mount/umount): utilizado para acesso a periféricos de maneira modular (criando os diretórios de dentro desse arquivo especial em algum subdiretório). A pasta padrão é /dev (device)
    - Major device number (tipo)/minor device number /dev/hda0, /dev/hda1
        - ^^^'- minor device number
        - '--- major device number

#### Estrutura do Minix
  
```  
  .--------------------------------------------------------------.
 4| User process | User process | User process |      ...        | }User
  |--------------'--.-----------'-.------------'.----------------| }
 3| Process manager | File system | Info server | Network server | }space
  |-------------.---'--------.----'------------.'----------------| }
 2| Disk driver | TTY driver | Ethernet driver |       ...       | }
  |-------------'------------'.----------------'.----------------|
 1|           Kernel          |   System task   |   Clock task   | Kernel
  '--------------------------------------------------------------' space
```

* Cada layer se comunica somente com uma camada abaixo dele. O espaço do kernel tem o gerenciado básico do sistema e o relógio, usado para os sistemas básicos (camada do Kernel).
* A segunda camada (drivers) são os programas que servem de interface para os componentes eletrônicos.
* A terceira camada (servidores) são os que se comunicam com os programas, e são todos são os que podem se comunicar com os que estão abaixo.
* A quarta camada são os processos normais do usuário, se comunicando com servidores de cada camada.
    
* 4 camadas: kernel (escalonamento de processos, tempo, tabelas, sistema de mensagens), drivers (abstração dos dispositivos), estratégias de escalonamento, sistema de arquivo e comunicação (apresentam "máquina virtual"), processos de usuário.
* Apenas 1 camada em "espaço de kernel".
* Outras camadas possuem processos sem compartilhamento de memória: comunicação por mensagens. As mensagens são implementadas usando uma chamada de sistemas específica. Essas mensagens têm uma estrutura específica, em que é guardada um ponteiro para o texto e o processo que enviou. A pessoa que enviou é bloqueado enquando ocorre a mensagem. O recebedor terá a mensagem COPIADA dentro do outro  processo, para ser lida. Quem implementa esse envio é o SO, fazendo a parte do baixo nível. O processo que ENVIA manda uma mensagem com a chamada 'send'. O processo que recebe fica com um 'receive', aguardando até receber algo. O 'send-receive' faz uma chamada e espera por ela. Esse processo é síncrono.
* A mensagem é um processo síncrono, e é bem diferentes de sinais. Os sinais são processos ASSÍNCRONOS, em que o programa é interrompido no meio de qualquer ação.
* O envio de mensagens é feito nas camadas inferiores, ABAIXO de servidores. Para as aplicações, existe a biblioteca de programas de chamadas ao sistema, que fazem por si próprias mensagens para os componentes corretos.
* Sinais são tratados pelo Kernel, ao receber alguma transmissão, via o barramento, de um componente do hardware. Ele gera uma mensagem e envia para o Kernel correspondente.
    - Essa arquitetura é a arquitetura de microkernel.

#### Chamadas de sistema no Minix

* Para os programas, se comunicar com o SO são funções em C armazenadas em uma biblioteca.
* Cada chamada de sistema manda um sinal que gera uma interrupção, e o SO, ao vê-lo, desempacota a rotina correspondente. O programa do usuário manda a requisição aos servidores, e estes comunicam-se com o driver.
* Chamadas de sistema do Minix
    - fork (única maneira de criar novos processos)
        - Gera duas cópias idênticas nas memórias separadas
        - Chamada volta zero para processo filho, e pid para o processo pai
    - wait()
        - Processo pai aguarda o final de execução de um dos filhos
    - execve(name, argv, envp)
        - A imagem do processo é substituída pelo conteúdo de arquivo, passados argumentos e ambientes (que descreve o terminal, etc).
        - Essa é a maneira de um programa "mutar", e deixar de ser igual ao processo pai. A imagem vem de algum arquivo executável, que deve ser passado como argumento.   
    - exit()
        - Termina o processo e retorna o parâmetro como código de saída (o zero representa OK). O código único (0) é bom porque todos os outros podem ser usados como erro.
    - brk()
        - Altera o tamanho da área de dados do parâmetro
    - getpid()
        - Pede o ID do processo (para poder comunicar via fork).
    - signal(sig,func)
        - Determina captura de um sinal pela função func (em geral, se não der para tratar, o processo é morto).
    - kill(pid,signal)
        - Manda um sinal a um processo. Existe um sinal KILL que realmente mata um processo - mas, de forma geral, ele só funciona para processos filhos.
    - alarm(seconds)
        - Agenda um envio do sinal SIGALARM, para que o processo seja avaliado depois de certo tempo. Usando signal, é possível dar tratamentos depois.
    - pause()
        - Suspende p processo ae queeste rebeba o próximo sinal.
    - creat(name,mode)
        - Cria um novo arquivo e acre, ou abre e seta o tamanho para zero, retornando o descritor (como 'touch')
    - mknod(nome,mode,addr)
        - Cria um i-node regular, especial ou de diretório, retorna um descritor. "Addr" é major + minor device number.
    - open(file,how)
        - Abre um arquivo considerando o nome dele e se terá leitura/escrita. Devolve um descritor de arquivo.
    - close(fd)
        - Fecha um arquivo aberto.
        - É importante para que os i-nodes sejam salvos, o que não ocorre se morrer.
    - read(fd,buffer,nbytes)
        - Lê dados em um buffer, retorna bytes lidos.
        - Lê até nbytes, ou menos. Retorna o número de bytes realmente gravados.
    - write(fd,buffer,nbytes)
        - Grava dados de um buffer e retorna o número de bytes gravados (porque poderia dar problemas).
    - lseek(fd,offset,whence)
        - Muda o poteiro do arquivo, retona a nova posição para posição. Whence indica se o offset relativo à posição atual, início ou fim.
    - stat(name,&buf),fstat(fd,&buf)
        - Lê e retorna o estado do arquivo
    - dup(fd1)
        - Aloca um novo descritor de arquivos (número que descreve qual é o i-node que representa o arquivo) para o arquivo aberto. Dup cria um novo número para o mesmo i-node.
        - Dup costuma ser útil para SUBSTITUIR o arquivo apontado por aquele descritor específico, e depois retornar. É útil principalmente para redirecionar stdin e stdout.
        - Ex:
            ```
            fd = dup(1);
            close(1);   // LIBERA o descritor '1'
            open(name); // Pega o MENOR descritor o possível: no caso, será o 1
            ...
            close(1);
            dup(fd);    // Agora, dup pegará o menor fd disponível. Ele será, novamente, 1
            ```
    - pipe(&fd[0])
        - Cria um pipe, retorna fd de leitura e escrita no vetor.
        - Ex:
            ```
            Pipeline(process1, process2)
            char *process1, *process2; // nomes dos programas
            {
                int fd[2];
                pipe(&fd[0]);
                if(fork() != 0) {
                    /* Processo pai, escreve no filho */
                    close(fd[0]);  // este processo não lerá do pipe. Pode fechá-lo
                    close(stdout); // Fechamos stdout para para liberar o fd
                    dup(fd[1]);    // Pega a saída do pipe e a associa ao menor fd disponível: stdout.
                    close(fd[1]);  // Não precisa mais do pipe, e libera esse descritor.
                    execl(process1,process1,0); // Roda o processo e os outros
                }
                else {
                    /* Processo filho, lê do pipe */
                    close(fd[1]);  // este processo não escreverá no pipe. Pode fechá-lo
                    close(stdin);  // Fechamos stdin para liberar o fd
                    dup(fd[0])     // Pega a saída do pipe a a associa ao menor fd disponível: stdin
                    close(fd[0]);  // Não precisa mais do pipe, e libera esse descritor.
                    execl(process2,process2, Roda o processo e os outros
                }
            } 
        ```
    - ioctl(fd,request,argp)
        - Executa operações em arquivos especiais (em geral terminais)
    - link(name1,name2)
        - Faz entrada name2 ser o mesmo i-node que name1
    - unlink(name)
        - Remove uma entrada do diretório corrente
    - mount(special,name,rwflag)
        - Monta um sistema de arquivos
    - unmount(special)
        - Desmonta um sistema de arquivos
    - sync()
        - Sincroniza todos os blocos de disco do cache
    - chdir(name)
        - Muda o diretório corrente
        - O cd, do Bash, usa esse programa
    - chroot(dirname)
        - Muda o diretório raiz (cuidado)
        - Funciona APENAS para a shell correspondente
        - Ela é perigosa porque os comandos dentro daquele contexto usarão comandos em outros lugares - e, nesse caso, pode-se dar problemas. Por esse motivo, chroot só funciona como superusuário.
    - chmod(name,mode)
        - Muda os bits de proteção do arquivo
    - uid = getuid()
        - Retorna o UID do usuário
    - gid = getgid()
        - Retorna o GID do usuário.
    - setuid()
        - Muda o UID do usuário.
    - setgid()
        - Muda o GID do usuário.
    - time(&seconds)
        - Retorna o tempo em segundos desde 1/1/1970.
    - stime(tp)
        - Reinicializa o tempo desde 1/1/1970.
    - utime(file,timep)
        - Seta o valor do último acesso do arquivo.
    - times(bufer)
        - Retorna o tempo do usuário e de sistema.

#### Estrutura do programa

```
    --------- .----------.
    |     | Símbolos | Símbolos de #define do programa
    |     |----------| 
    |     | Data     | Dados de entrada do programa
    |     |----------| 
    |     | Text     | Programa de fato
    |     |----------| 
    |     | Header   | Área do programa com informações dos campos acima
   --------- '----------'
                                
                     .---------------.
    Se, ao dar um    :     stack     : Empilhamento
    "push" na pilha  :       |       : (aumentado via push)
    e ele invadir    :       ^       : 
    o brk, haverá    :               : 
    um sinal de erro :       ^       : 
    um erro.         :       |       : Memória dinâmica
                     :     heap      : (aumenta/diminui via brk)
    O malloc tenta   :---------------: 
    ver se áreas da  :               : área para dados
    heap liberadas   :     bss       : estáticos não
    podem ser usadas :               : inicializados
    para novas       :---------------: 
    alocações. Caso  :     data      : Dados pré-definidos
    não possa, tenta :---------------: 
    brk. Se não der, :     text      : Programa em si
    retorna que não  '---------------' 
    há mais espaço
    para alocação
```
            
Quando há acesso FORA do frame, o sistema operacional MATA o programa. 
Os sinais são, para o software, o mesmo que uma interrupção para o hardware.
                
```
                      .---------------.
 .----.           .-- |     Pilha     |
 | PC |   empilha |   |---------------|
 '----'    o PC   '-> |      PC       |
                      |---------------|
          Pula para   |               |
          o sinal...  |               |
                      '---------------'
```

-------------------------------------------------

## Tabela de Processos

* A partir desse momento, consideraremos o modelo de SO single core.
* Armazena dados sobre cada processo necessários à sua execução.
* Essencial para troca de contexto.
    - Na tabela de processos, estão todos os dados relacionados ao processo e que são necessários para executá-los. Uma das informações essenciais é a dos registradores.
    - De forma geral, a memória da tabela de processos fica no Kernel. Porém, no Minix, como há um microkernel, mesmo a tabela de processos está separada pelo SO.
* Informações sempre devem estar presentes.
* No Minix, existem cópias parciais da tabela nos processos servidores.
    - Atualizações são feitas por mensagens, de modo a manter as tabelas de vários locais sincronizadas.
    - Para haver a comunicação, é necessário que o Kernel pareça uma mensagem. O "System task" é uma parte do Kernel que age como um processo, podendo receber os sinais.

### Partes da tabela de processos

* Kernel
    - Registradores
    - Program counter (PC)
    - Program Status Word (PSW): Registrador do hardware usado para proteção e permissões.
    - Stack pointer (registradores da pilha)
    - Process state (rodando, pronto, bloqueado e suspenso)
    - Prioridade atual do processo
    - Prioridade máxima do processo
    - Tamanho do quantum (máximo de ticks possíveis): Gerencia o máximo de tempo (contado por interrupções do relógio) que o processo irá rodar. Isso limita o tempo de uso da CPU.
    - Total de ticks restantes para o processo:  Contador para verificar quanto tempo falta para acabar.
    - Tempo de CPU já utilizado
    - Ponteiro para fila de mensagens
    - Vetor de sinais ("interrupção do software")
    - Bits para Flags
    - Nome do processo
* Gerenciamento de processos
    - Ponteiro para o segmento de texto (código binário) do processo
    - Ponteiro para o segmento de dados (dados pré-definidos) do processo.
    - Ponteiro para o segmento de bss (dados estáticos não inicializados) do processo.
    - Status de saída
    - Status de sinal
    - ID do processo
    - Processo pai
    - Processo de grupo
    - Tempo de CPU para o filho
    - UID Real
    - UID Efetivo: É mudado para poder alterar arquivos como o de senhas (que pertence ao superusuário) para mudá-lo.
    - GID Real
    - GID Efetivo
    - Informações de arquivos para compartilhar texto
    - Bitmap para sinais
    - Bits para flags
    - Nome do processo
* Gerenciamento de arquivos
    - Máscara UMASK
    - Diretório raiz
    - Diretório de trabalho
    - Descritor de arquivos
    - UID Real
    - UID Efetivo
    - GID Real
    - GID Efetivo
    - TTY controladora (terminal)
    - Área para leitura/ecrita
    - Parâmetros de chamadas de sistemas
    - Bits para flags

### Processos: esquema de interrupção e hardware

* Associado acada classe de dispositivo de entrada/saída, existe uma entrada em um vetor chamado VETOR DE INTERRUPÇÃO
    - É um dos primeiros a serem carregados ao inicializar o SO.
    - Torinas de baixíssmo nível.
    - Contém endereços do procedimento de serviço da interrupção.
* Quando ocorre uma interrupção, o hardware da máquina coloca algumas informações na pilha
    - Apontador de instroções (PS)
    - PSW
    - Talvez mais alguns registradores
* CPU então vai para a entrada relativa do vetor de interrupção.
* A partir daí, tudo vai por software:
    - Código inicial em assembly
        - Procedimento de serviço de interrupção armazena os registradores na tabela de processos (assembler)
            - Número do processo atual e apontador na tabela de processos mantido em variáveis globais para acesso rápido.
                - Os dados são salvos no estado do usuário.
        - Informação depositada pelo hardware é removida da pilha e o apontador da pilha é redirecionaro para a pilha do administrador de processos (o tratador da rotina).
        - Rotina em C é chamada para realizar o resto do trabalho.
    
    - Código em C
        - Construção de uma mensagempara ser enviada ao processo associado ao driver (que deve estar bloqueado esperando). 
            - Mensagem indica interrupção para distinguí-la de mensagens do servidor de arquivos.
            - Estado do driver alterado para pronto.
            - Escalonador de processo é chamado
                - Como drivers são os processos de maior prioridade, o driver específico deveser chamado.
                - O driver só espera se o processo interrompido tiver a mesma prioridade: outro driver
            - O procedimento em C chamado pelo tratador de interrupção retorna.
            - Chama-se um código em assembly para realizar a resposta relativa ao driver.

### Inicialização do SO

* Inicializa todos os drivers e servidores, que chamam a rotina "receive" esperando por mensagens.
* Começa várias shells (tty's) que esperam por um usuário logar.
* Quando o usuário loga, abre-se uma shell para o usuário.
* Um programa quase sem prioridade fica ocupando a CPU para que o sistema rode sem parar, até que outros processos ajam.

----------------------------------------

## Exclusão mútua

### Condições de concorrência

* Processos rotineiramente precisam se comunicar
* Em muitos SOs, quando processos trabalham juntos, eles compartilham algum armazenamento comum onde podem ler e escrever.
* Quando processos funcionam de maneira indepedente, pode ocorrer um "data race", em que um processo atualiza uma região que estava sendo usada pelo outro, gerando inconsistências.
    
* Ex: duas threads atualizam a variavel global
    - Situação ideal:
    ```
        int i ← 0
        (T1) registrador1 ← i                 ;; i = 0
        (T1) registrador1 ← registrador1 + 1  ;; i = 0
        (T1) i ← registrador1                 ;; i = 1
        (T2) registrador2 ← i                 ;; i = 1
        (T2) registrador2 ← registrador2 + 1  ;; i = 1
        (T2) i ← registrador2                 ;; i = 2
    ```

    - Exclusão mútua:
    ```
    int i ← 0
    (T1) registrador1 ← i                 ;; i = 0
    (T2) registrador2 ← i                 ;; i = 0
    (T2) registrador2 ← registrador2 + 1  ;; i = 0
    (T2) i ← registrador2                 ;; i = 1
    (T1) registrador1 ← registrador1 + 1  ;; i = 0
    (T1) i ← registrador1                 ;; i = 1
    ```
    
* Ex.: spooling -  Atualização de uma área para impressão
    - Resolução:
    - REGIÃO CRÍTICA: regiões dos processos que podem gerar condições de concorrência são chamadas regiões críticas.
    - 4 CONDIÇÕES PARA UMA BOA SOLUÇÃO:
        - Só um processo deve entrar na região crítica de cada vez.
        - Não deve ser feita nenhuma hipótese sobre a velocidade relativa dos processo.
        - Nenhum processo executando fora de sua região crítica deve bloquear outro processo.
        - Nenhum processo deve esperar um tempo arbitrariamente longo para entrar na sua região crítica (adiamento indefinido).
        
    - Solução 1: inibir interrupções
        - Hardware possui instrução específica para inibir interrupções. 
        - SO usa quando código do kernel está processando (ele precisa para que o próprio tratamento de interrupções não seja afetado).
        - Solução ruim para outros processos
            - Loops infinitos
            - Operações inválidas
            - Etc.
        - Versões antigas do Windows usavam essa técnica - e acabavam travando o computador.
        - Funciona, geralmente, quando há apenas um processador.
        
    - Solução 2: usar código de software
        - Tentativa 1: usar variável global
            ```
            : int A = 0; /* variável global */                :
            :                                                 :
            : // Processo 1          : Processo 2             :
            : while(A == 1) {};      : while(A == 1) {};      :
            :                        :                        :
            : A = 1;                 : A = 1;                 :
            : ... // região crítica  : ... // região crítica  :
            : A = 0;                 : A = 0;                 :
            ```  
            - Não há garantias de exclusão mútua (que ambos não entrarão na região crítica - o que pode ocorrer porque o teste não tem sincronização boa).
        
        - Tentativa 2: revezamento
            ```
            : int vez = 1; /* variável global */                :
            :                                                   :
            : // Processo 1           : Processo 2              :
            : while(1) {              : while(1) {              :
            :                         :                         :
            :   /* Espera */          :   /* Espera */          :
            :   while(vez == 1) {};   :   while(vez == 2) {};   :
            :                         :                         :
            :   ... // região crítica :   ... // região crítica :
            :   vez = 2;              :   vez = 1;              :
            ```
            - Não há garantias de que o outro processo deixará o primeiro trabalhar. Pior ainda, se o outro programa entrar em loop infinito, o primeiro processo ficará congelado.

        - Tentativa 3: revezamento mais complexo
            ```
            : int p1dentro = 0;                                 :
            : int p2dentro = 0;                                 :
            :                                                   :
            : // Processo 1           : Processo 2              :
            : while(p2dentro) {};     : while(p1dentro) {};     :
            :                         :                         :
            : p1dentro = 1;           : p2dentro = 1;           :
            : ... // região crítica   : ... // região crítica   :
            : p1dentro = 0;           : p2dentro = 0;           :
            :                         :                         :
            : ... // resto            : ... // resto            :
            ```
            - Tem a chance de que ambos entrem ao mesmo tempo.
            
        - Tentativa 4: gentileza
            ```
            : int p1querEntrar = 0;                             :
            : int p2querEntrar = 0;                             :
            :                                                   :
            : // Processo 1           : // Processo 2           :
            : p1querEntrar = 1;       : p2querEntrar = 1;       :
            :                         :                         :
            : while(p2querEntrar) {}  : while(p1querEntrar) {}  :
            :                         :                         :
            : ... // região crítica   : ... // região crítica   :
            :                         :                         :
            : p1querEntrar = 0;       : p2querEntrar = 0;       :
            :                         :                         :
            : ... // resto            : ... // resto            :
            ``` 
            - Um dos processos pode acabar esperando um tempo arbitrariamente longo, caso haja problemas de sincronia.
            - Outra possibilidade é que ambos tentem entrar, e nenhum deles entre.
            
        - Tentativa 5: gentileza mais complexa
            ```
            : int p1querEntrar = 0;                                 :
            : int p2querEntrar = 0;                                 :
            :                                                       :
            : // Processo 1             : // Processo 2             :
            : p1querEntrar = 1;         : p2querEntrar = 1;         :
            :                           :                           :
            : while(p2querEntrar)       : while(p1querEntrar)       :
            : {                         : {                         :
            :   if(favorito == 2)       :   if(favorito == 1)       :
            :   {                       :   {                       :
            :     p1querEntrar = 0;     :     p1querEntrar = 0;     :
            :     while(favorito == 2)  :     while(favorito == 1)  :
            :     { /* ... */ }         :     { /* ... */ }         :
            :     p1querEntrar = 1;     :     p1querEntrar = 1;     :
            :   }                       :   }                       :
            : }                         : }                         :
            :                           :                           :
            : ... // região crítica     : ... // região crítica     :
            :                           :                           :
            : p1querEntrar = 0;         : p2querEntrar = 0;         :
            :                           :                           :
            : ... // resto              : ... // resto              :
            :                           :                           :
            ```
            - Apesar de ser quase impossível termos o problema, ainda é possível que ambos entrem na região crítica.
        
        - Tentativa 6: Algoritmo de Dekker
            ```
            : int p1querEntrar = FALSE;                                 :
            : int p2querEntrar = FALSE;                                 :
            : int favorito = 1; // 1º processo é o favorito             :
            :                                                           :
            :                                                           :
            : while(TRUE)                 : while(TRUE)                 :
            : {                           : {                           :
            :   // Processo 1             :   // Processo 2             :
            :   p1querEntrar = TRUE;      :   p2querEntrar = TRUE;      :
            :                             :                             :
            :   while(p2querEntrar)       :   while(p1querEntrar)       :
            :   {                         :   {                         :
            :     if(favorito == 2)       :     if(favorito == 1)       :
            :     {                       :     {                       :
            :       p1querEntrar = FALSE; :       p2querEntrar = FALSE; :
            :       while(favorito == 2); :       while(favorito == 1); :
            :       p1querEntrar = TRUE;  :       p2querEntrar = TRUE;  :
            :     }                       :     }                       :
            :   }                         :   }                         :
            :                             :                             :
            :   ... // região crítica     :   ... // região crítica     :
            :                             :                             :
            :   p1querEntrar = 0;         :   p2querEntrar = 0;         :
            :                             :                             :
            :   ... // resto              :   ... // resto              :
            : }                           : }                           :
            ```
            - O algoritmo de Dekker usa a ideia de FAVORITISMO para definir quando entraremos ou não em um dado processo.
            - Esse algoritmo impede que os dois processos entrem ao mesmo tempo na região crítica. O 'favorito' serve como uma camada extra que demore tempo demais para liberar o outro.
            
        - Tentativa 7: Algoritmo de Peterson
            ```
            : int p1querEntrar = FALSE;                                 :
            : int p2querEntrar = FALSE;                                 :
            : int favorito = 1; // 1º processo é o favorito             :
            :                                                           :
            : while(TRUE)                 : while(TRUE)                 :
            : {                           : {                           :
            :   // Processo 1             :   // Processo 2             :
            :   p1querEntrar = TRUE;      :   p2querEntrar = TRUE;      :
            :                             :                             :
            :   favorito = 2;             :   favorito = 1;             :
            :                             :                             :
            :   while(p2querEntrar        :   while(p1querEntrar        :
            :      && favorito == 2);     :      && favorito == 1);     :
            :                             :                             :
            :   ... // região crítica     :   ... // região crítica     :
            :                             :                             :
            :   p1querEntrar = TRUE;      :   p2querEntrar = TRUE;      :
            :                             :                             :
            :   ... // resto              :   ... // resto              :
            : }                           : }                           :
            ```
            - Essa solução é um pouco mais concisa, que faz o mesmo que o algoritmo de Dekker.
        
    - Solução 3: Exclusão mútua por Hardware (instrução Test_and_set)
        - Instrução especial do hardware, atômica
        - Test_and_set(a,b) → { a = b; b = TRUE; }
        ```
        :                                                           :
        : while(TRUE)                 : while(TRUE)                 :
        : {                           : {                           :
        :   int p1_nao_pode_entrar    :   int p2_nao_pode_entrar    :
        :       = TRUE;               :     = TRUE;                 :
        :                             :                             :
        :   while(p1_nao_pode_entrar) :   while(p2_nao_pode_entrar) :
        :   {                         :   {                         :
        :     test_and_set(           :      test_and_set(          :
        :       p1_nao_pode_entrar,   :        p2_nao_pode_entrar,  :
        :       ativo);               :        ativo);              :
        :   }                         :   }                         :
        :                             :                             :
        :   ... // região crítica     :   ... // região crítica     :
        :                             :                             :
        :   p1querEntrar = TRUE;      :   p2querEntrar = TRUE;      :
        :                             :                             :
        :   ... // resto              :   ... // resto              :
        : }                           : }                           :
        ```
        - A instrução test_and_set é atômica e evita o problema da tentativa de solução 1: lá, poderia haver uma interrupção da execução entre o teste e o assinalamento. Porém, esse tipo de uso depende de uma instrução do HARDWARE.
        - Esse comportamenteo pode ser simulado em software se transformarmos a função test_and_set como uma função que é uma região crítica - e usando o algoritmo de Dekker ou de Peterson.

    - Solução 4: Exclusão mútua por Semáforos (SO/compiladores)
        - Novo tipo de variável
        - 2 ações atômicas:
            - P(semáforo)/down(semáforo)
                - Se valor > 0 subtrai 1 e continua
                - Senão trava e espera mudar de fila
            - V(semáforo)/up(semáforo)
                - Se semáforo tem fila, libera o primeiro
                - Senão incrementa valor em 1         
        - Variação: semáforo binário
            - Valor nunca excede 1
        - Facilita implementação de exclusão mútua em vários
          processos.
        - Pode ser oferecido no SO ou pelo compilador (ex: Java)
            - Em compiladores é implementado usando outro esquema
              disponível de exclusão mútua.
        - Código para semáforo binário
            ```
            Semaforo mutex = 1;
            while(TRUE)
            {
                P(mutex);
                // região crítica
                V(mutex);
                // região não crítica
            }
            ```
        - Ex: problemas com progutores e consumidores.
            ```  
            Semaforo_binario mutex; /* exclusão mútua */
            Semaforo_contador vazio = TAMBUFFER; /* Controle buffer */
            Semaforo_contador cheio = 0;         /* Controle buffer */
            
            // PRODUTOR
            while(TRUE)
            {
                registro item_produzido;
                produz(&item_produzido);
                P(vazio); /* Vê se há algum slot vazio */
                P(mutex); /* Pega acesso ao buffer de itens */
                coloca_item(item_produzido);
                V(mutex); /* Deixa acesso do buffer de itens */
                V(cheio); /* Aumenta o nº de recursos cheios */
            }
            
            // CONSUMIDOR
            while(TRUE)
            {
                registro item_consumido;
                P(cheio); /* nº de recursos disponíveis */
                P(mutex); /* Pega acesso ao buffer de itens */
                pega_item(&item_consumido);
                V(mutex); /* Deixa acesso do buffer de itens */
                V(vazio); /* Aumenta a quantidade de slots vazios */
            }
            
            
                                                    MUTEX VAZIO CHEIO
                                   ||                 0     3     0
             0 1 2                 || Fila de  PRO1   1     2     1
            | | | |                || Execução PRO1   1     1     2
             ^                     ||          PRO2   1     0     3
             '- ini = 0; prox = 0; ||          PRO2   1   interrupção
                                   ||          CON1   1     1     2
                                   ||               PRO1^   0     3
            VAZIO  CHEIO  MUTEX    ||          PRO1   1   interrupção
                                   ||          CON2   1     1     2
                                   ||               PRO1^   0     3
                                   ||          CON1   1     1     2
                                   ||          CON1   1     2     1
                                   ||          CON2   1     3     0
                                   \/          PRO2   1     2     1
        ```
    - Solução 5: Monitores (compilador)
        - Os semáforos são uma forma interessante de fazer o controle de acesso para a exclusão mútua. Porém, ele ainda depende de que os programadores usem os semáforos corretamente.
        - Dentro da lógica de POO, o monitor funciona como um objeto.
        - Implementado pelo compilador.
        - Somente 1 processo "entra" no monitor de cada vez
            - Controle de entrada por técnicas de exclusão mútua
        - Processo pode emitir um WAIT
            - "sai" do monitor e entra em uma fila associada à variável da operação WAIT.
            - Reativado pela operação SIGNAL.
        - Processo pode emitir SIGNALlogo antes de sair do monitor
            - Quem estiver esperando na fila entra em seguida (se houver).
        
        - Ex: estilo C, originalmente em ADA
            ```
            Monitor ProdutorConsumidor{
              condition full, empty;
              int count;
              procedure coloca_item(item) {
                if(count == TAMBUFFER) { WAIT(full) } 
                /* Espera buffer ter espaço */
                
                entra_item(item); /* Altera buffer comum */
                count++;
                if(count == 1) { SIGNAL(empty) } 
                /* avisa que tem dado */
              }
              
              procedure pega_item(&item) {
                if(count == 0) { WAIT(empty); }
                /* Espera buffer ter espaço */
                
                tira_item(item); /* Altera buffer comum */
                count--;
                if(count == TAMMBUFFER-1) { SIGNAL(full); }
                /* Avisa que buffer não está mais cheio */
              }
            }
            
            /* Produtor */
            while(TRUE) {
              registro item_produzido;
              produz(&item_produzido);
              coloca_item(item_produzido);
            }
            
            /* Consumidor */
            while(TRUE) {
              registro item_consumido;
              pega_item(&item_consumido);
              consome(item_consumido);
            }
            ```
            - Cada uma das variáveis de acesso pode ser implementada usando um semáforo.
            - É necessário termos um MUTEX (semáforo binário) para que entremos no monitor.
            - O SIGNAL funciona igual a um V(), porque ele libera alguém de uma fila para entrar.
            - O WAIT, porém, é mais complexo. Precisamos, primeiro, dar um V() para liberar o MUTEX para outro processo entrar. Depois, damos dois P's: um para conseguir entrar na fila de aguardo do recurso e outro para acessar o mutex (depois).


-----------------------------

## Comunicação entre processos

* Semáforos, monitores: para memória compartilhada
* Envio de mensagens
    - send(destino, mensagem);
    - receive(fonte, &mensagem); -- fonte pode ser Pid ou ANY
* Questões
    - Comunicação síncrona ou assíncrona?
        - Síncrona: como uma chamada de telefone. Quando um envia a mensagem, o outro já recebe.
        - Assíncrona: como uma carta. Quando um envia a mensagem, o outro pode receber depois de algum tempo.
    - Confirmação
        - Mensagem de confirmação para certificação de entrega.
        - Retransmissão se certificação for muito demorada.
        - Mensagens fuplicadas devem ser ignoradas (contar mensagens). Isso é necessário porque, se uma mensagem demorar muito, ela pode ser reenviada. Porém, no final, ambas podem chegar.
    - Endereçamento
        - Serve para identificar se há comunicação entre máquinas.
        - processo@maquina, processo@maquina.dominio
    - Autenticação: criptografia
    - Eficiência quando na mesma máquina.
    - Mailbox (pode compartilhar) x bloqueio total (MINIX)
        - Mailbox: continua agindo e só bloqueia quando receber uma mensagem de volta. Porém, para isso, é necessário haver um pool que guarde as mensagens recebidas. Isso é ruim para um SO, pois gasta memória do sistema, e seria sucetível a um ataque DDoS (se fossem geradas várias mensagens constantemente).
        - Minix: quando é enviada uma mensagem, o programa é bloqueado até que ela seja recebida. Isso evita o problema de armazená-las. A tabela de processo precisa, apenas, armazenar um campo que diga que ele está bloqueado enviando uma mensagem.

* Chamada de procedimento remota
    - Para cliente, envio de mensagem parece chamada de procedimento
    - Sempre síncrono.
    ```
    Client CPU
                
    Client
    .----.------.
    |    | Stub |
    '----'------'
    ```
    - Problemas:
        - Passagem de parâmetros por referência não é possível, pois a memória entre cliente e servidor pode não ser compartilhada.
        - Representação diferente de informações:
            - Pode variar dependendo do hardware (esses problemas diminuíram recentemente)
            - Ponto flutuante, inteiros, ASCII.
        - Comportamento em caso de falha:
            - Alternativas: tentar de novo? Matar processo? (quando?)
            - At least once, at most once, maybe.

-----------------------------------------------
## Escalonamento de processos

* Quando se tem vários processos é necessária uma política para escolher o próximo processo a ter o controle da CPU.
* Ganho muita força com a criação dos primeiros sistemas de janelas (Macintosh e Windows)

* Objetivos
    - Justiça: todos rodam.
    - Eficiência: CPU utilizada 100% do tempo pelos processos.
    - Tempo de resposta: usuários interativos não devem notar latência.
    - Fluxo: maximizar o número de processos que completam (BATCH).
    - Minimizar tmpo para processos batch completarem.

* Alguns os objeticos são contraditórios:
    - Fluxo x eficiência
    - Fluxo x tempo de resposta

* Problema: processos imprevisíveis.
    - Não podemos saber quanto tempo algum deles rodará.

* Praticamente todos os SOs têm interrupção periódica por relógio.
    - Todas as vezes que o relógio dá um tick, ocorre uma interrupção. O processamento é parado, empilhando PSW e PC e saltando para o código que dará o controle para o escalonador de processos. Isso evita que um usuário tenha controle do sistema o tempo todo.
    - NOTA: MS Windows por muito tempo não utilizou este mecanismo (multiprocessamento cooperativo).
        - Ponto positivo: não precisa de hardware.
        - Ponto negativo: o compilador introduzia chamadas de sistema que fazia mensagens para chamar o escalonador.

* Alto nível
    - Job scheduling - quais tarefas concorrem pelos recursos do sistema (BATCH).
    - Serve para processamentos enormes, em que os BATCHes rodam durante um tempo, com o gerenciamento feito por um operador ou algum algorítmo maior do sistema.
* Nível intermediário
    - Quais processos podem concorrer pela CPU.
    - suspende | ativa processos para controlar carga do sistema.
    - Suspende, por exemplo, processos que já rodavam há muito tempo.
* Baixo nível
    - Qual dos processos que estão prontos deve ter o controle da CPU

* Preemptivo | Não preemptivo
    - CPU pode : não pode ser retirada de um processo
    - Preempção ocorre ao custo de um overhead
        - No mínimo o estado do processo deve ser salvo.
        - Uma nova rotina precisará ser rodada.
    - Preempção é necessária em sistemas onde tempo de resposta é importante (tempo real -- precisam ter tempo de resposta preditível | sistemas interativos multiusuários)
    - A interrupção também pode ser feita quando algum hardware age - como uma leitura pronta de HD ou memória, entrada no teclado, etc.

* Interrupção de relógio
    - Instrução de hardware determinam tempo até que a próxima interrupção seja gerada pelo relógio do sistema.

### Algoritmos de escalonamento

#### Data limite
* Tempo real
* Processo ao iniciar, determina quando sua tarefa precisa estar completa.
* Planejamento complexo: envolve cuidadosa estimação de uso dos recursosdo sistema (disco, CPU, etc.)
* Sobrecarga de planejamento (sistemas em tempo real consomem CPU apenas para serem sistemas em tempo real).

#### FIFO
* Mais básico.
* Razoável apenas em sistemas BATCH.

#### Shortest Job First (Batch)
* Estimativa de tempo por processo (cada processo dá a sua própria estimativa - se exceder, ou o processo é morto, ou o custo pode aumentar).
* Objetivo é aumentar fluxo de processos.
* Pode ser ruim porque processos pequenos podem ficar rodando durante muito tempo, e os grandes (e mais caros) são maiores.
    
#### Shortest Remaining Time First (Batch)
* Especialização do anterior, mas melhora a justiça. É a melhor heurística para batch.
* Um processo que falta apenas 1 min para ser concluído, mas durava 3 dias, não é interrompido para a execução de vários processos de uma hora.
    
#### Prioridade
* Número estavelece ordenação nos processos
| Estática               | Dinâmica                                                  |
| Não pode ter alteração | Pode mudar durante a execução do processo (enquanto roda) |
* A prioridade pode ter custos diferentes
* Highest response ratio (forma de implementação)
* Prioridade = (tempo espera + tempo serviço) / tempo serviço
    
#### Round-robin
* Fila circular
* Revezamento estrito
    ``` 
    Ready queue
          
    .> P9 -> P7 -> P3 -> P1 -|-> | P0 |
    |                        |     |
    |  ^---------------------'     |
    |          preempted           |
    |                              |
    | ready                        |
    |                              |
    P2 P4 P5 P6 P8    <-------------'
                     Blocked on I/O
    I/O Queue
    ```

#### Multi-level queues
* 1 fila por classe de prioridade
* Prioridade estática (fica sempre na mesma fila)
* Round-robin em cada fila
    ```       
        Maior prioridade
                ^
      real time | --> P -> P -> P -> P -|-> | P | (Running)
                |     ^-----------------'    
                |          preempted         
                |                            
         system | --> P -> P -> P -> P -|-> |   |
                |     ^-----------------'    
                |          preempted         
                |                            
    interactive | --> P -> P -> P -> P -|-> |   |
                |     ^-----------------'    
                |          preempted         
                |                            
          batch | --> P -> P -> P -> P -|-> |   |
                |     ^-----------------'    
                |          preempted          
                ready                       
    ```

#### Multi-level FEEDBACK queues
* Filas de prioridade flexíveis, com mudanças dependendo de tempo sem rodar e interrupções I/O
* Interrupção de tempo reduz prioridade
* Promoção: tempo sem rodar, interrupção I/O
* Prioridade dinâmica - de acordo com o comportamento do processo, a prioridade é mudada - se houver interrupção por I/O, o processo AUMENTA de prioridade (e diminui o quantum de uso do processaor). Se houver interrupção por relógio, o processo DIMINUI de prioridade (aumentando o quantum de uso do processador). No primeiro caso, o tempo de CPU costuma ser menor, e no segundo, maior. Essa é uma forma de heurística.
    ```    
    Maior prioridade
      ^                                            |
    0 | --> P -> P -> P -> P -|-> | P | (Running)  |
      | .---------------------'                    |
      | |      preempted                           |
      | |                                          |
    1 | '-> P -> P -> P -> P -|-> |   |            |
      | .---------------------'                    |
      | |      preempted                           |
      | |                                          |
    2 | '-> P -> P -> P -> P -|-> |   |            |
      |     ^-----------------'                    ^
      |        preempted                     Maior quantum
    ```

#### Escalonamento garantido
* Tenta dar acada USUÁRIO uma fatia igual detempo
* Tempo do usuárioreal: tempo desde login / n
* Escolhe processo de usuário com fatia real mais distante do real (ou seja, dá prioridade para o usuário que usou uma pequena fatia de tempo em comparação com outros usuários).
* Pode ser usado para GRUPO de usuários
    - VM da IBM usava para sistemas operacionais
* Vantagens
    - Justo com usuários/grupos de usuários
    - Favorece usuários com poucos processos
* Desvantagens
    - Injunsto com processos
    - Favorece usuários com poucos processos
    - Pode dar fatia a um usuário que não vai precisar
    - Overhead (mais tempo para guardar estatísticas de usuário + percorrer a tabela de usuários)
    
#### Sistemas multiprocessados
* Diferente de multiprogramação (que tinha múltiplos programas na mesma CPU).
* Agora, temos também múltiplas CPUs, múltiplos cores e processadores em hyper-threading (que extrai e decodifica instruções (do ciclo fetch-decode-execute) duas instruções ao mesmo tempo. A execução, porém, continua em um só). Atualmente, há computadores com múltiplas CPUs com vários cores com hiper-threading.
* Porém, os mesmos algoritmos e aplicam:
    - Assume-se SMP (simetrical multi processing)
    - Todos têm acesso à memória e aos mesmos recursos.
    - Cada processo vê como se houvesse apenas uma linha.
    - O Kernel tem mais interrupções (das várias CPUs/cores), mandando os processos para uma delas conforme o caso.
* Afinidade de processos
    - Hard affinity: sistema farante que processo roda no mesmo processador.
    - Soft affinity: sistema tenta na mesma CPU mas pode mudar processo para outra CPU.
    - Melhor que CPU sem nada a fazer
    - Sistema tenta balanceamento de cargas nas CPUs de maneira que cada um tenha processos suficientes.
    - Push-migration: SO verifica periodicamente carga em cada processador (número de processos na fila) - muda se houver desbalanceamento.
    - Pull-migration: escalonador verifica se sua fila. Se estiver vazia, procura na fila de outros processadores e "rouba" processos (cada um tem um escalonador).
    - O Linux combina pull e push. 

-----------------------------------------------
## Deadlocks

* Competiçãopor recursos (ex, dispositivos, arquivos, plotters, canais de comunicação, etc).
* Ocorre quando tempos regiões críticas - que não podem ser compartilhados e só um processo pode usá-lo por vez.
* Os deadlocks são um IMPASSE que ocorre nesse sistema - quando um processo P1 precida de dois recursos, R1 e R2, e o processo P2 precisa dos mesmos recursos. Se P1 pegar R1, e P2 pegar R2, eles entram num IMPASSE. Nem P1 terminará, liberando R1 para P2 terminar, nem P2 terminará, liberando R2 para P1 terminar.
* Ex:
```
          P1     } O uso de recursos pode ser modelado usando um
        ↗   ↘    } grafo de recursos, com vértices sendo recursos
      R1     R2  } e processos, e arcos ligando processo + recurso.
        ↖   ↙    } Se houver ciclos nesse grafo, então há deadlock,
          P2     } e ele precisa ser tratado.
```

* Existem 4 condições NECESSÁRIAS para que os impasses ocorram (Coffmann et al, 1971):
    - Exclusão mútua: cada recurso pode apenas ser designado a um processo.
    - Espera e segura: processo que requisitaram recursos previamente podem requisitar novos.
    - Não preempção: recursos previamente designados a processo não podem ser retirados. Os processos precisam liberá-los explicitamente.
    - Espera circular: deve existir um conjunto de 2 ou mais processos que podem ser organizados em uma lista circular, onde cada processo está esperando um recurso do processo anterior.
* Corresponde a um grafo de usos de recurso

* Métodos de solução:
    - Ignorar o problema (método avestruz)
        - Se acontecer, o sistema pode ser reiniciado
        - Avalia-se que o custo de tratar o problema é muito alto.
        - Se usuário tiver problema, apenas mata o processo
        - Abordagem Unix
    - Detecção (proteção e recuperação)
        - Mantém o grafo de recursos e realiza a busca de ciclos. Toda vez que um recurso for criado, é necessário checar o grafo e matar um dos processos que o querem.
        - Pode ser efetivo, porque não há tantos recursos preemptivos
    - Prevenção (evita uma das 4 condições)
        - Ataca uma das condições necessárias (Havender)
        - Espera e segura (wait for) - programa requisita todos os recursos de uma só vez
            - Desperdício de recursos
            - Variação: dividir programa em vários passos, com alocação de recursos por passos.
            - Espera indefinida: recursos não disponíveis ao mesmo tempo.
            - Exemplo: impressora com spooling
        - Não preempção - se não consegue um recurso libera todos
            - Adiamento indefinido (pode ocorrer)
            - Mais difícil de detectar
        - Espera circular - numerar os recursos e alocar sempre em ordem
            - Numeração significa que se novos recursos sãoadicionados programas podem terque ser reescritos.
            - Se numeração não reflete ordem de uso dos recursos, temos desperdício.
            - Aplicativos têm que se preocupar com restrição arbitrária e dependente da instalação.
    - Evitar impasses (avoidance)
        - Algoritmo do Banqueiro (Djikstra)
            - Metáfora de "empréstimos" e "pagamentos".
            - Usado em recursos do mesmo tipo (estensível a vários recursos).
            - O objetivo do algoritmo é que encontremos ALGUMA maneira de os processos terminarem de rodar (geralmente, "dando" o recurso para o processo que fez a requisição - para fazemos o teste - e então vemos se seria possível, ou não, acabar o processamento).
            - Cada processo devedeclarar uso máximo de recursos.
            - 3 números associados a cada processo: empréstimos, máximo, diferença.
            - Pelo algoritmo, recurso é concedido se o estado resultante é "seguro", ou seja, todos os processos eventualmente têm suas necessidades máximas de recursos atendidas.
                - Máximo de cada processo <= máximo do sistema
                - Supõe-se que todos os processos conseguem terminar se conseguirem o máximo de recursos
                - Deve existir recursos disponiveis para algum dos processos conseguir sua quantidade máxima.
                - Liberação dos recursos deste último processo deve liberar recursos para outro processo ter sua alocação máxima, e assim sucessivamente até que todos terminem.
        - Algoritmo do Banqueiro para vários recursos
            - Substituímos uma matriz por 2:
                - 1 linha por processo
                - 1 coluna por recurso
                - Recursos máximos (N)
                - Recursos alocados ao processo (A)
            - E um vetor: recursos disponíveis (D)
            - Ex:
            ```
                      .----------------------------------.
                      | Allocation |   Need   | Avaiable |
                      |  q1 q2 q3  | q1 q2 q3 | q1 q2 q3 |
                 .----|============|==========|==========|
                 | p1 |   0  3  1  |  7  2  2 |  3  1  3 |
                 | p2 |   2  0  0  |  1  2  2 |          |
                 | p3 |   1  0  0  |  8  0  2 |          |
                 | p4 |   2  1  1  |  0  1  1 |          |
                 | p5 |   2  0  2  |  2  3  1 |          |
                 '----'------------'----------'----------'
                
                  3 1 3 →  P4 pode terminar: libera 2 1 1
                  5 2 4 →  P2 pode terminar: libera 2 0 0
                  7 2 4 →  P1 pode terminar: libera 0 3 1
                  7 5 5 →  P5 pode terminar: libera 2 0 2
                  9 5 7 →  P3 pode terminar: libera 1 0 0
                 10 5 7 →  Sem mais processos: ESTADO SEGURO
            ```
            - O método GARANTE que, se precisarem de o máximo de recursos - no pior caso - todos PODEM acabar (existe uma maneira).

--------------------------------
## Entrada e Saída (IO)

* Dispositivos
    - De bloco:
        - Informação em blocos de tamanho fixo (128b -> 4K). Cada bloco com número sequencial.
        - Blocos podem ser acessados independentemente (acesso aleatório).
        - Discos, USB, disquetes, CD, DVD.
    
    - De carater:
        - Informação na forma de sequência de caracteres.
        - Leitura/escrita sequencial, sem retorno (stream).
        - Terminais antigos, teclado, mouse, interface de rede sensores.
        - Tem a ideia de FLUXO: uma vez retirada a próxima leitura, não teremos volta. Elas não podem ser mudadas.

* Organização
    - Dispositivos
    - Controladores
    - Software dependente de dispositivo (acessado pelo compilador C)
    
    - Sistema operacional lida com controlador
    ```    
        Dispositivo     Dispositivo : Componentes mecânicos
                  \     /           : 
                   \   /            : 
                Controlador         : Placa eletrônica
                     |              : (conversão analógico/digital)
                     |              : 
            Sistema operacional     : Software
    ```
    - Cada controlador tem alguns registradores que são endereçados pela CPU e acessados diretamente.
    - Uma possibilidade é usar espaço de endereçamente especial para I/O, sendo que cada controlador é designado para um intervalo.
    - SO executa entrada e saída "escrevendo" comandos nos registradores dos controladores.
    - Quando um comando é aceito, a CPU é usada para outro processo.
    - Quando uma requisição é realizada, o controlador gera um sinal eletrônico que causa uma interrupção.
    - Sistema consulta vetor de interrupção para localizar rotina de tratamento.
    - CPU faz a transferência da informação do buffer do controlador para a memória.


* Direct Memory Access
    - Antigamente, a CPU controlava a escrita do controlador para a memória. Como isso desperdiçava capacidade de processamento, foi criado um controlador especial (em hardware) chamado DMA. O DMA tem buffers e recebe/trata as interrupções dos controladores de IO. Assim, a CPU continua processando, e o DMA usa o barramento da CPU para isso. Como a CPU não poderá acessar a memória DURANTE a cópia do buffer do DMA para ela, ele usa os registradores + cache para seu processamento.
    - Informação é tranferida do dispositivo para a memória sem intervenção da CPU.
    - Interrupção gerada após final da transferência.
    - Durante transferência, CPU utilizada para outros processos.
    - CPU fornece local do buffer e tamanho da transferência além do número do setor do disco.
    - Controlador tem buffer local, pois transferência do dispositivo pode ser interrompida.

* Interleaving
    - Todos os discos têm vários pratos, cada um separado em setores com várias trilhas magnéticas. Um SETOR tem o tamanho do buffer do controlador de disco/DMA.
    - Ler várias trilhas consecutivas é apenas uma OTIMIZAÇÃO que aumentaria a velocidade. Porém, quando termina de ler um setor, o buffer está completo, e demora um tempo para passar do buffer do controlador do disco para o DMA.
    - Uma otimização é fazer o setor 1 não ir para o setor 2 diretamente. Considerando a velocidade de rotação do disco e a taxa de transferência do barramento, coloca-se o setor 2 três setores depois (por exemplo), pois num mesmo giro da cabeça de leitura, já seria possível ler a mesma trilha.
    - Se a leitura for de trilhas diferentes, essa otimização não ajuda.

* Software para IO: Objetivos
    - Objetivos
        - Independência do tipo de dispositivo:
            - "sort < input > output" deveria funcionar sempre
        - Uniformidade da detecção de erros
        - Tratamento síncrono/assíncrono
    - Quatro camadas
        1. Processamento de interrupção
        2. Drivers de dispositivo
        3. Software de I/O independente de dispositivo (interface)
        4. Software de usuário (funções em C)

* Processamento de interruções
    - Eliminam a ilusão de multiprocessamento
    - Objetivo: esconder interrupões o mais internamente possível do sistema (está dentro do microkernel, no Minix)
    - Em minix, processos são cloqueados quando comando de I/O emitido e interrupção é esperada (mesmo drivers)
    - Quando interrupção ocorre o processador de interrupção faz o necessário para desbloquear processo.
        - V() em semáforo, envio de mensagem em Minix, etc.

* Drivers de dispositivo
    - Um para cada tipo de dispositivo (ou classe de dispositivos semelhantes, ex. SATA)
    - Envarregado de traduzir comandos emitidos pelo software independente de dispositivo para particularidades do controlador.
        - Início e fim de buffer, tamanho da transferência, etc.
    - Encapsula conhecimentos sobre:
        - Número e dunção dos registradores de um controlador
        - Particularidades na organização do dispositivo
            - Linear, cilindos + trilhas + setores;
            - Cabeças de leitura;
            - Movimento do braço do disco;
            - Fatores de interleaving;
            - Atrasos mecânicos.
    - Ex: disquete
        1. Bloco N é requisitado.
        2. Se driver está livre, pedido é aceito, senão requisição é
           colocada em fila de espera.
        3. Requisição é traduzida em termos concretos:
            1. Verificar onde estão os setores correspondentes ao bloco pedido.
            2. Onde está o braçodo disco.
            3. Se motor do braço está funcionando.
            4. Se disquete está girando.
        4. Decide comandos a serem emitidos (iniciar rotação, mover braço, etc.)
        5. Emite comandos, escrecendo nos registradores do controlador (um de cada vez ou lista ligada, dependendo do controlador)
        6. Driver bloqueia esperando resultado
            - Quando operação termina sem demora (ex. Memória gráfica), driver precisa bloquear
        7. Após execução, verifica se houve erro e pode pedir para reexecutar.

* Software independente de dispositivo
    - A maior parte do do software de I/O é independente de dispositivo.
        - Nomes de arquivo, número de bytes a ser lido, posição a ser olhada, etc.
    - Cuidado: ainda temos CLASSES de I/O: bloco e caracter.
    - Divisão exata de limites entre software independente de dispositivo e drivers depende do sistema.
        - Em alguns sistemas, driver faz funções que poderiam ser independentes, oferencendo visão mais abstrata.
    - Funções básicas
        1. Interface uniforma para drivers
        2. Administração de nomes de dispositivos
            - Mapeia nomes simbólicos para dispositivos reais
            - Em Minix I-node para arquivo especial, o "major device number" é usado para localizar driver e o "minor device number" é passado como parâmetro para localizar qual o dispositivo.
        3. Proteção para dispositivos
            - Como prevenir acesso indevido?
            - Minix/UNIX usa proteção do sistema de arquivos
        4. Possibilitar blocos de tamanho padrão
            - Dispositivos diferentes podem ter tamanhos de setores distindos
            - Camadas mais altas vêem apenas "dispositivo virtual" padrão.  
    - Outras funções básicas
        1. "Buffering"
            - Bloco: Hardware lê/grava blocos grandes, usuário pode ler/gravar até um byte.
            - Char: usuários podem escrever mais rápido que dispositivo pode aceitar/teclado pode chegar antes do uso.
        2. Cuidar da administração do espaço alocado em dispositivos de bloco (se não houver um bloco grande o suficiente, é ele que pode, por exemplo, particionar os dados em várias partes).
        3. Reservar e liberar dispositivos dedicados
            - Fitas magnéticas, gravador de backup, dvd, etc...
        4. Relato de erros
            - Maior parte feita pelos drivers, já que erros em sua maioria são específicos de dispositivos.
    - Esse é o SISTEMA DE ARQUIVOS

* Software no nível de usuário
    - Rotinas de I/O em Bibliotecas
        - printf, scanf, fprintf, etc.
    - Comandos de leitura/escrita em linguagens.
    - Colocam parâmetros nos locais corretos para chamadas de sistema.
    - Rotinas que constroem I/O a partir de especificações (atoi, printf, etc.)
    - Outros programas
        - Spooling
            - Spooling directory + daemon. O daemon é o único com acesso à impressora.
        - E-mail, acesso à redes, etc.
    - NÃO fazem parte do SO.

* I/O em Minix
    - Interrupções
        - Já tratado, feito no Kernel
    - Drivers dos dispositivos
        - 1 para cada classe de dispositivos de I/O
        - Processos completos, com seu estado, registradores, mapas de memória, etc.
        - Comunicação inter-processs e com sistema através de mensagens (em geral, para o System Task)
        - Cada driver escrito em arquivo fonte separado.
            - Escrita de áreas de outros processos através de mensagens do Kernel.
        - Processos normais, apenas com priviléfios diferentes de mensagens (funcionam do mesmo modo, mas podem mandar algumas mensagens que outras partes do sistema não podem).
    
    - Implementação diferente do Unix

    | Minix | Unix |
    | ----- | ---- |
    | processo independente, comunicação por mensagens | Duas áreas no processo: user space e kernel space |
    | mais modular | Chamada ao sistema como chamada de procedimento |
    | vide figura abaixo | Causa interrupção, kernel verifica se OK, muda para kernel space |
    |  | Mais rápido |

    ```
             User   User  ...  User      
             app    app        app        
                     ↘                   
             Pros      File             
            Server    Server              
                        ↘                
            Audio  ...  Disk   Printer    
            Driver     Driver  Driver           
                         ↙              
             Kernel  System    Clock           
                      Task     Task     
    ```
    
### Principais Drivers do Minix: 

* RAM Disk
    - Permite tratar a memória como dispositivo de block, mas com acesso instantâneo.
        - Várias áreas da memória podem ser usadas como RAM disk, cada uma com minor device number diferente.
        - RAM Disks podem ter interrupção.
        - 4 RAMdisks
            - /dev/ram
                - Disco RAM própriamente dito, usado para salvar os
                  arquivos mais acessados frequentemente com o objetivo
                  de acelerar seu acesso.
                - Utilizado para versões cd/dvd
                - Em Minix1 era usado para rodar Minix em computadores
                  com apenas 1 floppy, colocava-se o "root" em um
                  RAMdisk
            - /dev/mem
                - Usado para tornar o acesso a memória padronizado 
                  como qualquer acesso a dispositivo de bloco.
                - Só pode ser usado como super-usuário
            - /dev/kmem
                - Semelhante ao anterior, mas tem como endereço ZERO
                  o primeiro byte da memória do Kernel.
                - Localização intersecta a do ramdisk anterior.
            - /dev/null
                - Arquivo especial que aceita dados e dos descarta
                - Utilizado por usuários da shell para descartar saídas.       
    * Discos
        - Hardware
            - Discos são organizados em cilindros, cada um contento uma
              ou mais trilhas (dependendo do número de cabeças)
            - Cada trilha é dividira em setores, todos com mesmos
              tamanho.
            - Velocidades de acesso têm velocidades diferentes.
        - Software
            - Algoritmos para escalonamento de disco
            - 3 tempos dominam o uso de um disco: tempo para rotação do braço, tempo de latência para ligação e tempo de leitura.
                - Otimizando-se o movimento do braço do disco pode-se melhorar significativamente a performance de sistemas com muitos processos.
                - Requisições são geralmente mantidas em uma tabela por cilindro, cada entrada em uma lista ligada.
                - Suporemos requisições para cilindros: 3, 36, 16, 34, 9, 13 e braço no cilindro 14.
                - Métodos:                    
                    - FIFO
                        - Dispensa explicações
                        - No exemplo: 3, 16, 16, 34, , 13 (111 trilhas percorridas)
                        - Pode ter adiamento indefinido
                    - SSTF (Shortest Seek Time First)
                        - Próxima requisição é aquela que envolve menor movimento do braço
                        - No exemplo: 13, 16, 9, 3, 14, 16 (53 trilhas percorridas)
                        - Melhor fluxo (throughput)
                        - Espera indefinidamente longa é provável, conjunto pequeno de processos pode monopolizar discos.
                        - Favorece trilhas do centro, pode ter adiamento indefinido
                    - Algoritmo do elevador
                        - Próxima requisitção a ser tratada é aquela que envolve o menor movimento do braço, na mesma direção do último movimento, senão apenas o mais próximo.
                        - No exemplo: 13, 9, 3, 16, 34, 36 (44 trilhas percorridas)
                        - Favorece setores internos (centro) do disco (alguma probabilidade de adiamento indefinido)
                    - Elevador unidirecional com duas filas
                        - Usa apenas uma direção para atender os pedidos
                        - Supondo de fora para dentro: 13, 9, 3, 36, 34, 16 (64)
                        - Elimina favorecimento de trilhas
                        - Aceleração do braço tem demora, voltar direto ao início é mais rápido que percorrer soma de trilhas.
                        - Algoritmo ideal, cria fila para novas requisições
                    - Variação: após início do movimento, novas requisições entram em lista separada
                        - Elimina possivbilidade de adiamento indefinido
                    - Escalonamento de setores dentro de trilha: ordenação otimiza atrasos de rotação desnecessários.
                    - Buffer de trilha: tempo de busca para noa trilha maior que ler trilha completa.
                        - Alguns drivers guardam trilha inteira para otimizar acessos consecutivos na mesma trilha.
                        - Às vezes feita pelo controlador.
            - Erros
                - Erro de programação (# de setor inválida) - abortar ou avisar
                - Checksum error: transiente (poeira?) - repetir
                - Checksum error: permanente (bloco com dano físico) - marcar bloco
                     - Problema: backup pode ser por bloco
                     - Arquivos com blocos inválidos (sistema de arquivos)
                     - Trilhas reserva, blocos inválidos substituídos por outros reserva automaticamente.
                - Erro de busca (braço foi para setor errado): alguns controladores já reconhecem sozinhos.
                - Erro no controlador: precisa ser resetado.
            - Minix Floppy: exemplo de driver
                 - Dispositivo removível barato comum até os anos 90
                 - Disco de material flexível
                 - Alto desgaste, baixa velocidade
                 - Pouca probabilidade de concorrência no floppy
                     - Blocos de 1k, setores de 512b: blocos grandes minimizam acesso ao disco, mas são opostos.
                     - FIFO: pouca probabilidade de concorrência no acesso ao floppy.
                 - do_rdwt (rotina do driver para leitura e gravação)
                     - dma-setup: inicializa dados do chip DMA para que esta transfira diretamente dados à memória.
                     - start_motor: verifica se motor está funcionando, senão emite comando para ativá-lo.
                     - seek: verifica se o braco está no lufar certo, senão instrui o controlador e dá um receive.
                     - transfer: emite comando de leitura/escrita e em seguida dá um receive, o qual verifica registradores do controlador. Se erro, retorna código de erro
                 - clock_mess: manda mensagem ao clock driver para acionar timer que chama procedimento para desligar o motor em 3 segundos (caso haja novo acesso em breve, motor já está ligado, senão, desligamento evita desgaste excessivo).
                 - stop_motor: procedimento para parar motor.

-------------------------------------
## Drivers de relógio

* Hardware
    - Dois tipos:
        - Ligado à rede elétrica (gera interrupções em ciclos de 1/60 segundos, no Brasil) - não usados atualmente
        - Criscal (oscilador de cristal + contador + registrador com constante): contador é inicializado e decrementado a cada sinal do criscal, a frequências de até 20Mhz)
    - Manter hora correta
        - 64 bits
        - Tempo em segundos + tics
        - Hora do boot em segundos + tics
    - Funções importantes:
        - Não deixar processos rodarem por mais que sua fatia de tempo.
        - Quando proceso começa a rodar, relógio é iniciado pelo valor do quantum, quando interrupção é gerada, escalonador é gerado.
    - Contabilizar uso de CPU
        - Campo na tabela de processos contabilizado por tics.
        - Atualização (verificação de qual processo irá rodar) feita a cada interrupção (não a cada tic, o que seria muito caro).
    - Implementar alarmees (chamada ao sistema por processos de usuário)
        - Alarme geralmente é um sinal (para que o processo possa rodar  e essa chamada seja ASSÍNCRONA).
        - Pode-se usar lista ligada com tempos dos alarmens, com diferença para a próximo alarme e contador de tivs até próximo alarme: quando hora do dia é atualizada, driver verifica se algum timer venceu.
    - Fornecer alarmes especiais para o sistema
        - Watchdog timers: semelhante aos sinais para usuário, mas driver chama procedimento estabelecido quando se programou alarme (e.g. stop_motor no Floppy Minix).
    - Coleta de estatísticas em geral
        - Profiling: a cada tic, driver verifica se processo está sendo "profiled" e atualiza contador de tivs do "bin" correspondente ao PC.
    - Tipos de mensagem 
        - SET_ALARM
            - PID, procedimento a ser chamado, espera
            - Para usuário, chamada de sistema através de processo servidor.
            - Cada processo pode ter apenas um alarme, novo alarme cancela o anterior.
        - GET_TIME
            - Retorna segundos desde 1/1/1970
        - SET_TIME
            - Apenas pelo super-usuário
            - Poderia alterar funcionalidades relacionadas ao chrown, (agenda de tarefas). Todos os procedimentos periódicos do SO poderiam ser desativadas - e com isso, o procedimento de segurança.
        - CLOCK_TICK
            - Mensagem enviada ao driver quando interrupção de relógio ocorre.
            - Driver atualiza contadores, verifica alarmes e vê se fatia de tempo venceu.

---------------------------------------------------
## Driver de terminal

* Hardware RS232
    - Terminais com tela e teclado que se comunica por interface serial, 1 bit de cada vez, em geral 10 bits de 25 pinos: 1 para transmitir, 1 para receber e 23 para funções de controle (300-9600bps).
    - 10 bits para transmitir 1 bit de indormação: codificação mais comum usa 1 bit deinício, 7-8 bits de dados com 1 bit de paridade, 1 ou 2 bits de parada.
    - Gradualmente substituídos por USB nos micros pessoais, mas ainda muit usados em vaixas registradoreas, leitoras de código de barra, e dispositivos de controle remoto industrial.
    - Hardcopy (impressoras), glass TTY (terminais burros), terminal inteligente (memória + CPU, entende comandos e após sequência de escape), blit (terminal inteligente gráfico serial).
    - Barramento/UART (Universal Assincronous Receiver/Transmiter)
        - Driver escreve um caracter por vez que UART transmite.

* Hardware Mapeados na Memória
    - Memória especial (vídeo RAM), que é parte do espaço do endereçamento (controlador de video)
    - Parte integral do computador
    - Cοntrolador do vídeo, no mesmo cartão que a VRAM, lê VRAM e manda sinal de vídeo para monitor.
    - "Character mapped display" (IBM-PC original)
        - Cada caracter ocupava 2 bytes, um com dado e outro com atributos - cor, reverso, piscar, etc).
        - Tela com 25x80 varacteres,
        - Caracter aparece na tela no próximo ciclo do monitor.
        - Muito mais rápido que RS232: 12ms x 2083ms.
    - Bit mapped
        - Cada pixel no terminal controlado por 1 ou mais bits na memória.
        - Sinais para apertar e soltar uma tecla, permitindo combinações diferentes na tecla.

* Tarefas de teclado
    - Coletar caracteres
    - Converter # da tecla para código (ASCII, etc.)
    - Bufferização (usuário ainda não está esperando entrada).
        - "poop" de buffers: alocados dinamicamente conforme necessidade
        - Buffer direto no registro do terminal - driver mais simples.
    - echo (imprimir na tela o que foi digitado)
        - echo em hardware é ruim (como para modo senha).
        - pode ser complicado se usuário quando programa está "imprimindo".
        - Tratar quando o tamanho é maio que a tela (wrapping).
        - TABS (como colocar seus tamanhos).
        - Tratamento de teclas de escape e suas ações.

* Tarefas de tela
    - Mais simples que teclado
    - RS232 diferente de Mapeado em Memória
        - RS232:
            - BUffers de saída diferentes para cada terminal
            - Após toda a saída ser copiada para buffer, caracter é enviado e driver bloqueia até completar transmissão.
        - Mapeado em memória
            - Cada caracter é colocado unicamente na tela.

--------------------------------------------
## System Task (driver em kernel mode)

* Processo rodando no espaço de endereçamento do kernel (juntamente com clock task).
* Até o Minix 2, todos os drivers eram tasks (dentro do Kernel Space), por questão de eficiência.
* Recebe todas as chamadas ao Kernel
    - Divisão em camadas do Minix impede comunicação dos servidores com Kernel, porém existem tarefas que devem ser relegadas a este (ex: criação de processo com chamada 'fork')
    - Recebe também chamadas dos Drivers.
* Mensagens tratadas
    - **SYS_FORK:** Process manager informa sistema que novo processo deve ser criado. Mensagem contém índices onde pai e filho devem estar na tabela (da qual PM e FS têm sua própria cópia)
    - **SYS_NEWMAP:** Após criar bnova entrada na tabela, PM aloca memória para este processo filho. COm esta mensagem, MM informa o Kernel sobre novo mapa de memória para processo filho. Passa um ponteiro para o mapa. Essa informação é usada para preencher os p_regs contendo os "segment registers"
    - **SYS_EXEC:** Chamada pelo PM quando o processo invoca exec(). Cria nova pilha para o processo e aloca argumentos nela. SYS_ECEV recebe apontador para nova pilha e informa o kernel sobre ela.
    - **SYS_XIT:** Quando o processo emite a chamada EXIT.

-----------------------------------------------

## Administração de memória

* Quando?
    - Sempre que existe um Sistema Operacional

* Hierarquia
    - CACHE ↔ Memória Principal ↔ Memória Secundária
    - Princípio da localidade central na ideia de Cache
        - De forma geral, ao criarmos um programa, as regiões de memórias que acessamos são as que estão próximas da que acabamos de acessar (o que costumamos fazer ao percorrer vetores, matrizes, entre outros).
* Tipos de administração de memória
    - trivial: colocamos todo o programa na memória
    - Escolha (fetch) - quando escolher o próximo pedaço a ser transferido para a memória
        - "demand": sóquando o trecho de memória é requisitado.
        - antecipatória: sistemas de alta memória
    - Colaboração onde colocar um programa/trecho na memória
        - Qual será a posição do programa na memória
    - Reposição ("replacement"): o que retirar da memória para livrar espaço.


### Memória real

* Monoprogramação sem "swapping"
    - Só 1 programa +SOnamemória decada vez.
    - Quando um processo termina (e só então), um novo processo é carregado em seu lugar.
    ```
     .------------------------.
     | Extended Memory        |
     |                        |
     | BIOS ROM               |
     |                        |
     | UMB (unused mem block) | 
     |                        |
     | Network ROM BIOS       |
     |                        |
     | Hard Disk ROM BIOS     |
     |                        | 
     | UMB (unused mem block) | 
     |                        |
     | Video ROM BIOS         | BIOS: Basic Input
     |                        |       Output System
     | UMB (unused mem block) | (uma espécie de "driver")
     |                        |
     | Video RAM              |
     |                        |
640K |------------------------|
     |                        |
     | Avaiable for Programs  |
     |                        |
     |------------------------|
     | COMMAND.COM (shell)    |
     | Configuration Files    |
     | BIOS Code              |
     | System Data Place      |
     '------------------------'
        ```
* Programas muito grandes: OVERLAY
    - Programador definia que parte do programa é carregado de cada vez.
    - Área fixa e área de troca de memória
    - Comandos para substituir partes da memória.
* Multiprogramação
    - Computadores usavam em média apenas 20% da CPU durante a execução dos programas
    - Computadores extremamente caros:necessário maximixar o uso
    - Mais de um programa namemória de cada vezpermite usar a CPU quando outro programa está aguardando ação de I/O
    - Como fazer com a memória? (onde colocar? quem colocar?)    
* Particionalemnto da memória
    - A memória é dividida em várias partes.
    - O programa é colocado em uma das partições.
* Partições fixas
    - Partições na memória são pré-definidas na instalação
    - Proteção por "boundaru registers" (no IBM360 o código de acesso ficava na PSW)
    - 1 fila de "jobs" para cada partição (simples para o SO).
    - Problemas: Fragmentação Interna (uso parcial da memória da partição - FRAGMENTAÇÃO é usualmente associada a um desperdício de memória) e partições podem ficar ociosas por muito tempo (pois os programas ficam APENAS dentro daquela partição, e não podem usar outra vazia enquanto ela está ocupada).
    - Tradução e "loading" absolutos
        - Compilador é informado sobre qual será a partição utilizada e gera diretamente endereços absolutos dentro da partição.
        - Filas de jobs por partição.
    - Tradução e "loading" relocáveis
        - Compilador compila programa de maneira que ele possa rodar em qualquer partição
        - Mantém lista de endereços absolutos e os atualiza para carregar o programa (tabela com endereços). O programa é carregado com um pré-processamento pelo LOADER.
        - Endereço relativo a um registrador-base (possível mover programa na memória) - torna o programa  mais lento (para acessar o registrador), mas permite mais flexibilidade.
    - Usando qualquer uma das técnicas, o SO pode colocar o processo para rodar onde ele couber.
        - Ambas as técnicas são difíceis em C.
* Partições variáveis
    - Sem limites fixos
    - Programas ocupam apenas um espaço previsto.
    - É necessário uma "lista livre" para os pedaços disponíveis.
    - Problema: Fragmentação externa (buracos pequenos demais para serem aproveitados).
        - "coalescinf holes" (buracos adjacentes se juntam)
        - Compactação: mantém espaço livre contíguo
            - Tempo de compactação é improdutivo (overhead).
            - Sistema previsa parar (ruim para tempo real e interetivo).
            - Como envolve realocar programas, é necessário interromper toda a execução do sistema.
            ```
                    .----------.    .----------.    .----------.
                    |    SO    |    |    SO    |    |    SO    |
                    |----------|    |----------|    |----------|
                    |==========|    |          |    |          |
                    |==========|    |   FREE   |    |   FREE   |
                    |==========|    |          |    |          |
                    |++++++++++|    |++++++++++|    |++++++++++|
                    |++++++++++|    |++++++++++|    |++++++++++|
                    |++++++++++|    |++++++++++|    |++++++++++|
                    |**********|    |**********|    |          |
                    |**********|    |**********|    |   FREE   |
                    |**********|    |**********|    |          |
                    |   FREE   |    |   FREE   |    |          |
                    '----------'    '----------'    '----------'
            ```
    - Para melhorar, podemos usar estratégias de colocação:
        - First-fit, next fit: rápido e simples.
        - Best-fit:  quanto menor o desperdício de memória, melhor (mas gera vários pedaços pequenos, fragmentados, e inúteis).
        - Worst-fit: melhor deixar buracos maiores, com maior chance de serem usados.
    - Implementação
        - Bitmap:
            - 1 bit para cada região de memória (a região deve ser maior do que bytes)
            - Encontrar "zero runs" ao alocar (se o programa precisa de uma certa quantidade de memória, é necessário encontrar um trecho, pelo bitmap, identificado pelos 0's, para ele ser alocado)
            - Operação muito lenta (precisa buscar no bitmap e ficar analisando ao alocar o programa)
        - Lista ligada
            - Mais rápido que a anterior.
            - Uma ou duas listas (partições usadas e livres).
        - "Buddy system"
            - Tamanhos pré-definidos como potências de 2
            - 1 lita para cada tamanho (32 listas em memória de 4GB, mas havia MUITO MENOS na época).
            - Quando o espaço é liberado, Vê-se se o espaço contíguo é livre. Nesse casp, junta e repete o processo. A busca é apenas em uma lista.
            - O processo é muito rápido
            - Problema: fragmentação interna (média de 25%, com podencial de quase 50%)
    - Swapping
        - Possível com ambos os esquemas anteriores (partição fixa e variável).
        - Programa pode ser retirado damemória para otimização de recursos.
        - Programas são retirados quando:
            - Inativo e há outro para rodar (esperando I/O).
            - Programa precisa de mais memória - precisa trocar de partição.
        - Precisa de "swap space" na memória secundária (disco).
        - No Linux, existe área de swap, mas ela funciona para a memória virtual. Em SO, porém, costuma-se associá-la com a memória real.
            
### Memória virtual
    
* Com o swapping, era possível carregar e retirar um programa de lugar. Por que não poderíamos colocar trechos de programas espalhados?
* Os endereços "virtuais" podem não ser os mesmos dos reais
* É necessário um mecanismo de tradução eficiente, sob o preço de perder a vantafem de reuso da CPU: o apoio vem do hardware.
* Permite endereçar um espaço maior de memória
    ```    
                     Virtual                   Physical
                    addresses       Address    addresses
                |               |  Conversion  |       |
                |       *-------:-----------.  |       |
                |       *-------:-------.   |  |       |
                |       *-------:---.   |   '->|       |
                |               |   |   |      |       |
                |               |   '---:----->|       |
                |               |       |      |       |
                |               |       |      
                |               |       |    Disk addresses
                |               |       |        _____
                |               |       |      .´     `.
                |               |       '----->|`-----´|
                                               |       |
                                                `-----´
    ```        
* Como a memória virtual pode ser maior que a real, é necessário armazenar trechos não utilizados.
* Quando não está armazenada dentro da memória, pode-se colocar a parte do programa no disco. Isso, porém, é eficiente APENAS se valer o princípio da localidade.
* Simplifica a codificação e compilação do programa.
 * Memória organizada sempre em BLOCOS
    - Overhead e manipilação de 1 palavra de cada ve é excessivo
    - Blocos pequenos envolvem maior overhead
    - Blocos grandes demrammais para transferir e cabem em menor número na memória real.
    - PÁGINAS: blocos com tamanhofixo
    - SEGMENTOS: blocos com tamanho variável

#### Funcionamento:
* Cada endereço tem 2 partes: base e deslocamento.
* Tabela de mapeamento de ndereços (por processo? por memória?).
* Registrador especial indica início da tabela de tradução.

    ```
            .---------.           .---------.---------.
            |    A    |           |    B    |    D    |
            '---------'           '---------'---------'
                 |                     |            |
               .---.                   |            |
               | + |<------------------'            |
               '---'                                |
                 |                                  |
                 |   A  .-------.            .---.  |
                 |    |||       |       .----| + |<-'
                 |    |||       |       |    '---'
                 |    |||       |       |      |
                 |   B|||       |       |      |
                 |    \/|-------|       |      |
                 '----->|R|  |B'|-------'      '-> E = B' + D
                        |-------|
                        '-------'
    ```
* Funcionamento básico
    - Hardware do computador verifica tabela de indexação (bit R da tabela)
        - Se a página está na memória, acessa o endereço real (a partir de B', usando o deslocamento)
        - Se a página não está na memória, lê seu conteúdo da memória secundária (ex: disco)
            - Caso a memória esteja cheia, escolhe um bloco para ser removido temporariamente.
            - Eventualmente a remoção temporária pode envolver escrita no disco.
* Campos da tabela de indexação
    - Endereço na memória real do início do bloco
    - Endereço namemória secundária do bloco
        - Usado para buscar páginas ausentes e para gravar páginas que são retiradas da memória
        - Dirty bit
            - Defina quando uma página deve ser escrita na memória secundária (se não foi modificada, já economiza um acesso ao disco)
        - Reference bit
            - Informa se a página foi acesso após a primeira carga.
        - Proteção
            - Usado para verificar se o acesso é válido.
- Todas as memórias virtuais costumam oferecer um dirty bit e o reference bit. A proteção nem sempre existe.
* Blocos de tamanho fixo chamados "páginas"
* Páginas da memória alinhadas em endereços múltiplos do tamanho das páginas.
    - Para obter enderelo real basta concatenar número da página real com o deslocamento.
    - Entrada na tabela de mapeamento:
        - Residence bit
        - Endereço da página na memória secundária
        - Número da página na memória real
    - Problema: fragmentação interna
        - Média de meia página de desperdício
* Mapeamento direto
    - Antes de um processo começar a rodar, o SO carrega o endereço da tabela de mapeamento no registrador especial.
    - Ciclo de mapeamento direto domina tempo de ecevução da instrução.
        - Mapeamento direto duplica tempo de acesso a memória (dois acessos para obter um endereço: acesso à tabela e acesso final).
        - Pode-se tentar colocar a tabela de processos em memória de alta performance (cache).
* Mapeamento associativo
    - Memória associativa é endereçada por conteúdo.
    - Verificação feita em paralelo, tempo de acesso de uma ordem de magnitude mais rápido que a memória ordinária.
    - Memória associativa guarda a tabela com últimos acessos.
    - Endereçamento da MA é feito pelo número da página virtual. O conteúdo é o número da página real.
    - Princícipio da localidade garante o funcionamento.
    
    ```
      .------------------------.
      | Virtual Page | Displa- |
      |    Number    | cement  |
      '------------------------'
             |
             |              .--------.--------.
             |------------->|        |        |
             |              |--------|--------|
             |------------->|        |        |
             |              |--------|--------|
             |------------->|        |        |
             |              |--------|--------|
             |------------->|        |        |
             |              |--------|--------|
             |------------->|        |        |
             |              |--------|--------|     .-------.---------.
             |------------->|        |        |---> | Page  | Displa- |
             |              |--------|--------|     | Frame | cement  |
             |------------->|        |        |     '-------'---------'
             |              |--------|--------|
             '------------->|        |        |
                            '--------'--------'
    ```
* Tabela de páginas + memória associativa
    ```
        .-------.
        | P | D |
        '-------'
         | |
         | |      .--------.--------.
         | |----->|        |        |
         | |----->|        |        |
         | |----->|        |        |
         | |----->|        |        |
         | |----->|        |        |
         | |----->|        |        |---> TLBbit
         | |----->|        |        |        |
         | '----->|        |        |        |
         |        '--------'--------'        \/
         |
         |  TLB miss  
         '----------->
    ```

#### TLBs
* Memória de acesso rápido para aumentar velocidade da tradução do endereço virtual para o endereço real.
* Em geral, de 4 a 64 entradas.
* Várias maneiras de organização:
    - Mapeamento direto:
        - Primeros bits do endereço indicam entrada da tlb, que então verifica se outros bits da página virtual armazenada são iguais, caso seja, usa número da página real.
        - Cada página pode estar em apenas uma posição na TLB.
        - Podemos ter que remover a entrada mesmo quando há entradas vazias.
    - Mapeamento totalmente associativo:
        - busca simultânea de todas as chaves
    - Maepamento n-associativo:
        - bancos associativos, cada um com n entradas
        - primeiros bits indicam banco, cada um com n entradas
        - cada página pode estar em n posições da TLB
    - Custo aumenta com associatividade, velocidade também.

#### Segmentação
* Um programa pode ocupar vários "segmentos" de memória, cada um com um tamanho diferente.
* Elimina fragmentação interna.
* Divisão em segmentos geralmente é "lógica".
* Proteção:
    - Implementação mais complexa para proteção de acesso inválido.
    - Porém, implementação de códigos de acesso por segmento simplificada (divisão lógica, não física).
    - Códigos de acesso: leitura, escrita, execução, "append" (este último, que aumentava o tamanho do segmento)
* Campos tipicos de uma tabela de segmentos:
    - Residence bit
    - Tamanho
    - Bits de acesso (r,w,e,a)
    - Endereço do início do segmento na memória.
    - Endereço do início na memória secundária.

#### Compartilhamento de memória

* 2 entradas em tabelas de blocos diferentes podem apontar para o mesmo segumento da página na memória.
* Compartilhamento pode reduzir utilização de memória
    - Ex: código reentrante - dados separados, mesmo segmento de código.
* Compartilhamento pode aumentar velocidade (menos falhas de página).
* Compartilhamneto mais fácil em sistemas segmentados, posi a divisão de clocos e memória é lógica.
    - Em sisteams paginados, a administração de "páginas parciais" pode ser complexo.

#### Segmentação + Paginação

* Memória dividida em páginas
* Páginas agrupadas em segmentos
* Uma tabela de segmentos
* Cada segmento tem uma tabela de páginas
* Endereço maior que a memória
* Vantagens de ambos os sistemas
    - Sem fragmentação externa (mas TEM interna).
    - Unidade lógicas para compartilhamento.
* Maior overhead de processamento (minimizado pela TLB).
* Endereço dividido em 3 partes:
    - segmento
    - página
    - deslocamento

    ```    
                    Memória Virtual                       Memória Real
            
          9 bits    11 bits      12 bits                   |       |
        |   1    |     3     |    3267    |                |-------|
            |          |            |                      |       |
            |          |            '------------------.   |       |
            |          '---------.                     |   |       |
            |                    |       Tabela de   .-:-> |-------|
            |      Tabela de     |        páginas    | |   |       |
            |      segmentos     |      do segmento  | |   |       |
            |      .--------.    |      .----------. | '-> |       |
    STRB .--:--> 0 |        |  .-:--> 0 |          | |     |-------|
     *---'  |    1 |        |  | |    1 |          | |     |       |
            |    2 |        |  | '--> 2 |    *-----:-'     |       |
            '--> 3 |   *----:--'      3 |          |       |       |
                 . |        |         . |          |       |-------|
                 n |        |         m |          |       |       |
                   '--------'           '----------'       |       |
                                                           |       |
    ```

#### Administração de memória virtual  

* Estratégias de colocação (placement stragegies)
    - Em qual dos lugares vagos colocar?
    - Sistemas segmentados: mesmas estratégias que partições variáveis (first fit, best fit, worst fit).
    - Sistemas com paginação (paginação pura ou segmentação com paginação) → indiferente.
    - Em geral, para otimizar, traz-se a quantidade de páginas correspondente a um bloco dentro do disco (se o disco tem blocos com 2mb, e as páginas têm 4kb, teremos 5 páginas carregadas ao mesmo tempo).

##### Estratégias de reposição de páginas

* A melhor página para ser retirada da memória, caso não haja espaço, é aquela que demorará mais tempo para ser usado (inclusive as que enventualmente nunca seráo usadas).
* Randômica
    - Supõe poucos processos com uso de memória
    - Baixa probabilidade de escolher uma página importante.
* FIFO
    - Princípio: se a p[agina está há mais tempo na memória, já deve ter sido bastante usada e, portanto, a localidade já deve ter mudado.
    - Probelma: muitas vezes, algumas páginas ficam muito tempo na memória porque são muito usadas (ex: Código de editor de textos, rotinas de chamada de SO).
    - Anomalia FIFO (Belady et a, 1969): em alguns casos, mais memória na FIFO pode causar mais falhas de página.
        - Memória Real com 3 ou 4 páginas
        - Páginas virtuais acessadas: 0,1,2,3,0,1,4,0,1,2,3,4
* Variação de FIFO: fifo segunda chance
    - Só são respostas às páginas com o "reference bit" desligado.
    - Quando é necessário repor a página, a fila vai sendo examinada.
    - Se a página da frente tem o "reference bit" desligado, a página é reutilizada.
    - Caso contrário, o vit é desligado e a página é movida para o final da fila.
* LRU - Least recently used
    - Repõe a página menos usada recentemente, ou, em outras palavras, que foi usada há mais tempo.
    - Se a págna não é usada há muito tempo, não deve ser usada no futuro próximo.
    - Considerada boa política: problema é a implementação.
    - Possíveis implementações:
        - Contadores para cada página, atualizadado a cada acesso, resposta de página com menor contador.
        - Quando uma página é acessada, tira-se do meio da pilha e coloca no topo. Resposta: página embaixo da pilha.
    - Alto overhead (cada acesso)
    - Falhas:
        - Loops longos
        - "deeply nested calls"
* NUR - Not Used Recently
    - Aproximaão de LRU com menos overhead.
    - Mantém marcadas páginas usadas "recentemente".
    - Leva em conta também que é melhor repor página que não foi alterada na memória (1 vs 2 acessos ao dico).
    - Usados 2 bits por página: access bit (para referência) e dirty bit (para modificação)
    - Procura de páginas a serem repostas:
        - Página não referenciada
        - Páginas referenciadas e não alteradas.
        - Párinas referenciadas e alteradas.
    - Problema: eventualmente todas as páginas serão referenciadas.
        - Pediodicamente zerado o "access bit" - páginas muito usadas ficarão vulneráveis por pouco tempo.
    - Implementação
        - Hardware com atualização automática de dirty bit e access bit.
        - Uso de residence bit e bit de proteção de escrita
            - Residence bit substitui access bit: páginas após serem carregadas não são marcadas como residentes. Tabela adicional indica que a página está presente. No próximo acesso, após falha de página, algoritmo de paginação apenas seta o "residence bit".
            - Protection bit substitui dirty bit: página ao ser carregada é marcada como protegida para escrita. Primeira escrita gera falha de proteção. Algoritmo de paginação seta bit em tabela adicional, indicando que a página foi modificada e retira proteção.
* Working set
    - Peter Denning (1968) criou teoria de que cada programa tem seu "conjunto de trabalho" (ex: página com o loop e páginas com matrizes sendo multiplicadas).
        - Em geral, páginas que envolvem o código do programa, a pilha, etc (usando partes de cada uma delas).
    - Para um program rodar eficientemente, seu conjunto de trabalho precisa estar na memória real.
    - Política de working set tenta manter este conjunto sempre na memória.
    - Um processo deve ser mantido em memória apenas se seu conjunto de trabalho está inteiro na memória, senão deve ser retirado (swapped out).
    - Ideia é prevenir "thrashing" (quantidade excessiva de falhas de página que causa velocidade inviável de processamento) e, ao mesmo tempo, manter o maior número de processos em memória possível.
    - Implementação
        - Conjunto de trabalho CE(t,j) calculado no tempo T é o conjunto de páginas utilizadas nos últimos j acessos.
        - j é a "janela" do CE.
        - Problema: como manter o working set:
            - Como calcular T?
            - Janela é móvel, ao acessarmos a página nova, a página mais antifa referenciada pode sair do CE - muito caro calcular T.
        - Para evitar sobrecarga de contar todos os acesso, mantém-se uma aproximação do tempo do último acesso eusar janela de tempo.
            - Podemos usar contabilidade a cada TIC e resetar o "reference bit"
            - Se resetarmos o reference bit, periodicamente temos o NRU.
    - Como "simular" um thrashing?
        - Criar vetor maior que a memória (ex: 8GB).
        - Acessar elementos aleatoriamente.
    - Working set: WSClock
        - Conceitos de WS e de fifo segunda chance
        - Funciona bem e é utilizado.
        - Porém um conjunto de não muito claro de ideias diferentes, típico de sistemas reais.
        - Funcionamento
            - Para cada processo temos residence bit (R) e dirty bit(M) mantidos por hardware. A cada k clicks R é resetado
        - A cada tic todas as páginas são examinada e o tempo é guardado para aquela sonde R vale 1. Campo de tempo é aproximação do último acesso, pela resolução do relógio (na verdade, podem ser k tics)
        - Páginas que têm tempo igual a k-j são candidatas à reposição.}

##### Tamanho da página

1. Tamanho da página = tamanho da memória (extremo)
    - Não ocorrem falhas de página.
    - Ao trocar um programa, é preciso recarregar toda a memória.
    - Muita fragmentação interna.
2. Tamanho da página = 1 byte (extremo)
    - Falhas de página frequentes
    - Precisa carregar dados da memória todas as vezes
    - Fragmentação externa.
3. Crar estatísticas para o tamanho da tabela de página
    - Usando simulações, é possível qual tamanho é otimizado.
    - Hoje, os tamanhos estão em torno de 4Kb a 8Kb.

##### Outros mecanismos de paginação
    
* Paginação em múltiplos níveis
    - Uma tabela de página por processo pode usar muita memória (páginas de 4k, 42 bits, 20 bits para número de página, 1 milhão de entradas).
    - Tabela pode ser dividida em tabela primária e várias tabelas secundárias (e terciárias, quartenárias, etc.)
    - Endereço dividido em 4 ou mais seções, cada uma usada para indexar uma das tabelas.
    - Mantidas apenas tabelas secundárias utilizadas pelo programa.
    ```
                Memória virtual                        Memória Real
            
    |    ID1    |    ID2    |   OFFSET   |              |       |
         11           9           12                    |       |
          |           '--.                              |       |
          |              |                              |       |
          |     |      | |                      .-----> |       |
          |     |      | |  .-->|      |        |       |       |
          |     |      | |  |   |      |        |       |       |
          |     |      | |  |   |      |        |       |       |
          |     |      | |  |   |      |        |       |       |
          '---> | P1EA |-:--'   |      |        |       |       |
                |      | '----->| P1E2 |--------'       |       |
                |      |        |      |                |       |
    ```
* Tabela de páginas invertida
    - Tabela flocal, cada entradadeve conter núemro da página virtual e id do processo.
    - Tabela indexada por número da página real
    - Para procurar endereço virual do Ev para processo PID, procurar entrada na tabela com este conteúdo.
    - Se estiver na entrada i da tabea, a página real é a i-ésima página.
    - Implementações reais usam tabela de hash para busca.
    - Arquitetura Itanium.

#### Esquemas de memória em processadores atuais: ARMv7

* Procesador RISC (Reduced instruction set)
* Memória paginada
* Corresponde BEM à necessidade de cada tipo diferente de programa.
* Dois níveis para estrutura da tabela de páginas
    - Tabela de primeiro nível (pro processo) que contém ponteiro para superseção, seção ou tabela secundária.
    - Superseções: blocos de memória de 16Mb (oddset de 24 bits).
    - Seções: blocos de memória de 1Mb (offset de 20 bits).
    - Páginas granes: 64bk (offset de 16 bits)
    - Páginas pequenas: 4kb (offset 12 bits)
* Estrutura flexível MMU (memory management unit)
    - Pode ser configurada para usar os vários tamanhos de página
      bem como seções e superseções.
    - Seções e superseções podem ser combinadas com páginas.
    - SO pode ou não usar.
* Seções e superseções.
    - Possibilidade de endereçar com apenas um nível de hierarquia com grandes proporções de memória (por exemplo, SO)
    - Uso com páginas introduz problemas comuns à segmentos de tamanho variável.
* TLB em dois níveis
    - microTLB
        - Menor e mais rápida
        - Uma para instruçes (32 entradas) e uma para dados (32 o 64 entradas)
        - TOtalmente associativa
        - LOokup em apenas um ciclo
        - Address Space Identifier (ASID) perminte entrada de vários processos na TLB.
        - Entradas globais (compartilhadas) e locais (por processo).
        - Páginas têm bits de proteção verificada a cada aesso.
        - Reposição round-robin (default) ou randômico.
    - main-TLB
        - Utilizada quando não há hit na micro-TLB.
        - apenas 1 por processador, usado para instruções e dados.
        - 4 entradas totalmente asociativas, que podem ser travadas (nda: SO deve travar também páginas na memória real)
        - 64 ou 128 entradas em estrutura 2-associativa (2x32 ou 2x64).
```        
     micro-TLB                               superseção |       |
      | ______                                          |       |
      |-|    |               Memória virtual    .--->   |       |
      |-|    |                  |       |       |       |       |
      |-|    |     main-TLB     |   *---:-------'       |       |
      |-|    |    | ______      |       | Tab.páginas   |       |
      '-|    |    |-|    |      |       |  |    |       |-------|
        ¨¨¨¨¨¨    |-|    |      |       |  | *--:------>|       |
                  |-|    |      |   *---:->|    |       |-------|
      | ______    |-|    |      |   *   |  |    |       |       |
      |-|    |    '-|    |      |   |   |               |-------|
      |-|    |      ¨¨¨¨¨¨          |        |    |   .>|       |
      |-|    |                      |        | *--:---' |-------|
      |-|    |                      '------->|    |     |       |
      '-|    |                               |    |     |       |
        ¨¨¨¨¨¨                             Tab. páginas |       |
```

#### Esquema de memória em processadores atuais: Intel IA-32
- Arquitetura segmentada
- 3 modos de endereçamento
    - Flat memory (lidear de 4 Gb)
    - Segmentado
        - Endereço: seletor de segmento + deslocamento
        - Endereço de memŕia de 16 bits (segmento) + 32 bits
          (deslocamento)
- Endereçamento segmento-paginado
- 2 tabelas de segmentos, cada uma contém endereços base de até
  8.191 segmentos.
    - Local descriptor tabel (LDT): segmentos privados ao processo.
    - Global descriptor tabel: segmentos compartilhados entre todos 
      os processo (ex: SO).
- Entrada na GDT contém
    - Sflag (código ou dados?)
    - Accessed (access bit)
    - Dirty bit
    - Data/write enable (read only ou read/write?)
    - Data/expansion direction (por default sefmentos no topo do
      segmento, vit muda direção da expansão)
    - COde/execute-onlu ou execute/read
    - Conforming (indica se execução pode continuar mesmo se
      nível de privilégio é elevado)
    - Detalhe da paginação: páginas de 4Kb ou 4Mb

#### Esquema de memória em processadores atuais: Intel IA-64

* Apenas paginação (segmentação tem suporte no modo emulação de IA-32 -> compatibilidade).
* Três modos de paginação
    - Paginação 32 bits
    - PAE (Physical Address Extension mode - IA-32)
    - Paginação IA-32e - 48 bits
* Paginação 32 bits (Páginas 4Kb ou 4Mb)
    - Páginas 4Kb:
        - Paginação em 2 níveis: 10 bits + 10 bits + 12 bits.
        - Tabela de páginas (nível 2) contém base de 20 bits.
 ```           
                | Directory | Page table |   Offset    |
                | (10 bits) | (10 bits)  |  (12 bits)  |
 ```       
    - Páginas de 4Mb:
        - Paginação em apenas 1 nível (10 bits + 22 bits).
    ```            
                | Page table |         Offset          |
                | (10 bits)  |        (12 bits)        |
    ```
* Paginação IA-32e
    - Endereço de 48 bits (256 terabyts) gera endereço real de 52 bits (4000 terabytes)
    - Endereços virtuais de 32 bits.
    - Gera endereços virtuais de até 52 bits.
    - Páginas de 4Kb, 2Mb, 1Gb.
    - Páginas de 4k:
        - Paginação em 4 níveis (9b+9b+9b+9b+12b).
        - Cada nível com 512 entradas.
        - Devolve endereços reais de 40 bits
    - Páginas de 2Mb:
        - Paginação em 3 níveis (9b+9b+9b+21b)
        - Devolve endereços reais de 31 bits
    - Páginas de 1Gb:
        - Paginação em 2 níveis (9b+9b+30b)
        - Devolve endereços reais de 48 bits
* PAE: Physical Address Extension
    - Emula PAE do IA-32
    - Endereços virtuais de 32 bits
    - Gera endereços virtuais de até 52 bits
    - Páginas de 2Mb ou 4Kb
    - Com páginas de 4Kb, tabela de páginas com 3 níveis:
        - 2 bits + 9 bits + 2 bits + 12 bits
        - Page director pointer tabela
        - Page directory
        - Page table
* TLBS
    - Conjudaa com outras  estruturas
    - Region reisters
    - Protection key registers
    - Virtual hash page table walker (VHPT)
    - Mecanismo de tradução da TLB
        - Endereço em  partes:
            - VRN (Virtual Region Number)
            - VPN (Virtual Page Number
            - Page offset
    - 4 unicades lóicas
        - ITLB (instruções)
        - DTLB (dados)
        - Cada uma tem:
            - translation caches (ITC, DTC)
                - Reposição determinada pelo hardware
                - Mínimo 1 entrada
            - translation reisters (ITR, DTR)
                - Reposição determinada pelo software
                - Mínimo 8 entradas

## Sistemas de arquivos

* Pode-se arumentar que a melhor maneira de armazenar informação no computador seria deixá-la toda no espaço de endereçamento
    - ex: 2^64 bytes: 2^32 bytes)
* Usando proteção e compartilhamento manteria-se toda a informação na memória virtual.
* Alguns segmentos poderiam guardar diretórios que garantiriam estrutura hierárquica da informação.
* Teríamos espaço de endereçamento padrão inicialiado pelo SO com todas as informações compartilhadas.
* MULTICS tentou essa abordagem (1964, no MIT o sistema rodou até 10/00)
* Esta abordagem, por sua complexidade não foi mais utilizada.
* Maioria dos sistemas tem uma hierarquia de memória.
    - Caches (memória dentro do computador)
    - Memória primária
    - Memória secundária (dispositivos)
* "Sistemas de arquivos" provê a organização da memória secundária. Geralmente persistente e unico para uma instalaçã de um SO.
* Funções dos sistemas de arquivo
    - Criação, eliminação e modificação de arquivos
    - Compartilhamento de arquivos
    - Controle de acesso
    - Backup e recuperação de informação
    - Acesso simbólico (por nomes) aos arquivos
    - Prover visão lóica (não física) dos arquivos: independência de
      dispositivo
    - Segurança (criptografia)
    - Integridade de arquivos

### Organização de arquivos

* Blocos
    - Registros (comum em mainframes, principalmente os antigos)
    - Bytes (Unix, Minix)
* Métodos de acesso
    - Sequencial (Unix/Minix - character)
    - Indexado
    - Aleatório (Unix/Minix - block)
        - A leitura é igual à outra, mas é possível mudar a posição inicial de leitura.

#### Organização interna de arquivos

* Unix block:
    - Sequência de bocos mas endereçado por bytes
    - read/write/seek
* QSAM (Queued Sequencial Access Method)
    - Arquivos organizados por blocos físicos
    - Acesso do usuário por registro lógico definido pelo usuário
* ISAM (Indexed Sequencial Access Method)
    - Organização em árvore: blocos com número fixo de registros
    - Apontadores de outros blocos entre registros
    - Acesso é sequencial pelo índice
    - Esses métodos sumiram, aos poucos, porque guardar as informações dessa maneira foi passado para os sistema gerenciadores de bancos de dados.
* Outros métodos (IBM): XDAP, BDAM, BSAM, BPAM, VSAM, OAM
    - Em sistemas derivados dos computadores pessoais, em geral acesso aleatório e organização por blocos.

### Tipos de arquivos

* Regulares
    - Arquivos de caracter (ASCII, ...)
        - Linhas terminadas por "line feed" ou "carriage return" ou ambos.
        - Linhas de tamanho variável
        - Arquivos ASCII úteis porque codificação padrão de informação, facilitam uso de "pipe" para conectar saída de um arquivo com entrada de outro.
    - Arquivos binários
        - Código: todos os SO's precisam reconhecer arquivos formatados para códio executável (TOPS-2 - DEC - inclusive verifica se código estava em dia com o "fonte" recompilava se necessário.
            - Campos:
                - Header: Número mágico (descreve arquivo executável), tamanho da área de texto, tamanho da área de dados, tamanho do BSS, tamanho da tabela de símbolos, ponto de entrada, flas.
                - Texto, dados, bits de realocação (para colocar o  prorama na memória), tabela de símbolos (depuração).
                - Archive (Unix)
                    - Coleção de módulos de bibloteca, compilados mas não linkados.
                    - Cada módulo tem entradas com:
                        - header com seu nome, data de criação, dono, códio de proteção e tamanho.
                        - "object module"
                - Arquivos "fortemente tipados" podem ser problema
                    - Ex: pré-processador de C (C++) que gera como resultado programa em C.
                    - Deveria gerar um .c, mas só podia gerar .dat.
            - Os scrips shell podem ser rodados porque o símbolo shbang (#!) é convertida para um número binário que indica que o arquivo a seguir é, na verdade, um arquivo de dados que devem ser passados para o programa binário indicado pelo path seguinte.

* Atributos comuns em arquivos
    - Proteção
    - Senha
    - Criador
    - Dono
    - Fla de eitura/escrita
    - Flag de "hidden file"
    - System flag
    - Último acesso
    - Última alteração

### Implementaão de sistemas de arquivos

* Tabela (FAT)
    - Alguns blocos guardam indexador, que é carregado na memória.
    - Uma entrada para cada bloco da partição do disco
    - Entrada no diretório contém número do primeiro bloco
    - Cada entrada na FTA contém índice do bloco do arquivo
    - Como tabelaé residente na memória, acesso aleatório é rápido
    - Problemas tamanhos:
        - FAT-1, FAT-16, FAT-32 (máximo 4 blocos)
* Cada arquivo tem um reistro que contém ponteiros para os primeiros blocos e descrição do arquivo (veja em atributos)
* Arquivos maiores usam esquema de acesso indireto
    - Sinle indirect
    - Double indirect
    - Triple indirect
* Pouco overhead para arquivos pequenos
* Acessos indiretos para arquivos Pequenos
* Acessos indiretor envolvem novas leituras do disco
* Acesso totalmente aleatório de arquivos grandes é lento: princípio da localidade ameniza problema.
* Tamanho de arquivo MUITO escalável:
    - 32 bits endereçam 4 * 10^9 blocos
        - Bloco 4k, 16Tb disco, arquivo 4Tb
        - Bloco 8k, 32Tb disco, arquivo 32Tb

#### Implementação de diretórios

* CP/M
    - Primeiro SO para computadores pessoais de 8 bits
    - Sistema para computadores e 8 bits
    - 1 diretório para todos os arquivos
    - Entrada de tamanho padrão
        - Códio usuário (1 byte)
        - Nome (8 bytes)
        - Extensão (3 bytes)
        - Extent (1 byte) - para mais de uma entrada para o mesmoarquivo, diz numero da entrada.
        - Livre (2 bytes)
        - Blocos em uso (1 byte)
        - Apontadores para disco (16 bytes)
        - Arquivos grandes repetem entradas.
```
    U   NOME   EXT E FR B  Blocos no disp
   | |        |   | |  | |                |
```
* DOS (precursor do Windows)
    - Originalmente para sistemas de 6 bits, disco de 10 Mb
    - Diretório raiz é fixo (11 entradas no floppy)
    - Outros são arquivos normais
    - Estrutura hierárquica de diretórios
    - Sem usuário
    - Entrada tamanho padrão
        - Nome (8 bytes)
        - Extensão (3 bytes)
        - Atributos (1 byte)
        - Livre (2 bytes)
        - Primeiro bloco na FAT (2 bytes)
        - Tamanho (4 bytes)
```                   
    NOME   EXT A FR FT SIZ
 |        |   | |  |  |   |
```
* Windows 95
    - 2 tipos de entrada (para compatibilidade)
    - Entrada tamanho padrão
        - Nome (8 bytes)
        - Extensão (3 bytes)
        - Atributos (1 byte)
        - Campo NT (1 byte) - para múltiplas entradas
        - Sec (1 byte)
        - 16 bits superiores do primeiro bloco (2 bytes)
        - Data e horário da última escrita (4 bytes)
        - 16 bits inferiores do primeiro bloco (2 bytes)
        - Tamanho (4 bytes)
```                    
    NOME   EXT A N S     HIGH FAT      TS      LOW FAT     SIZE
 |        |   | | | |                |    |               |    |
```    
* Unix
    - Tabela de i-nodes em local fixo na partição (local no superbloco)
    - Localização do i-node a partir do número é trivial
    - Diretórios são todos arquivos normais
    - I-nore para diretório raiz na primeira entrada
    - I-nore contém infromação sobre o arquivo, não o diretório
    - Entrada
        - Número do I-node (2 bytes)
        - Nome (16 bytes)
* NTFS (Windows 4 000 e depois)
    - Em Unix, arquivo é conjunto de bytes
    - Nos sistemas windos modernos, conjunto de atributos

### Administração de espaço

* Questão importante é como o espaço em disco é administrado
    - Sequência contínua de bytes
    - Sequência de blocos (não necessariamente contínuos)
* Tamanho do bloco
    - Candidatos
       - Setor, trilha, cilindro
        - Desperdício de espaço vs. taxa de transderência de dados (Latência de movimento de braço + rotação + taxa de transferência de cada bloco)
* Tamanho dos arquivos
    - Unix (2005): mediana do tamanho dos programas é 2k
    - Windows em sistemas científicos (Cornell), idem

* Espaço livre
    - Blocos com listas liadas de clovos livres
        - Última entrada é próximo bloco da lista
        - Apenas 1 bloco na memória de cada vez
        - Simples implementação
        - DIsco de 1Tb, bits para número do setor, setor de 1kb, lista
          livre ocupa até 4Gb
    - Vetor de bits
        - 1 bit por bloco do disco
        - Tamanho fixo
        - Muito compacto, 1 bit por bloco
            - Bloco de 1k contém indexação de 8 mil blocos (8Mb)
            - Disco de 8Tb indexado com 125Mb de lista
        - Implementação de busca mais complexa.


### Compartilhamento de arquivos

* Quando usuários diferentes compartilham arquivo, é conveniente se o arquivo aparece simultaneamente em diretórios diferentes
* Se é possível acessar arquivos por caminhos diferentes,  estrutura de diretórios-vira DAG (não mais árvore)
* 2 maneiras
    - Hard links: entradas em diretórios distintos compartilham descritor do arquivo
        - Assim lista de blocos não faz parte da entrada d diretório
            - UNIX implementa dessa maneira
            - Muito mais difícil
        - Problemas:
            - Remoção de arquivo deve cuidar para que arquivo real seja removido apenas quando não há Mais "links" (I-node tem contador de links)
            - Usuário original continuará sendo cobrado pelo espaço
            - Arquivo removido pelo dono continua sendo usado
            - Ciclos
    - Soft links: arquivo que aponta para a localização do original
        - Problemas:
            - Sobrecara de acesso: apenas na abertura
            - Entradas órfãs de arquivos eliminados
    - Back-ups devem cuidar para não duplicar trabalho e reconstituição deve manter unidade.

### Confiabilidade de sistemas de arquivos

- Arquivos são mais valiosos que o hardware
- Localização dos setores ruins do disco
    - Hardware: setor do disco contém lista dos blocos ruins com lista dos blocos substitutos. Controlador usa substitutos automaticamente.
    - Software: sistema de arquivos contrói arquivo contendo os blocos ruins.
- Atualização atômica
    - Garantia que atualização parcial nunca acontece, mantendo arquivos sempre consistentes.
    - Se atualização falhar antes de completar, basta rodar novamente.
    - Discos com atualização atômica tolerante a falhas são implementados usando 2 ou mais discos físicas ou uma partição lógica.
- RAID
    - Esquemas para operação de vários discos como se fosse apenas 1, criando um "disco virtual"
    - Original "RAID5" - 1978
    - Implementação por hardware (placa) ou software    
    - **RAID0**
        - 2 ou mais discos se juntam para criar um disco maior,
          sem redundância ou recursos de recuperação.
        - Aumento de capacidade de um sistema de arquivos.
        - Leitura e escrita mais rápidos.
    - **RAID1**
        - Disco espelho
        - Copias os blocos do primeiro disco no segundo
        - Falha simples recuperada
            - Uma das cópias de setor com paridade ruim
            - Um disco falha
        - Leitura mais rápida (também pode fazer leitura em paralelo)
    - **RAID5**
        - "stripping" de vlocos com paridade distrivuída
        - Conteúdo de bloco ausente pode ser recuperado com blocos restantes
        - Recupera falha em qualquer disco (mínimo 3 discos)
        - Leitura e escrita mais rápidas: ((n-1)X)
        - Performance com falha é reduzida
        - Disco com falha pode ser reconstituído
    - **RAID6**
        - Semelhante a RAID5
        - Paridade dupla distribuída (min. 4 discos)
        - Até duas falhas
        - Leitura e escrita mais rápida ((n-2)X)
    - Outros RAIDS
        - Bit stripping, byte stripping (RAID2, RAID3)
        - RAIDS compostos ("nested"), (RAID 10 (1 e 0 em nível), etc.)
- Controle de concorrência
    - Unix tradicional: requisição de leituras e escritas executadas na ordem em que cheam
    - Pode gerar problemas quando exclusão mútua é necessária
    - Solução mais comum são travas ("locks)
        - Em Unix, 1 por arquivo
    - Apresenta os mesmos problemas que exclusão mútua
    - Difença importante: enfase nos dados (ao invés de código)
    - Os SGBDs foram feitos justamente para facilitar o controle de concorrência e automatizá-lo nos softwares.
* Transações
    - Travamento automático + atualização atômica
        - Formato
        ```
            begin transaction
            atualizações
            end transaction
        ```
        - Nenhuma modificação acontece até o end "transaction"
* Outras questões
    - Unix pode ter "buracos" nos arquivos
        - "open, write, seek, write".
        - Core dumps têm espaço entre códio e pilha.
        - Não queremos "buracos" preenchidos na recuperação.
    - Cuidado com links para evitar duplicação (e loops)


#### Backup

* Dois objetivos
    1. Recuperação de desastres
    2. Recuperação de erros
* Potencialmente lentos e alto uso de espaço
* O que recuperar?
    - Dump Físico vs Dump Lógico
* Dump físico
    - Disco inteiro é copiado (simples, mas demorado)
    - Cuidado com blocos inválidos
        - Se mantidos pelo hardware, ok
        - Se mantidos pelo SO, programa de backu deve ter acesso a estruturas e evitar copiá-los.
* Dump lógico
    - Muitos sistemas não fazem backup de executáveis, arquivos temporários, arquivos especiais (/dev/ é até perigoso)
    - Em geral, é interessante especificar diretórios a serem uardados.
    - COmeça em um ou mais diretórios especificados e recursivamente percorre a estrutura de diretórios salvando os intens encontrados.
    - Não copiar arquivos especiais (pipes, etc.)
* Dumps incrementais
    - não tem sentido fazer novo backup de arquivos não mudados.
    - Dump completo + incrementos (ex: Mês + dia).
    - Complica recuperação.
    - Mesmo diretórios não modificados podem ser encontrados.


### Consistência do sistema de arquivos

* Sistemas de arquivos lêem/criam blocos, podem modificá-los e depois são salvos.
* E se o programa morre antes dos blocos serem salvos?
    - Mais grave é o i-node
* Maioria dos SOs têm programas para verificar consistência do sistema de arquivos.
    - Unix: fsck
    - Windows: chfdisk
* fsck
    - Consistência de blocos
        - Duas tabelas com contadores para cada bloco, inicializados com zero.
            - Quantas vezes um bloco está presente em um arquivo
            - Quantas vezes um bloco está presente na lista livre
        - Prorama lê todos os i-nodes e percorre lista de blocos
            - Toda vez que bloco é encontrado, atualiza primeira tabela
        - Programa percorre lista livre
            - Toda vez que bloco é encontrado atualiza seunda tabela
        - BLocos bons: (1,0) ou (0,1)
        - Missing block: (0,0), adicionados à lista livre
        - Bloco com mais de uma ocorrência em lista livre (0,n)
          - reconstrói a lista livre
        - Blocos presentes em mais de um arquivo (m,0)
            - Copia em dois blocos, associando cada um a um arquivo
        - Blocos presentes em mais de um arquivo e lista livre(m,n)
            - Tira da lista livre e copia como no (m,0)
    - Consistência de arquivos-e diretórios
        - Tabela de contadores, um por arquivo
        - Percorre árvore de diretórios
            - Incrementa contador toda vez que um inode é encontrado
    - Outras ocorrências suspeitas que podem ser reportadas
        - Diretórios com muitos arquivos (ex: mais de mil)
        - Permissões estranhas (ex: 0007)
        - Arquivos em diretório de usuário, mas pertencentes ao root e com setuid ligado

### Performance do sistema de arquivos

* Blocos sempre carregados na área de cache antes de lidos
* Tabela de hash (dispositivo + número do bloco) e lista ligada.
* Reposição semelhante aos algoritmos de memória virtual.
    - Como chaces-Lidam com menos frequência, (e sempre chamada de sistemas), é viável isar LRU.
    - Hash + lista livre (quando bloco usado para fim)
* Considerações adicionais:
    - Blocos mais antigos em geral são modiifcados.
    - i-nodes são importantes para consistência do sistema (como a cache está na memória, se houver falhas pode haver inconsistências por causa da perda. Modificações deveriam ser gravadas imediatamente).
* Blocos devem ser divididos em categorias (i-nodes, blocos indiretos, doretórios, blocos de dados completos, blocos de dados parciais)
    - Blocos que não devem ser usados quando vão na frente, outros no final (como blocos que são arquivos abertos para escrita, mas não fechados).
* No Windows, o FAT era write-throug - tudo era imediatamente guardado. Menos eficiente, mas mais seguro.
* Os caches podem fazer leitura preventiva de blocos, para tentar "prever" o que será lido (porque ela costuma ser lida sequencialmente).
* Reduzindo movimentos do braço do disco
    - Colocar blocos que podem ser utilizados sequencialente próximos um do outro, de preferência no mesmo cilindro.
    - Ex: se a lista livre for usada como bitmap, podemos pegar o mais próximo.

### Sistema de arquivos estruturados como logs

* Log strutured file systens - LSU (Berkeley)
    - Aumento da diferença de performance motivou aumento das caches.
    - Enorme aumento das memórias.
    - Caches devem conter porcentagem cada vez maiores de acessos
* Sistemas de arquivos são estruturados como buffers circulares.
* Sistemas de arquicos organizados como buffers circulares.
* Periodicamente, (ou quando necessário), todas as escritas pendentes são coletadas em um segmento que é escrito de uma só vez e de maneira contínua.
* Assim, as atualizações do segmento são mais frequentes, mas só alterando o que foi feito.



### CACHES

* Blocos sempre carregados na área de  cache antes de serem lidos.
* Tabela de hash (dispositivo + número do bloco) e lista ligada.
* Repositório semelhante aos algoritmos de memória virtual.
    - Como chaces lidas com menos frequência (e sempre via chamada de sistema) e viável o uso de LRU.
    - Hash + lista livre (quando bloco usado vai para o fim).
* Considerações adicionais
    - Blocos mais antigos em geral são modificados
    - i-nodes são importantes para a consistência do sistema, e deveriam ser mantidos.
* Blocos devem ser divididos em categorias (i-nodes, blocos indiretos, diretórios, blocos de dados completos, blocos de dados parciais).
* Blocos que são essenciais para o sistema devem ser implementados via write-through.
* Mesmo assim, não devemos deixar blocos modificados sem serem escritos por muito tempo.
    - UNIX: syncs periódicos (0 segundos)
    - Windows: write-though cache (i.e USB e FAT são seguros).
* Leitura preventiva de blocos
    - Tentar colocar blocos na cache antes de sua leitura/escrita ser requisitada.
    - Ex: maioria dos arquivos lidos sequencialmente
        - FS pode verm, quando bloco K é requisitado, se o bloco K+1 está na cache, e carrá-lo em seguida (funciona por causa do princípio da localidade).
        - Esse sistema costuma ser desativado quando o programa faz seeks (acesso aleatório).
* Reduzindo movimento do braço do disco
    - Colocar blocos que odem ser utilizados sequencialmente próximos um do outro, de preferência no mesmo cilindro, pois em geral a leitura exige carregar o i-node e, em seguida, já ler o bloco.
    - Variação: i-nodes no meio do disco.
* Sistemas de arquicos estruturados como log
    - O sistema avalia um conjunto de modificações e os coloca diretamente nos semgentos do disco, escrevendo uma série de modificações ao mesmo tempo no log.
    - O processo aumenta o fluxo do disco, usando-o quase 100% do tempo.
    - Uma vez encontrar i-node, o acesso é feito do modo usual
    - Os i-nodes estão espalhados no disco, e o sistema utiliza uma cache na memória para eles.
    - Abertura de arquivos agora exige localizar o segmento do i-node.
        - Neste esquema, segmentos podem conter blocos obsoletos.
        - Após falha, o sistem de log recupera a partir do último checkpoint.

### Segurança

* Segurança vs proteção
    - Segurança é o problema geral.
    - Proteção são os mecanismos para garantir a informação no sistema.
    - Objetivos
        - Confencialidade dos dados: Sistema deve garantir que dados de usuário só serão conhecidos com sua permissão.
        - Integridade dos dados: Usuários não autorizados não devem poder modificar dados sem permissão (inclui remoção e adição de dados).
        - Disponibilidade: Ninguém deve conseguir pertbar o sistema a ponto de tornar acesso aos dados impossível (denial of service).
        - Privacidade: Proteger usuários do uso indevido de informações a seu respeito (padrões de uso, etc).

#### Intrusos

* Usuários não autorizados no sistema
* Passivos vs Ativos: apenas olham os dados ou tentam modificá-los.
    - 4 tipos de intruso:
        - Curiosos ocasionais: pessoas com pouco conhecimento que xeretam arquivos não protegidos (e.g em Unix é comum arquicos serem criados com permissão de leitura universal).
        - "xeretas descolados": pessoas que são usuários do sistema que consideram um desafio pessoal conseguir quebrar a sgurança da informação. Em geral, são muito habilidosos e dispostos a usar muito tempo na tarefa.
        - Estelionatários: programadores em bancos. Ameaças incluem truncar (e não arredondar) transações, uso de fundos d contas inativas, chantagem.
        - Espionagem comercial ou militar: esforços sérios e muito bem financiados para roubar programas, informações, tecnologia, etc. Iniciativas podem envolver escutas de linhas de transmissão, antenas para detectar sinais eletromagnéticos do computador.
* Esforço dedicado à segurança deve ser proporcional à qualidade
          do possível intruso e do potencial dano a ser infringido.

#### Programas maliciosos

* Vírus: pedaço de códgo que pode se reproduzir ntroduzindo uma cópia de-si mesmo em outros programas e causar vários danos.
    - Destruição de informação
    - Envio de informação privada
    - Inserção de informação
    - 'Denial of service': consome recursos do sitema a ponto de torná-lo inutilizável (CPU, disco, canais de comunicação, etc).
    - DDOS (Distributed Denial of service): vírus tem duas fases: reprodução, onde se espalha pela rede, e ataque. Em data pré-determinada, todas as cópias "acordam" e iniciam ataque (ex: Webpage específica).
* Key lgger: monitora teclato procurando padrões (ex: conta de e-mail seguida de sequência de caracteres).
* Warm: diferente do vírus, que está incluído em outro programa ou arquivo, é um programa independente.
    - Worm pode se colocar no "startup directory".
* Cavlo de Tróia: programa que desempenha uma função, mas que esconde funcionalidade secundária (lançar um worm, inserir vírus, bisbilhotar sistema de arquivos, etc). Em geral, baixados voluntariamente.
* Bomba lógica: programa elaborado por programador do sistema que exgem senha periódica. Se não receber a senha, a bomba é "detonada". As ações podem envolver todo o tipo de dano como remoção de arquivos (aleatórios ou não), encriptação de arquivos, mudanças difíceis de identificar, etc.
* Spyware: obtidos visitando-se sites na Web - instalando-se vendo cookies ou por plugins no navegador.

#### Um pouco de história

* Multics (cavalo de Tróia)
    - Não havia nenhuma segurança para processamento BATCH (sistema foi feito para usuários interativos, processamento BATCH adicionado depois).
    - Qualquer um podia submeter prgramas batch que lia conjunto de cartões (sim, cartões de computador) e copiava seu conteúdo no diretório De QUALQUER usuário.
    - Para roubar arquicos de um usuário era só modificar o fonte do editor de textos para copiar as informações do outro usuário para cartões.
* TENEX (popular nos sistemas DEC-10)
    - Um sistema com memória paginada, que analisava as senhas um caracter por vez (dando erro se o caracter estivesse errado).
    - Para descobrir a senha, programas colocavam a senha com o caracter a ser descoberto em uma página à frente do restante. Para descobrir, bastaria ver se havia erro ou falha de página (caracter correto).
    - Com uma senha de tamanho N, era possível descobrir em N * 128 vezes (e não 128^N).
* OS360
    - Era possível começar uma transferência de Dados de uma fita e continuar computando em paralelo.
    - O truque era emitr uma chamada ao sistema e esperar que o sistema de arquivo carregasse. Então, bastava substituir o endereço a ser procurado por algum outro path, e então acessá-lo.
* UNIX
   - Arquicos de senhas tinham leitura pública. Algoritmo 'one way' (usado para encriptar senhas). Usuários podiam olhar arquivos e testar listas de senhas prováveis aplicando algoritmo e vendo se codificação da senha batia (nomes, dicionários, datas...)
    - Lpr tem uma opção para remoção do arquivo após sua impressão. Versões iniciais do Unix permitiam que qualquer um imprimisse e depois removesse arquivo de senhas.
    - Usuário podea linkar arquivo com nome "core" no seu diretório ao arquivo de senhas. O introso então forçava um "core dump" de um programa com SETUID. O sistema escrevia o dump no arquivo "core", substituindo o arquivo de senhas do sistema.
    - Mkdir foo: mkdir é um programa com SETUID cujo dono é root. Mkdir cria primeiro cria o i-node para 'foo',  e após isso muda o "dono" do arquivo do UID efetivo (root) para o real (usuário). Se o sistema fosse lento era possível ao usuário remover o inode de 'foo' após o mknod, mas antes do chown. Então, trocando o arquivo por outro (como o de senhas), que depois era entregue ao usuário.
* 1988 - Robert T. Morris (Cornell) - warm com 2 programas: o comando de bootstrap e o warm em si.
    - Programa de bootstrap tinha 99 linhas em C, compilado e executado no sistema sob ataque.
    - Uma vez rodando, conectava-se à máquina de origem e carregava o warm principal, executando-o.
    - Após algumas etapas de camuflagem consultava tabelas de rastreamento de seu hospedeiro e tentava espalhar o comando bootstrap nelas.
    - Esse programa derrubou o sistema de muitas universidades dos EUA.

#### Ataques

* Todo SO tem lacunas. Maneira normal de se testar é uso de times de invasão (peritos contratados ou desafio).
* Ataques mais comuns
    1. Requisitar páginas de memória, espaço em disco e simplesmente investigar o conteúdo: muitos SOs não apagam o conteúdo - apenas reutilizam.
    2. Tentar chaadas de sistmas ilegais, ou chamadas legais com parâmetros estranhos.
    3. Faça login e clique DEL. O computador pode matar o programa e, então, relogar.
    
#### Princípios de segurança

* Design de um sistema deve ser público:
    - Supor que o intruso não vai saber como funciona o sistema é uma ilusão (pior ainda atualmente, com a web).
    - A ação padrão deve ser sempre negar o acess. Erros no quais um acesso autorizado é negao são relatados mais rapidamente do que casos de permissão de acesso não autorizado.
    - Faça verificação contínua de permissões.
    - A cada processo deve ser dado o mínimo possível de privilégio.
    - Mecanismo de proteção deve ser simples, uniforme e implantado o mais internamente no sistema.
    - Mecanismos devem ser de fácil aceitação: usuários não  adotam senhas muito complexas.

#### Senhas

* Facil de quebrar pois difícil forçar usuário a escolher boas senhas.
* Bloqueio de senhas fáceis.
* Substituição periódica obrigtória.
* Variação: perguntas e respostas armazenadas no sistema.
* Variação: resposta a desafio - algoritmos pré-escolhidos (ex: x^2), computador faz perguntas periódicas e espera resposta da aplicação do algoritmo.
* Identificação física
    - Digitais
    - Comprimento de dedos
    - Identificação de retina
* Contramedidas
    - Delays no processo de lgin dificultam tentativas exaustivas. Delays crescentes melhor ainda.
    - Registro de todos os logins: ataques podem ser detectados por logins falhos sucessivos de alguma fonte.
    - Armadilhas:
        - login fácil com senha fácil (silva, 1111): login nestas contas alertam administradores que podem monitorar uso.
        - Bugs fáceis de serem achados.

#### Mecanismos de proteção

* Domínio = (objeto, direitos de acesso)
* Objeto: entidade do SO (arquivo, etc.)
* Direitos: permissões para efetuar cada oepração relativa ao objeto.
    - A cada momento processo executa em um domínio de proteção
        - Há coleções-de objetos que ele pode acessar, cada um com conjunto de direitos.
        - Domínio pode ser mudado.