# Questionário Sistemas Embarcados I

```Make
Aluno: Marcellus Mendonça Barbosa da Silva
Matrícula: 12111EAU022
```

## 1. Explique brevemente o que é compilação cruzada (***cross-compiling***) e para que ela serve.

A compilação cruzada é um processo no qual se cria código executável para uma plataforma diferente daquela em que o compilador está sendo executado. Essa diferença de plataforma pode envolver outro sistema operacional ou outra arquitetura. Por exemplo, é possível compilar para Windows a partir do Linux ou para Arm64 a partir do x64.
Isso é comum quando se desenvolve software para dispositivos embarcados, sistemas operacionais diferentes ou arquiteturas específicas.
No contexto de sistemas embarcados, é possível executar um código em um microcontrolador, como o STM32F411, por exemplo, gerado em um ambiente com arquitetura Windows.

## 2. O que é um código de inicialização ou ***startup*** e qual sua finalidade?

O código de inicialização é uma parte essencial do firmware que é executada quando o microcontrolador é ligado ou reiniciado.
Ele é responsável por configurar o ambiente inicial, preparando o microcontrolador para a execução do código do usuário.
O código de inicialização tem várias tarefas importantes:

- Configuração do Hardware:
	- Inicializa os periféricos integrados no microcontrolador, como GPIOs, timers, ADCs, UARTs, etc.
	- Define as configurações iniciais, como taxas de clock, modos de operação e interrupções.

- Configuração da Memória:
	- Configura a memória RAM e memória flash.
	- Copia os vetores de exceção (que lidam com interrupções e exceções) para a memória RAM.

- Chamada do Ponto de Entrada Principal:
	- Após a configuração, o código de inicialização chama a função `main()`, que é o ponto de entrada para o código do usuário.

- Chamada do Ponto de Entrada Principal:
	- Após a configuração, o código de inicialização chama a função `main()`, que é o ponto de entrada para o código do usuário.

- Tratamento de Exceções:
	- Configura os vetores de exceção para lidar com interrupções, erros e exceções.

- Inicialização do Stack Pointer e do Link Register:
	- Define os valores iniciais do ponteiro de pilha (SP) e do registrador de link (LR).

- Configuração do Relógio:
	- Define a frequência do clock do sistema (HCLK) e dos periféricos (PCLK).

- Configuração do Modo de Operação:
	- Define o modo de operação do microcontrolador (modo normal, sleep, standby, etc.).

O código de inicialização geralmente fica armazenado em uma área específica da memória flash, conhecida como vetor de inicialização.

O endereço desse vetor é definido no arquivo de linker durante o processo de compilação.
No caso do STM32F411, o código de inicialização é fornecido pela biblioteca CMSIS (Cortex Microcontroller Software Interface Standard).

A função SystemInit() é chamada no início do programa e configura os periféricos e o relógio.
Em seguida, a função main() é chamada para iniciar a execução do código do usuário.


## 3. Sobre o utilitário **make** e o arquivo **Makefile responda**:

#### (a) Explique com suas palavras o que é e para que serve o **Makefile**.

Um Makefile é um arquivo de script usado pelo utilitário `make` em sistemas operacionais baseados em Unix. Ele contém um conjunto de diretivas e regras que descrevem como construir um programa ou um conjunto de programas.

O Makefile possui várias vantagens e é utilizado em situações como:

1. **Automatização da compilação:** O Makefile automatiza o processo de compilação, permitindo que você compile todo o projeto com um único comando, `make`, em vez de compilar cada arquivo de código fonte individualmente.
2. **Gerenciamento de dependências:** O Makefile rastreia as dependências entre os arquivos do projeto. Se um arquivo de código fonte for modificado, `make` recompilará apenas esse arquivo e os arquivos que dependem dele, em vez de recompilar todo o projeto.
3. **Se deseja eficiência:** Como o `make` recompila apenas os arquivos que foram alterados desde a última compilação, ele pode economizar muito tempo, especialmente em projetos grandes.

