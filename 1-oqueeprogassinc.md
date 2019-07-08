# O que é programação assíncrona?

Normalmente, estamos acostumados a programar proceduralmente, ou seja, uma ação é feita imediatamente após a outra, podendo haver repetições, decisões, etc.
Geralmente é algo assim:

    fazercoisa1()
    fazercoisa2()
    fazercoisa3()
    usarDadoDaCoisa2()
    usarDadoDaCoisa3()
    ...
    
Tudo feito em questão de milissegundos quase que imediatamente e cada coisa espera sua vez para ser feita, e viveram felizes para sempre.
Mas sabemos que nem sempre resolver problemas através de códigos é um conto de fadas.
  
Imagine o exemplo anterior, onde as funções ***fazercoisa1*** e ***fazercoisa3*** são executadas sem problemas e quase que imediatamente, mas a função ***fazercoisa2** demora um pouco para ser executada. 

Mas como algo assim poderia acontecer?

Imagine que essa função depende de um fator que não podemos controlar, como por exemplo, **acessar um dado no banco de dados em um servidor**.

Essa operação demora uma quantia de **tempo** que depende de vários fatores, como por exemplo, a internet do usuário. Como usamos o dado obtido através da ***fazercoisa2*** em ***usarDadoDaCoisa2*** temos um problema, e se quando minha função ***usarDadoDaCoisa2*** for executada mas meu dado obtido em ***fazercoisa2*** não esteja pronto? (isso pode acontecer quando, por exemplo, a internet do usuário está **MUUUUUITO** lenta e demorou **10s** para retornar o dado)

Teríamos um erro já que ***usarDadoDaCoisa2*** usa um dado que não está pronto, provavelmente esteja como *null*, ou *undefined*. Deu pra entender que tipo de problema teríamos?

Se esse exemplo com funções de nomes estranhos (e até mesmo idiotas devo dizer) não conseguiu demonstrar o exemplo, vamos para uma abstraço mais do mundo real. Suponha que temos uma linha de montagem onde devemos montar um carro. Essa linha de montagem funciona bem, e todos fazem seu trabalho muito rápido ao montar um carro. 

Primeiro trazem o parachoque e o paralama, o motor, os bancos, o para brisa, o capô e os faróis, seguido dos pedais, da marcha e etc. Eu sei que um carro não é montado assim (e acredito que você também saiba, se não souber melhor repensar sobre a validade da sua carteira de motorista se tiver uma) mas vamos imaginar por hora que um carro possua essas partes:

* **Lataria**
* **Bancos**
* **Motor**
* **Faróis**
* **Rodas**
* **Freios**

E logo após a montagem, um motorista de testes pega o carro e dirige por um circuito onde ele precisa acelerar até 100km/h e depois desacelerar até parar o carro.
  
Bem simples e procedural não é? Podemos até tentar "javascriptzar" esse processo de montagem da seguinte forma:

    montaLataria()
    colocaBancos()
    instalaMotor()
    colocaFaróis()
    colocaRodas()
    instalaFreios()
    testeDriveA100kmh()
    
Tudo bem simples, parece um conto de fadas como estávamos dizendo. Mas está na hora da bruxa má do oeste chegar para fazer a casa voar e uma menina com um cachorro entrar em uma aventura com robôs, leões e espantalhos vivos (parando pra pensar, esses contos de fada são meio macabros).

Suponha que tudo corre perfeitamente bem, sabe-se que leva-se 10 min para cada operação, logo o motorista apenas espera 1h e vai testar o carro, afinal, o que pode ter dado errado não é mesmo?

Imagine que o o rapaz que ia instalar os freios tropeçou e deixou cair os freios, danificando-os, ele precisa voltar, pegas novos freios e instalá-los. Passou a 1h esperada e o motorista foi fazer o teste drive, **SEM OS FREIOS**. Que este descanse em paz. **R.I.P.**

Imagine outras situações similares. Instalaram o motor antes do banco devido à um atraso do estofamento e agora a fiação do motor está por todos os lados, um caos dentro do carro. Isso gera mais problemas e atrasos na instalaço do banco, que por sua vez gera atrasos em todas as outras. E quando menos se espera, lá se vai a casa voando.

Pode parecer um cenário muito irreal e bobo, mas acredite, isso acontece muito comumente no mundo da programação se não tivermos essa noção, e se você não tiver cuidado, vai acontecer com VOCÊ (cuidado com o desligamento rapaz/moça/ser-não-binário).

