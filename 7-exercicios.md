# Lista de Exercícios

Vocês realmente achariam que só ia ter 5 exercícios no MEU módulo? (para os que não se tocaram ainda pelo meu estilo de escrita, sou eu, **Batista**, que desenvolvi esse módulo... SURPRESA >:)  )

Pois bem, abaixo vocês vão encontrar diversos exercícios que testarão seus conhecimentos obtidos através desse módulo.

Vocês não precisar fazer tudo de uma vez e cada exercício está isolado em uma issue, logo, ao fazer um exercício, basta dar Pull Request na sua resolução.

Tente fazer os exercícios por conta própria. **SE** não conseguir, recomendo que volte ao tópico apropriado e releia-o (dê atenção aos links que eu coloquei, eles podem ajudar).

## Callbacks

***Exercício Proposto 6:***

Você deve criar o seu próprio ***Callback Hell***.
Para isso:

* Crie uma função ***recebo3*** que recebe 3 argumentos, sendo eles 3 funções. Essa função deve executar uma das funções como callback.

* Crie uma função ***recebo2*** que recebe 2 argumentos, sendo eles 2 funções. Essa função deve executar uma das funções como callback.

* Crie uma função ***recebo1*** que recebe apenas um argumento, sendo ele uma função. Essa função deve executar essa função como callback.

* Crie uma função ***recebo0*** que não recebe nenhum argumento.

* Cada função deve ter dentro de sua execução a seguinte linha de código:
```
	console.log("Sou o receboX");
```
Sendo receboX o nome da função que executa essa linha de código.

* Estruture seu código de modo que: ***recebo3*** chama ***recebo2*** que chama ***recebo1*** que chama ***recebo0***.


Você saberá se acertou se a sua saída no terminal for:

		Sou o recebo3
		Sou o recebo2
		Sou o recebo1
		Sou o recebo0

<br/>
<br/>
<br/>

***Exercício Proposto 7:***

Vamos brincar de novo com as funções  ***fs.readFile*** e ***fs.writeFile***.
Faça uma função que:

* Leia o texto de um arquivo teste.txt
* Escreva esse texto em um arquivo saidaNormal.txt
* Escreva esse texto com todas as letras em minúsculo em um arquivo saidaMinuscila.txt
* Escreva esse texto com todas as letras em maiúsculo em um arquivo saidaMaiuscula.txt


***obs:*** Está estritamente proibido o uso das funções ***readFileSync*** e ***writeFileSync***!

[Documentação: File Reading](https://nodejs.org/api/fs.html#fs_fs_readfile_path_options_callback) - NodeJs

***obs:*** Para utilizar as funções acima, primeiro de tudo, escreva a linha de código abaixo e depois poderá chamar as funções com ***fs.NOMEDAFUNCAO(ARGUMENTOS)*** :

		const fs = require('fs');

<br/>
<br/>
<br/>

## Promises

***Exercício Proposto 8:***

Faça uma função que recebe um número e retorna uma Promise, que:

* **SEMPRE** imprime o número recebido na tela com o uso de *console.log*

* **É REJEITADA** caso o número for menor que 0 (zero), nesse caso, imprima na tela com o uso de *console.log* a frase *"Número negativo!"*

* **É RESOLVIDA** caso o número for maior ou igual a 0 (zero), nesse caso, imprima na tela com o uso de *console.log* a frase *"Raiz quadrada: X"*, sendo X a raiz quadrada do número digitado.

<br/>
<br/>
<br/>

***Exercício Proposto 9:***

Crie duas funções, sendo que:

* Uma dessas funções recebe uma lista de Strings e retorna uma Promise, sendo essa Promise **rejeitada** caso algum elemento da lista não seja um objeto do tipo String ( use ***typeof()*** ), e **resolve** caso contrário, retornando a lista de modo que toda as Strings estejam em letras MAIÚSCULAS.
<br/>
* A outra recebe uma lista de Strings e retorna uma Promise, que é sempre **resolvida** e retorna a lista de Strings orndenada, para isso utilize o método ***sort***.

Depois de criar essas funções, ***encadeie as Promises***, de modo que:

* Você execute a primeira função
* Se a primeira função resolver, execute a segunda
* Quando a segunda terminar de executar, imprima a lista modificada na tela

Para isso, use o ***then()*** das Promises e preste atenção em como você pode manipular as rejeições e resoluções.

## Funções Assíncronas

***Exercício Proposto 10:***

Jooj está com problemas. Ele fez o módulo de Funções Assíncronas e está medindo seus conhecimentos, desenvolvendo um código que atualmente funciona da seguinte forma:

* Uma função ***lendoDB*** dorme assíncronamente por 1 segundo inteiro e depois retorna um valor 123

* Ele quer algo simples, apenas colocar o valor retornado de ***lendoDB*** dentro de uma variável e depois, imprimí-la com *console.log*.

Parece fácil não é? Pois é, Jooj não conseguiu fazer dar certo! Ele tentou e tentou e só conseguiu um:

		Promise { <pending> }

Puxa vida não é Jooj? Acho que você não prestou muita atenção ao módulo de ***Async Functions***. Ajude Jooj a fazer seu código funcionar corretamente

Segue abaixo o progresso até agora de Jooj:

```JavaScript
	const sleep = (milliseconds) => {
		return new Promise(resolve =>{
			setTimeout(resolve, milliseconds)
		});
	}

	async function lendoDB(){
		await sleep(1000);
		return 123;
	}

	let a;
	a = lendoDB();
	console.log(a);
```

O retorno ideal vindo do terminal para que o exercício esteja resolvido corretamente deve ser:

		123


***obs:*** Jooj copioi a função ***sleep*** do módulo, então ela está 100% correta e fazendo o que deveria fazer.