#### (b) Descreva brevemente o processo realizado pelo utilitário **make** para compilar um programa.

O `make` lê o Makefile, determina quais alvos precisam ser atualizados com base nas dependências, executa as regras para esses alvos e repete o processo para todos os alvos necessários. Ele faz toda essa ação utilizando as regras, diretivas e variáveis contidas no Makefile.

#### (c) Qual é a sintaxe utilizada para criar um novo **target**?

O `target` é o nome do alvo que você está criando. Pode ser o nome de um arquivo que é gerado, ou um nome para uma ação específica como `clean`, por exemplo. Para isso ele utiliza as `dependencias` que são os arquivos que o alvo depende. Se qualquer um desses arquivos for mais recente que o alvo, então os comandos serão executados. Além disso são utilizado os `commandos`, que são os comandos que o make deve executar para atualizar o alvo. Cada comando deve ser precedido por um caractere de tabulação. Sua sintaxe completa fica da seguinte forma:

```make
target: dependencies
    commands
```

#### (d) Como são definidas as dependências de um **target**, para que elas são utilizadas?

As dependências de um alvo (target) em um Makefile são definidas logo após o nome do alvo, separadas por dois pontos.

As dependências são arquivos dos quais o alvo depende. Podem ser arquivos de código fonte ou outros alvos no Makefile. Se qualquer uma das dependências for mais recente que o alvo, o `make` executará os comandos para atualizar o alvo.

Elas são utilizadas para duas principais finalidades:

1. Determinar se um alvo precisa ser atualizado: Se uma dependência foi modificada desde a última vez que o alvo foi construído, então o alvo precisa ser atualizado. O `make` verifica a data e hora da última modificação dos arquivos para fazer essa determinação.
2. Determinar a ordem de construção dos alvos: Se um alvo depende de outro alvo, o `make` construirá o alvo dependente primeiro. Isso garante que todas as dependências estejam atualizadas antes de um alvo ser construído.

Por exemplo, se você tem um alvo para vincular um programa que depende de vários arquivos de objeto, e cada arquivo de objeto é construído a partir de um arquivo de código fonte correspondente, você pode definir isso no Makefile. Se qualquer arquivo de código fonte for modificado, o make recompilará o arquivo de objeto correspondente e vinculará o programa.

A sintaxe fica da seguinte forma:

```C
target: dependency1 dependency2 ...
    commands
```



#### (e) O que são as regras do **Makefile**, qual a diferença entre regras implícitas e explícitas?

As regras do Makefile são instruções que especificam como criar ou refazer um ou mais arquivos.

- **Regras explícitas:** informam ao make quais arquivos dependem de outros arquivos, bem como os comandos necessários para compilar um determinado arquivo. Por exemplo, se você tem um arquivo de código-fonte C e quer compilar para um arquivo objeto, você pode escrever uma regra explícita para isso.

- **Regras implícitas:** são semelhantes às regras explícitas, mas com a diferença de que elas indicam os comandos a serem executados, mas o make usa as extensões dos arquivos para determinar quais comandos executar. Por exemplo, o make sabe que arquivos .c devem ser compilados com o compilador C para gerar arquivos .o. Portanto, se você tem um arquivo main.c e precisa de um arquivo main.o, o make pode usar uma regra implícita para compilar main.c mesmo se não houver uma regra explícita para isso no Makefile.

## 4. Sobre a arquitetura **ARM Cortex-M** responda:

### (a) Explique o conjunto de instruções ***Thumb*** e suas principais vantagens na arquitetura ARM. Como o conjunto de instruções ***Thumb*** opera em conjunto com o conjunto de instruções ARM?

As instruções Thumb são um subconjunto do conjunto principal ARM e têm instruções de 16 bits, em contraste com as instruções ARM padrão que são de 32 bits. Isso resulta em um código mais compacto, o que é uma grande vantagem em sistemas com memória limitada. Além disso, as instruções Thumb podem ser executadas mais rapidamente em alguns sistemas, especialmente aqueles com barramentos de dados de 16 bits para o flash interno.

