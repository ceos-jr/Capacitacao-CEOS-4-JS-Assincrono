# O Event Loop

Esse tópico é o pesadelo de vários programadores JavaScript, já que ele se vincula diretamente em como a programação JavaScript funciona por debaixo dos panos no nível design de linguagem. Porém, ele é de vital importância para que possamos entender por que as coisas funcionam do jeito que funcionam e como o JavaScript realiza suas ações.

Por exemplo, você viu que o ***setTimeout*** executa uma função callback após um determinado número de milissegundos não é mesmo?

Então pense, o que aconteceria quando executamos algo com esse parâmetro igual a 0? No nosso conceito de tempo em geral, quando algo demora 0ms para iniciar, ela é iniciada imediatamente, e como em JavaScript e outras linguagens de programação, a unidade mais baixa em que conseguimos quebrar o tempo são os milissegundos, um ***setTimeout*** para 0ms executa o código imediatamente.

Logo, você já deve esperar a saída do código a seguir:

```JavaScript
	console.log("1");
	setTimeout(()=> console.log("2"), 0);
	console.log("3");
```
Como vimos, a saída é trivial e mostra:
```
	1
	2
	3
```

E é isso, chegamos ao fim desse módulo... 

Só que as coisas não são bem assim, e esse pensamento relativamente "leigo" é o que leva pessoas a cometerem erros que podem ter consequências altas, principalmente quando trabalhamos com operações ***assíncronas***.

Se você já tem um conhecimento ok de JavaScript ou de linguagens de programação em geral, você provavelmente já sabe do que eu estou falando, mas senão, vá em frente, copie o código e rode-o no terminal.

Vamos, eu espero...

Rodou? Entendeu? Não? Muitos não entendem, mas a real saída é, na verdade, essa:

		1
		3
		2

E então? Tá errado? Bugou? O computador errou? JavaScript é uma linguagem lixo? Não tá entendendo nada?

Calma jovem, o código não bugou, o JavaScript não fez nada de errado ele funcionou exatamente do jeito que deveria funcionar. Não consegue engolir isso? ***Ótimo!*** Aqui nada deve ser engolido, mas sim, questionado. E é por isso que esse módulo se responsabiliza por explicar como o JavaScript funciona, para que um conhecimento mais profundo na linguagem deixe os procedimentos assíncronos claros como a luz para você, de modo que você tenha total controle do que você está fazendo.


Mas vamos com calma, antes de explicarmos o que acabou de aconteceu com essa aberração da lógica de programação, vamos entender um pouco sobre a ***Pilha de Funções***, ou, do inglês, ***Function Stack***.

## Function Stack

A pilha de funções basicamente descreve como as funções são executadas no JavaScript, principalmente em que ordem.

A melhor maneira de explicar a Pilha de Funções é através de um exemplo:

Suponha que você tem o código abaixo:

```JavaScript
	function bomDia(){
		console.log("Bom dia");
	}

	function boaTarde(){
		bomDia();
		console.log("Boa tarde");
	}

	function boaNoite(){
		boaTarde();
		console.log("Boa noite");
	}

	boaNoite();
```

E aí? Após executar esse código, o que você veria na tela?

Você esperaria ver isso?
	
		Bom dia
		Boa tarde
		Boa noite

Pois é aí que você está...

Completamente certo! Não se preocupe, não tem pegadinha aqui, esse código ainda imprime isso, você não vai precisar voltar ao módulo de JavaScript pra revisar funções. Mas vamos *dissecar* essa execução e ver o que está acontecendo.

A Pilha de Funções vai conter todas as funções que estão atualmente executando no programa. Quando uma função é executada ela entra para a pilha, quando essa função termina de executar ela sai da pilha. Desse modo, quando temos uma funçao sendo chamada dentro da outra, a primeira função é empilhada e, antes de ser desempilhada, a função mais interna empilhada, executada e desempilhada. Só então a função mais externa termina de executar e é desempilhada.

Por exemplo, a pilha de funções está da seguinte forma:

No início, nada foi chamado ainda, logo a pilha está vazia:

![PILHA1](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/raw/master/.github/pilha1.png)

Depois, a função ***boaNoite*** é chamada, e portanto, é empilhada:

![PILHA2](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/raw/master/.github/pilha2.png)

A função ***boaNoite*** por sua vez chama a função ***boaTarde*** e esta é empilhada:

![PILHA3](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/raw/master/.github/pilha3.png)

E a função boaTarde chama a função ***bomDia*** e esta é empilhada:

![PILHA4](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/raw/master/.github/pilha4.png)

A função bomDia está executando e chama a função *console.log*, que também é empilhada:

![PILHA5](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/raw/master/.github/pilha5.png)

Após ser chamada, a função *console.log* executa e se encerra, logo ela sai da pilha de funções (desempilhada):

![PILHA4](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/raw/master/.github/pilha4.png)

E como era de se esperar, a função ***bomDia*** se encerra e sai da pilha:

![PILHA6](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/raw/master/.github/pilha6.png)

Esse procedimento acontece também para a função ***boaTarde***:

