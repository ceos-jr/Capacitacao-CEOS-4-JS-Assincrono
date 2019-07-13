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

A pilha de eventos tem todo o processo de uma função enquanto esta está executando, logo, ele começa a encher quando a função começa e esvazia quando a função termina.

Por exemplo, a pilha de funções está da seguinte forma:

No início, nada foi chamado ainda, logo a pilha está vazia:

![PILHA1](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/blob/master/.github/pilha1.png)

Depois, a função ***boaNoite*** é chamada, e portanto, é empilhada:

![PILHA2](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/blob/master/.github/pilha2.png)

A função ***boaNoite*** por sua vez chama a função ***boaTarde*** e esta é empilhada:

![PILHA3](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/blob/master/.github/pilha3.png)

E a função boaTarde chama a função ***bomDia*** e esta é empilhada:

![PILHA4](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/blob/master/.github/pilha4.png)

A função bomDia está executando e chama a função *console.log*, que também é empilhada:

![PILHA5](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/blob/master/.github/pilha5.png)

Após ser chamada, a função *console.log* executa e se encerra, logo ela sai da pilha de funções (desempilhada):

![PILHA4](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/blob/master/.github/pilha4.png)

E como era de se esperar, a função ***bomDia*** se encerra e sai da pilha:

![PILHA6](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/blob/master/.github/pilha6.png)

Esse procedimento acontece também para a função ***boaTarde***:

![PILHA7](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/blob/master/.github/pilha7.png)

E finalmente para a função ***boaNoite***:

![PILHA8](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/blob/master/.github/pilha8.png)

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

![LOOP1](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/blob/master/.github/loop1.png)

E assim, temos no terminal:

	1

Ok, ok, tudo certo até aí.

Agora o ***setTimeout*** entra pra pilha, diz que após 0ms outra função vai ser executada, e, finalmente, sai da pilha:

![LOOP2](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/blob/master/.github/loop2.png)

0ms se passam ("*instantâneo*") e a função que foi solicitada pelo ***setTimeout*** é executada, mandando um *console.log* na tela e saindo da pilha:

![LOOP3](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/blob/master/.github/loop3.png)

![Loop4](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/blob/master/.github/loop4.png)

E temos no terminal

		1
		2

Finalmente, o terceiro *console.log* vai para a pilha, é executado e por fim, sai dela:

![LOOP5](https://github.com/ceos-jr/Capacitacao-CEOS-4-JS-Assincrono/blob/master/.github/loop5.png)

E nosso terminal fica assim

		1
		2
		3

E tudo faz sentido, tá tudo certo, JavaScript é ruim...

Pois é, isso poderia até fazer sentido, **se** fosse da responsabilidade da ***Pilha de Funções*** o tratamento da quantia de tempo que passa. O que na verdade, não é!

"*Mas são funções sendo executadas, e toda função vai pra Pilha de Funções, como que não é dela essa responsabilidade?*"

Pois é, querendo ou não  a ***Pilha de Funções*** é responsável por executar o *console.log* alguma hora, porém, não é dela a responsabilidade de saber *quando será essa hora*.

Na verdade, o responsável por isso é outro carinha muito legal também com um funcionamento um pouco diferente, o nosso querido ***Loop de Eventos***!