Na execução, as instruções Thumb de 16 bits são descomprimidas de forma transparente até instruções ARM completas de 32 bits em tempo real, sem perda de desempenho. Isso significa que o código Thumb é tipicamente 65% do tamanho do código ARM, e fornece até 160% do desempenho do código ARM de 32 bits. Isso é feito por meio de um processo chamado “decodificação dinâmica”, que permite que o processador ARM execute tanto instruções ARM quanto Thumb.

Essa capacidade de alternar entre os conjuntos de instruções ARM e Thumb é conhecida como “intertrabalho” e é uma característica chave da arquitetura ARM. O intertrabalho permite que o código ARM e Thumb coexista no mesmo programa, proporcionando uma flexibilidade significativa para os desenvolvedores. Eles podem escolher usar instruções ARM para partes do código que requerem a funcionalidade completa do conjunto de instruções ARM e instruções Thumb para partes do código onde a densidade de código é mais importante.

Embora as instruções Thumb sejam mais limitadas em capacidade do que as instruções ARM, se o seu código precisar apenas da funcionalidade das instruções Thumb, ele ocupará menos espaço que o ARM, mas terá o mesmo número de instruções e, outras coisas sendo iguais, será executado na mesma velocidade.


### (b) Explique as diferenças entre as arquiteturas ***ARM Load/Store*** e ***Register/Register***.

- **Arquitetura Load/Store:** Esta arquitetura é comumente associada a processadores RISC (Reduced Instruction Set Computing), como o ARM. Nesta arquitetura, as operações (como adição, subtração, etc.) só podem ser realizadas em dados que estão nos registradores. Portanto, os dados devem ser carregados da memória para os registradores antes que uma operação possa ser realizada. Após a operação, os resultados são armazenados de volta na memória. Isso resulta em um conjunto de instruções mais simples, o que pode levar a um desempenho mais eficiente.

- **Arquitetura Register/Register (ou Register-to-Register):** Esta arquitetura é comumente associada a processadores CISC (Complex Instruction Set Computing). Nesta arquitetura, as operações podem ser realizadas diretamente na memória ou entre registradores, sem a necessidade de carregar explicitamente os dados nos registradores antes de realizar uma operação. Isso resulta em um conjunto de instruções mais complexo, mas pode reduzir o número de instruções necessárias para realizar uma tarefa.

A principal diferença entre as duas arquiteturas é a forma como as operações são realizadas. A arquitetura Load/Store (como na ARM) separa as operações de carga e armazenamento das operações aritméticas e lógicas, enquanto a arquitetura Register/Register permite operações diretamente na memória ou entre registradores.

### (c) Os processadores **ARM Cortex-M** oferecem diversos recursos que podem ser explorados por sistemas baseados em **RTOS** (***Real Time Operating Systems***). Por exemplo, a separação da execução do código em níveis de acesso e diferentes modos de operação. Explique detalhadamente como funciona os níveis de acesso de execução de código e os modos de operação nos processadores **ARM Cortex-M**.

Os processadores ARM Cortex-M e Cortex-M3 possuem 2 estados de operação:

- **Thumb State (Estado Thumb):** Este é o estado em que o processador está processando código escrito com instruções Thumb.

- **Debug State (Estado de Depuração):** Este é o estado em que o processador se encontra quando uma depuração é acionada, seja por um depurador ou ao alcançar um breakpoint. Nesse estado, o processador cessa a execução de instruções e entra no modo de depuração.


Além disso, eles operam em dois níveis diferentes de acesso:

- **Nível de Acesso Privilegiado (PAL):** Se o código estiver rodando com PAL, ele tem total acesso aos recursos do processador e aos registradores restritos.

- **Nível de Acesso Sem-Privilégios (NPAL):** Se o código estiver rodando com NPAL, ele não terá acesso aos registradores restritos do processador.