![PILHA7](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/raw/master/.github/pilha7.png)

E finalmente para a função ***boaNoite***:

![PILHA8](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/raw/master/.github/pilha8.png)

Encerrando o nosso programa com a Pilha de Funções vazia.

Tendo em vista que é uma pilha, ela empilha as funções a serem executadas de modo que a última função a ser empilhada é a primeira a ser executada. Por isso que no nosso código ***bomDia*** é executada primeiro que ***boaTarde***, e esta primeiro que ***boaNoite***.

E não, eu não estou inventando isso, é exatamente assim que as coisa funcionam no JavaScript. Claro que eu abstrai as outras funções que são executadas com a função *console.log*, mas isso não nos diz respeito no momento, o nível de abstração que usamos é o suficiente pra ver a Pilha de Funções em ação.

Tudo bem, tendo entendido a Pilha de Funções, já podemos entender por que o nosso primeiro código imprimiu:

		1
		3
		2

Certo? **ERRADO!**

Vamos pensar um pouco, com a lógica da Pilha de Funções, teríamos o seguinte:

Primeiramente, com a pilha vazia, a função *console.log* iria pra pilha, executaria e sairia da pilha:

![LOOP1](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/raw/master/.github/loop1.png)

E assim, temos no terminal:

	1

Ok, ok, tudo certo até aí.

Agora o ***setTimeout*** entra pra pilha, diz que após 0ms outra função vai ser executada, e, finalmente, sai da pilha:

![LOOP2](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/raw/master/.github/loop2.png)

0ms se passam ("*instantâneo*") e a função que foi solicitada pelo ***setTimeout*** é executada, mandando um *console.log* na tela e saindo da pilha:

![LOOP3](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/raw/master/.github/loop3.png)

![Loop4](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/raw/master/.github/loop4.png)

E temos no terminal

		1
		2

Finalmente, o terceiro *console.log* vai para a pilha, é executado e por fim, sai dela:

![LOOP5](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/raw/master/.github/loop5.png)

E nosso terminal fica assim

		1
		2
		3

E tudo faz sentido, tá tudo certo, JavaScript é ruim...

Pois é, isso poderia até fazer sentido, **se** fosse da responsabilidade da ***Pilha de Funções*** o tratamento da quantia de tempo que passa. O que na verdade, não é!

"*Mas são funções sendo executadas, e toda função vai pra Pilha de Funções, como que não é dela essa responsabilidade?*"

Pois é, querendo ou não  a ***Pilha de Funções*** é responsável por executar o *console.log* alguma hora, porém, não é dela a responsabilidade de saber *quando será essa hora*.

Na verdade, o responsável por isso é outro carinha muito legal também com um funcionamento um pouco diferente, o nosso querido ***Loop de Eventos***, ou como também pode ser chamado, ***Event Loop***!

## Event Loop

Antes de entendermos como o ***Event Loop*** funciona, vamos entender o que é um ***evento*** JavaScript.

É bem difícil descrever o que é um evento JavaScript, mas ao mesmo tempo não é. Um evento JavaScript é similar a um evento da vida real. É uma coisa que acontece.

Por exemplo:

* Um semáforo abrindo
* O sinal de uma escola tocando para indicar o fim do recreio
* Você acenando para um amigo
* Uma encomenda que você pediu no *AliExpress* chegando após 6 anos e 3 dias

Tudo isso é um evento no mundo real. Alguns maiores e outros menores, alguns impactam várias pessoas, outros impactam poucas ou nenhuma pessoa. E um evento JavaScript é a mesma coisa, porém, logicamente, no contexto do JavaScript, podendo ser:

* Um clique em um botão
* Uma página web carregando
* Uma requisição ao banco de dados retornando
* O usuário digitando em um campo de texto
* O tempo de um ***setTimeout*** sendo atingido

E várias outras. Basicamente, tudo que depende de um fator externo acontece devido à um evento.

Podemos entender agora por que o ***setTimeout*** funciona. Após um certo número de milissegundos ele dispara um evento que é capturado pelo JavaScript e sinaliza que o callback associado à esse ***setTimeout*** deve ser executado.

E funciona assim para várias outras coisas. Como por exemplo, você disparar um alerta na tela quando um usuário clicar em um botão.

Tudo bem, acredito que tenha dado para entender o que é um evento em JavaScript. Deu pra entender como o ***setTimeout*** sinaliza que um evento deve acontecer após uma quantia certa de milissegundos. E como vimos anteriormente, o ***setTimeout*** vai pra **Pilha de Funções** e de lá diz que um evento deve ser disparado após um certo tempo. Só que até agora não nos foi respondido como que esse evento faz com que a função seja executada!

Já foi dito que a **Pilha de Funções** não checa se o evento foi disparado. Como foi dito, o responsável para isso é um rapaz chamado ***Event Loop***.

O ***Event Loop*** basicamente é exatamente o que parece. Um laço infinito que checa sempre se algum evento aconteceu. Sempre que um evento acontece ele joga esse evento em um lugar, chamado ***Fila de Eventos***, ou como também é chamada, ***Event Queue***.