Cada procedimento pode ser comparado à chamadas de funções que geram os dados que precisamos, o test drive é quando mandamos os dados processados de algum modo para o cliente, e a morte do pobre motorista é quando o cliente vê isso na tela:

    ERRO: undefined is not a function
    callbackTrace(...).stopAtRecursion('parabéns_voce_estragou_tudo')
    ...
   
Vamos pensar, como podemos consertar e evitar isso? Alguns de vocês devem estar pensando:

*"Isso é problema da internet do usuário,ele que se *$%#@#%$*, arruma uma internet decente"*

Mas a culpa **NUNCA** é do usuário, logo podemos descartar essa desculpa. Lembre-se, o cliente sempre tem razão. A culpa não é da internet dele por sua aplicação ser mal feita.

Uma solução poderia ser proposta da seguinte maneira:
Cada operação só pode ser executada após a outra acabar. Desse modo evitamos o caos na linha de montagem e a morte do motorista.

Isso é válido e funciona, porém, está longe de ser o ideal. Isso trava a sua aplicação sempre que algo dá errado, mesmo que as próximas operações não dependam diretamente da que está sendo processada e poderiam ser adiantadas. Assim seu programa não vai ser fluido como o seu usuário espera.

O ideal é: O que depende de algo ser feito vai ser executado apenas quando essa coisa terminar, o que não depende não será afetado.

Voltando ao nosso primeiro exemplo, seria como se:

* ***fazercoisa1()***
* ***fazercoisa2()***
* ***fazercoisa3()***
    
São feitas, ***usarDadoDaCoisa2()*** é feito quando ***fazerCoisa2()*** termina e ***usarDadoDaCoisa3()*** é feito quando ***fazerCoisa3()*** termina. As outras operações não vão esperar nada se não precisarem.

Outro exemplo seria algo assim:

Você está em uma fila de um banco, porém, um cliente quer fazer um depósito, porém, o sistema está indisponível para depósitos, só irá voltar a funcionar para depósitos daqui a 20 minutos. Você apenas precisa fazer um saque, e o sistema está disponível para saques. O que seria o ideal? 
1. O caixa anda para as operações que não precisam de depósito e quando o sistema voltar a ser disponível para essa operação, os clientes que desejam depositar algo na conta são imediatamente atendidos.
2. Todo mundo espera 20 minutos até o cliente que quer fazer um depósito seja atentido?
  Idealmente, o primeiro caso é o mais agradável para todos, não concorda?

É nisso que a programação assíncrona tenta realizar com programas que dependem de fatores externos, deixar a aplicação mais fluida e contínua, dependendo menos do fator externo para que o resto da aplicação seja executada.

E respondendo à pergunta que é o título dessa parte, programação **síncrona** é quando uma operação executa completamente antes de executar a operação seguinte. Já na programaço **assíncrona** é quando não esperamos que uma operação seja terminada necessariamente, as operações que não dependem dela são executadas se esta demorar demais.

Se tudo isso ficou claro teoricamente, mas você no faz ideia de como aplicar isso com JS e com programação WEB, não se preocupe. Ao longo deste módulo, iremos desvendar todos os mistérios da programação assíncrona em JS, desde os modos mais básicos até as técnicas mais adequadas e recentes.

Se ainda está curioso a respeito ou meus exemplos geniais (são idiotas eu sei, deixa eu ser feliz só um pouco vai) não sanaram suas dúvidas, esses links possuem uma leitura interessante, que é recomendada à todos, e vital para aqueles que não entenderam o conteúdo ainda:

[O que é o assincronismo?](https://pt.stackoverflow.com/questions/124283/o-que-%C3%A9-o-assincronismo) - *Stack Overflow* <br/>
*Tempo médio de leitura: 3 minutos*

[Requisições síncronas e assíncronas](http://www.diogomatheus.com.br/blog/php/requisicoes-sincronas-e-assincronas/) - *Diogo Matheus* <br/>
*Tempo médio de leitura: 2 minutos*

[JavaScript Assíncrono](https://medium.com/@alcidesqueiroz/javascript-ass%C3%ADncrono-callbacks-promises-e-async-functions-9191b8272298) - *Medium* <br/>
*Tempo médio de leitura: 10 minutos*