O processador em modo de operação Thread pode rodar nos dois níveis de acesso, PAL e NPAL. O modo de operação Handler sempre roda somente em PAL. Quando se inicia uma aplicação, o processador por padrão roda no modo de operação Thread e com nível de acesso PAL.

Quanto aos modos de operação, o Processador ARM Cortex-M3 opera em dois modos diferentes:

- **Modo Thread:** O código normal da aplicação sempre é executado em modo Thread. Também conhecido como “Modo de Usuário” ou “User Mode”. Em modo Thread há níveis de privilégio de acesso, portanto, dependendo do privilégio podem haver restrições a acesso a registradores e outros recursos.

- **Modo Handler:** Todas as exceções ou interrupções de nossa aplicação rodam em modo Handler. No mode Handler o privilégio é total, sem restrições.

### (d) Explique como os processadores ARM tratam as exceções e as interrupções. Quais são os diferentes tipos de exceção e como elas são priorizadas? Descreva a estratégia de **group priority** e **sub-priority** presente nesse processo.

O controlador externo de interrupções/eventos consiste em até 23 detectores de borda para gerar solicitações de eventos/interrupções. Cada linha de entrada pode ser configurada independentemente para selecionar o tipo (interrupção ou evento) e o evento de disparo correspondente (ascendente ou decrescente ou ambos). Cada linha também pode ser mascarada de forma independente. Um registro pendente mantém a linha de status das solicitações de interrupção.

As interrupções são encontradas na tabela de Vetores de interrupção do manual do microcontrolador STM32F411 que segue:

| Posição  | Prioridade  | Tipo de Prioridade  | Acrônimo  | Descrição  | Endereço  |
| ------------ | ------------ | ------------ | ------------ | ------------ | ------------ |
|   | -  | Fixada  | NMI  | Non maskable interrupt  | 0x0000 0008  |
|   |  -3 | Fixada  | HardFault  | All class of fault  | 0x0000 000C  |
|   | -2  | Setável  | MemManage  | Memory management  | 0x0000 0010  |
|   | 0  | Setável  | BusFault  | Pre-fetch fault, memory access fault  | 0x0000 0014  |
|   | 1  | Setável  |   |   |   |
|   | 2  | Setável  | UsageFault  | Undefined instruction or illegal state  | 0x0000 0018  |
|   | -  | -  | -  | Reservado  | 0x0000 001C |
|   | 3  | Setável  | SVCall  | System Service call via SWI instruction  | 0x0000 002C  |
|   | 4  | Setável  | Debug Monitor  | Debug Monitor  | 0x0000 0030  |
|   |   | -  | -  | Reservado  | 0x0000 0034  |
|   | 5  | Setável  | PendSV  | Pendable request for system service  | 0x0000 0038  |
|   | 6  | Setável  | Systick  | System tick timer  | 0x0000 003C  |
| 0  | 7  | Setável  | WWDG  | Window Watchdog interrupt  | 0x0000 0040  |
| 1  | 8  | Setável  | EXTI16 / PVD  | EXTI Line 16 interrupt  | 0x0000 0044  |
| 2  | 9  | Setável  | EXTI21 / TAMP_STAMP  | EXTI Line 21 interrup  | 0x0000 0048  |
| 3  | 10  | Setável  | EXTI22 / RTC_WKUP  | EXTI Line 22 interrupt  | 0x0000 004C  |
| 4  | 11  | Setável  | FLASH  | Flash global interrupt  | 0x0000 0050  |
| 5  | 12  | Setável  | RCC  | RCC global interrupt  | 0x0000 0054  |
| 6  | 13  | Setável  |  EXTI0  | EXTI Line0 interrupt  | 0x0000 0058  |
| 7  | 14  | Setável  | EXTI1  | EXTI Line1 interrupt  | 0x0000 005C  |
| 8  | 15  | Setável  | EXTI2  | EXTI Line2 interrupt  | 0x0000 0060  |
| 9  | 16  | Setável  | EXTI3  | EXTI Line3 interrupt  | 0x0000 0064  |
| 10  | 17  | Setável  | EXTI4  | EXTI Line4 interrupt  | 0x0000 0068  |
| 11  | 18  | Setável  | DMA1_Stream0  | DMA1 Stream0 global interrupt  | 0x0000 006C  |
| 12  | 19  | Setável  | DMA1_Stream1  | DMA1 Stream1 global interrupt  | 0x0000 0070  |
| 13  | 20  | Setável   |  DMA1_Stream2  | DMA1 Stream2 global interrupt  | 0x0000 0074  |
| 14  | 21  | Setável  | DMA1_Stream3  | DMA1 Stream3 global interrupt  | 0x0000 0078  |
| 15  | 22  | Setável  | DMA1_Stream4  | DMA1 Stream4 global interrupt  | 0x0000 007C  |
| 16  | 23  | Setável  | DMA1_Stream5  | DMA1 Stream5 global interrupt  | 0x0000 0080  |
| 17  | 25  | Setável  | DMA1_Stream6  | DMA1 Stream6 global interrupt  | 0x0000 0084  |
| 18  | 25  | Setável   | ADC   | ADC1 global interrupts  | 0x0000 0088   |
| 23  | 30 | Setável  | EXTI9_5   | EXTI Line[9:5] interrupts   | 0x0000 009C   |