A ***Event Queue*** recebe os eventos disparados do ***Event Loop***, e é da sua responsabilidade passar esses eventos para a ***Pilha de Funções***. Como? Vamos explicar abaixo passo a passo:

* A Pilha de Funções executa um ***setTimeout***
* Após um certo período de tempo, o evento do ***setTimeout*** é disparado pelo ambiente
* Esse evento é capturado pelo ***Event Loop***
* O ***Event Loop*** joga o evento de execução da função na Fila de Eventos
* Quando a Pilha de Funções está *desocupada*, a Fila de Eventos passa a função para ela
* Finalmente, a Pilha de Funções executa a função que foi mandada pelo ***setTimeout***

Uma dúvida deve ter vindo à cabeça de vocês, sendo essa dúvida:

"*Mas se funciona assim, por que isso não acontece?*
*A Fila de Eventos manda a função imediatamente, pois 0ms se passaram e, portanto, 2 é impresso antes de 3 no exemplo*"

Pois é, isso até aconteceria, se não fosse por um detalhe. A Fila de Eventos NUNCA interrompe a Pilha de Funções, ela só manda a função para ela quando a Pilha de Funções está *desocupada*, ou seja, está **ociosa**.

Isso acontece devido à prioridade que é estabelecida no ambiente JavaScript. A Fila de Funções tem uma prioridade menor em relação às funções para serem executadas presentes no código.

Podemos ver isso mais claramente no exemplo abaixo:

```JavaScript
	console.log("1");
	setTimeout(()=> console.log("2"), 0);
	for(let i = 0; i< 1000000000; i++){
		// Não estou fazendo nada
		// Apenas ocupando a pilha
	}
	console.log("3");
```

Execute o exemplo para ver melhor seu funcionamento.

Podemos ver que não importa que o evento tenha sido disparado, ele vai esperar a Pilha ficar ociosa.

Mas ***ATENÇÃO***, o evento não está sendo disparado apenas depois do laço. Ele foi disparado realmente quando os 0ms passaram ('*instanteneamente*'), porém, a sua função só será executada quando a Pilha de Funções estiver ociosa, ou seja, não tiver mais nenuma função pendente para ser executada no código.

Veja abaixo como ***realmente*** está acontecendo no nosso primeiro exemplo:

Primeiramente, nada de especial, temos todos os nossos trabalhadores do JavaScript parados, nada começou ainda

![FULL1]

Então, o primeiro console.log entra na Pilha de Funções:

![FULL2]

Depois de ser executado, ele sai da Pilha:

![FULL1]

Então, o setTimeout entra na Pilha de Funções:

![FULL3]

Após deixar o evento para ser disparado alguma hora, ele sai da pilha

![FULL1]

Vamos supor que seja realmente instantâneos os nosso 0ms, desse modo, assim que ele sair da pilha o evento será disparado e capturado pelo nosso ***Event Loop***:

![FULL4]

O ***Event Loop*** então manda a função callback para a Fila de Funções, onde ela será executada assim que tiver chance:

![FULL5]

Porém, a Pilha de Funções ainda não está ociosa! Ela pode estar vazia, mas no nosso código, ainda há uma função *console.log* para ser executada, e é isso que acontece. Essa função entra para a Pilha:

![FULL6]

Após ela ser executada, ela sairá da Pilha:

![FULL5]

Agora sim! Nossa Pilha está ociosa, não há mais nada previsto para ser executado, logo ela está pronta para receber funções da Fila de Funções, e é isso que acontece, ela recebe o *console.log* que estava esperando na fila:

![FULL7]

Finalmente, nosso *console.log* é executado e sai da fila

![FULL8]

E isso encerra o nosso programa, com todos os nosso trabalhadores parados novamente.

![FULL1]

E é isso! Agora sim, essa realmente é a maneira que o JavaScript lida com essas operações.

Logicamente eu omiti várias outras coisas de complexidade muito maior que ocorrem por baixo dos panos, mas por hora elas não nos dizem respeito. Mas para quem tiver curiosidade e disposição pra quebrar a cabeça entendendo, super recomendo a leitura dos links abaixo:


[Entendendo Código Assíncrono em JavaScript](https://blog.bitsrc.io/understanding-asynchronous-javascript-the-event-loop-74cd408419ff) - Bits and Pieces
*Tempo médio de leitura: 13 minutos*

[Vídeo: Loop de Eventos JavaScript](https://www.youtube.com/watch?v=XzXIMZMN9k4) - ComSience Simplified
*Duração: 8:19 min*

[Como o JavaScript trabalha](https://blog.sessionstack.com/how-javascript-works-event-loop-and-the-rise-of-async-programming-5-ways-to-better-coding-with-2f077c4438b5) - SessionStack
*Tempo médio de leitura: 25 minutos*

[JavaScript e a mágica assíncrona](https://levelup.gitconnected.com/javascript-and-asynchronous-magic-bee537edc2da) - Git Connected
*Tempo médio de leitura: 14 minutos*

E para esse tópico é isso. Pense nisso toda vez que trabalhar com esses eventos, pode salvar a sua vida (e **muito** tempo também).