### (e) Qual a diferença entre os registradores **CPSR** (***Current Program Status Register***) e **SPSR** (***Saved Program Status Register***)?

- **CPSR (Current Program Status Register):** Este é o registrador de status do programa atual. Ele mantém bits de estado que indicam o resultado de operações lógicas e aritméticas, controlam interrupções e o modo de operação do processador.

- **SPSR (Saved Program Status Register):** Este registrador é usado para armazenar o valor atual do CPSR quando ocorre uma exceção, permitindo que o CPSR possa ser restaurado após o tratamento da exceção. Cada modo de tratamento de exceção pode acessar seu próprio SPSR.

Em outras palavras, o CPSR é usado para manter o estado atual do programa, enquanto o SPSR é usado para salvar o estado do programa que foi interrompido por uma exceção, permitindo que o estado possa ser restaurado após o tratamento da exceção.

### (f) Qual a finalidade do **LR** (***Link Register***)?

O **LR (Link Register)** é um registrador que tem a função de armazenar o endereço de retorno quando uma chamada de sub-rotina é concluída. Isso é mais eficiente do que o esquema mais tradicional de armazenar endereços de retorno em uma pilha de chamadas, às vezes chamada de pilha de máquina.

Quando uma chamada de sub-rotina é realizada, o endereço de retorno é colocado no **LR**. Após a conclusão da sub-rotina, o conteúdo do **LR** é copiado de volta para o contador de programa **(PC)**, permitindo que o programa continue de onde parou.

Portanto, a principal finalidade do **LR** é facilitar o controle do fluxo de execução do programa durante as chamadas de sub-rotinas, armazenando o endereço de retorno para que o programa possa continuar de onde parou após a conclusão da sub-rotina.

### (g) Qual o propósito do Program Status Register (PSR) nos processadores ARM?

O **Program Status Register (PSR)** nos processadores ARM é um registrador que mantém bits de estado que indicam o resultado de operações lógicas e aritméticas, controlam interrupções e o modo de operação do processador.

O **PSR** é composto por três registradores: o **Application Program Status Register (APSR)**, o **Interrupt Program Status Register (IPSR)** e o **Execution Program Status Register (EPSR)**.

- **APSR:** Contém flags do ALU (Arithmetic Logic Unit), que são bits de estado que indicam o resultado de operações lógicas e aritméticas.
- **IPSR:** Contém o número da ISR (Interrupt Service Routine) em execução.
- **EPSR:** Contém o bit T, que indica o estado do Thumb.

Portanto, o **PSR** é fundamental para o controle do fluxo de execução do programa, o gerenciamento de interrupções e a indicação do resultado de operações lógicas e aritméticas.

### (h) O que é a tabela de vetores de interrupção?

A tabela de vetores de interrupção é uma estrutura de dados que associa uma lista de rotinas de tratamento de interrupção a uma lista de solicitações de interrupção. Cada entrada na tabela, chamada de vetor de interrupção, é o endereço de uma rotina de tratamento de interrupção.

Quando uma interrupção é gerada, o processador salva seu estado atual e começa a executar a rotina de tratamento de interrupção apontada pelo vetor. Isso garante que nenhuma das tarefas executadas pelo sistema operacional entre em conflito.

Em muitas arquiteturas, a tabela de vetores de interrupção está localizada no início do espaço de memória, a partir do endereço 0. Em alguns sistemas, uma pilha independente é associada ao tratamento de interrupções.

Portanto, a tabela de vetores de interrupção desempenha um papel crucial no gerenciamento de interrupções em um sistema, permitindo que o processador responda de maneira eficiente a eventos internos e externos.

### (i) Qual a finalidade do NVIC (**Nested Vectored Interrupt Controller**) nos microcontroladores ARM e como ele pode ser utilizado em aplicações de tempo real?

O NVIC (Nested Vectored Interrupt Controller) nos microcontroladores ARM é um controlador de interrupções que fornece uma interface entre as fontes de interrupção externas ao núcleo (como periféricos e pinos externos) e o próprio núcleo. Ele suporta um número definido pela implementação de interrupções, na faixa de 1 a 240 interrupções.

A principal função do NVIC é gerenciar e priorizar essas interrupções. Cada fonte de interrupção tem uma prioridade programável, e se duas interrupções pendentes compartilham a mesma prioridade, a prioridade é dada à interrupção com o menor número de exceção (ou seja, o menor endereço do vetor de interrupção).

Em aplicações de tempo real, o NVIC é crucial para garantir uma resposta rápida a solicitações de interrupção, permitindo que uma aplicação atenda rapidamente a eventos de entrada. Uma interrupção é tratada sem esperar pela conclusão de uma longa sequência de instruções. Essas instruções serão reiniciadas ou retomadas após o retorno da interrupção.

### (j) Em modo de execução normal, o Cortex-M pode fazer uma chamada de função usando a instrução **BL**, que muda o **PC** para o endereço de destino e salva o ponto de execução atual no registador **LR**. Ao final da função, é possível recuperar esse contexto usando uma instrução **BX LR**, por exemplo, que atualiza o **PC** para o ponto anterior. No entanto, quando acontece uma interrupção, o **LR** é preenchido com um valor completamente  diferente,  chamado  de  **EXC_RETURN**.  Explique  o  funcionamento  desse  mecanismo  e especifique como o **Cortex-M** consegue fazer o retorno da interrupção. 

Em uma operação normal, o processador ARM Cortex-M utiliza a instrução BL (Branch with Link) para fazer uma chamada de função. Essa instrução altera o Contador de Programa (PC) para o endereço de destino da função e salva o endereço de retorno (o ponto de execução atual) no Registrador de Link (LR). Quando a função termina, o endereço de retorno é recuperado do LR e o PC é atualizado para esse endereço, permitindo que o programa continue de onde parou.

No entanto, quando ocorre uma interrupção, o comportamento do LR muda. Em vez de armazenar o endereço de retorno, o LR é preenchido com um valor especial chamado `EXC_RETURN`. O `EXC_RETURN` não é um endereço de memória, mas um código que informa ao processador como retornar da interrupção.

Quando a rotina de serviço de interrupção (ISR) é concluída, o valor do EXC_RETURN é movido para o PC. Isso faz com que o processador restaure o estado que foi salvo automaticamente quando a interrupção ocorreu, incluindo o PC, o LR e outros registradores. Isso permite que o programa continue de onde foi interrompido antes da interrupção.

### (k) Qual  a  diferença  no  salvamento  de  contexto,  durante  a  chegada  de  uma  interrupção,  entre  os processadores Cortex-M3 e Cortex M4F (com ponto flutuante)? Descreva em termos de tempo e também de uso da pilha. Explique também o que é ***lazy stack*** e como ele é configurado. 

Os processadores ARM Cortex-M3 e Cortex-M4F, ambos baseados na arquitetura ARMv7-M, são projetados para microcontroladores e possuem mecanismos eficientes para lidar com interrupções. No entanto, eles diferem na maneira como lidam com o salvamento de contexto durante uma interrupção, especialmente quando se trata de operações de ponto flutuante.

Cortex-M3: Quando uma interrupção ocorre, o processador automaticamente salva o estado de oito registradores (R0-R3, R12, LR, PC, xPSR) na pilha. Isso inclui o endereço de retorno da interrupção e o estado do Program Status Register (PSR). Isso é feito antes de começar a executar a rotina de serviço de interrupção (ISR). Quando a ISR é concluída, o processador restaura automaticamente esses valores da pilha.

Cortex-M4F: Este processador tem a capacidade adicional de realizar operações de ponto flutuante. Quando uma interrupção ocorre, além dos oito registradores salvos pelo Cortex-M3, o Cortex-M4F também pode salvar o estado de até 26 registradores adicionais na pilha. Isso inclui 16 registradores de ponto flutuante (S0-S15), FPSCR (Floating-Point Status and Control Register) e quatro registradores de uso geral (R4-R7). No entanto, esses registradores adicionais só são salvos se a ISR usar operações de ponto flutuante.

O salvamento de contexto adicional no Cortex-M4F leva mais tempo e usa mais espaço na pilha do que no Cortex-M3. No entanto, isso permite que o Cortex-M4F execute ISRs que usam operações de ponto flutuante sem corromper o estado dessas operações no código principal.

- **Lazy Stacking:** O Cortex-M4F também suporta um recurso chamado “lazy stacking”. Isso significa que os registradores de ponto flutuante só são salvos na pilha se forem usados pela ISR. Se a ISR não usar operações de ponto flutuante, o tempo e o espaço da pilha necessários para salvar esses registradores são economizados. Isso é configurado automaticamente pelo processador.


## Referências

### Básicas

[1] [STM32F411xC Datasheet](https://www.st.com/resource/en/datasheet/stm32f411ce.pdf)

[2] [RM0383 Reference Manual](https://www.st.com/resource/en/reference_manual/rm0383-stm32f411xce-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)

[3] [Using the GNU Compiler Collection (GCC)](https://gcc.gnu.org/onlinedocs/gcc/index.html)

[4] [GNU Make](https://www.gnu.org/software/make/manual/html_node/index.html)

### Vídeos Microsoft Teams

[5] [05 - Introdução à arquitetura de computadores](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[6] [06 - Arquitetura Cortex-M Parte 1/2](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[7] [07 - Arquitetura Cortex-M Parte 2/2](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[8] [08 - Microcontroladores STM32](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[9] [10 - Introdução à arquitetura de computadores](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

### Material Suplementar

[5] [A General Overview of What Happens Before main()](https://embeddedartistry.com/blog/2019/04/08/a-general-overview-of-what-happens-before-main/)
 
[6] [Bare metal embedded lecture-1: Build process](https://youtu.be/qWqlkCLmZoE?si=mn5yDnJYudQ1PpZH)
 
[7] [Bare metal embedded lecture-2: Makefile and analyzing relocatable obj file](https://youtu.be/Bsq6P1B8JqI?si=yuNLPj3JQ-2IT1yo)
 
[8] [Bare metal embedded lecture-3: Writing MCU startup file from scratch](https://youtu.be/2Hm8eEHsgls?si=c27MpZ47ApiMSwHR)
 
[9] [Lecture 15: Booting Process](https://youtu.be/3brOzLJmeek?si=MsHRUEJP8zofjwJQ)