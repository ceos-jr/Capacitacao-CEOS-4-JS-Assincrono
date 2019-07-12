# Funções Assíncronas

Até agora vimos dois modos de lidarmos com problemas que exigem um entendimento maior do nosso programa em relação ao tempo que as ações levam para ser executadas, sendo eles os ***Callbacks*** e as ***Promises***.

Vimos que os ***Callbacks*** são bem úteis, embora sejam relativamente problemáticos quando usados em larga escala.

Vimos também que as ***Promises*** são uma maneira melhor de tratar esse problema, visto que permitem um maior controle do nosso programa em relação ao estado das requisições, etc.

Mas com a chegada do ***ES6***, várias coisas foram adicionadas ao JavaScript, e uma delas que não podemos ignorar de jeito nenhum são as maravilhosas ***Async Functions***, ou ***Funções Assíncronas*** em português.

A melhor maneira de explicar o que é uma ***Async Function***, nada mais é que:

***Async Functions*** são funções que sabem *esperar*.

Sabemos que nem todo nosso código pode ser 100% procedural, e que funções podem ajudar muito no quesito de sua modularização. O que a ***Async Function*** faz é se preparar para lidar melhor com Promises dentro dela.

Podemos declarar uma ***Async Function*** usando o prefixo ***async*** antes de sua declaração, como vemos no exemplo a seguir:

```JavaScript
	function async minhaFuncao(){
		...
	}
```

Para declararmos uma função assíncrona anônima e atribuírmos seu valor a uma variável, podemos fazer da seguinte forma:

```JavaScript
	const minhaFuncao = async () => {
		...
	}
```

Mas será que issso já basta para termos uma função que realmente *sabe* como esperar? Vamos ver com a ajuda deste exemplo abaixo:

Temos a função ***sleep***, ela simplesmente manda o código dormir por um certo valor de tempo. Como você pode ver, ela simplesmente é uma função que retorna um ***Promise*** que se resolve após uma certa quantia de tempo.

```JavaScript
	const sleep = (milliseconds) => {
		return new Promise(resolve =>{
			setTimeout(resolve, milliseconds);
		});
	}
```

Mas essa função tem um aspecto ***não bloqueante***, ou seja, ela não irá bloquear a thread principal de execução do JavaScript (*falou tudo e n disse nada*).

Na minha opinião, a melhor maneira de explicar o que é um aspecto ***bloqueante*** e ***não bloqueante*** é com exemplos, como o que vemos abaixo:


A função ***sleepBlock*** abaixo é uma versão ***bloqueante*** da função ***sleep***. Execute o código para ver o seu funcionamento melhor.

```JavaScript
	function sleepBlock( sleepDuration ){
		var now = new Date().getTime();
		while(new Date().getTime() < now + sleepDuration){  } 
	}

	console.log("Vou dormir por 3s!");
	sleepBlock(3000);
	console.log("Yawn... Acordei depois de 3s! :)");
```

Agora veja como é com a nossa versão original e ***não bloqueante*** de ***sleep***:

```JavaScript
	const sleep = (milliseconds) => {
		return new Promise(resolve =>{
			setTimeout(resolve, milliseconds);
		});
	}

	console.log("Vou dormir por 3 segundos!");
	sleepFor(3000);
	console.log("Mas já? :(");
```

Acredito que depois de ver na prática a diferença vai dar pra enter melhor o que é cada uma dessas coisas.

Quando um código tem ***aspecto bloqueante***, ele bloqueia a *thread* principal de execução, o que significa que uma coisa só será executada depois que a anterior terminar, e nunca de outro modo. Eles são bem comportados (e meio burros) e sempre irão esperar o código anterior. Por isso que quando chamamos ***sleepBlock(3000)*** , um loop que não faz nada por 3 segundos bloqueia a thread, impedindo que qualquer coisa que venha depois dele seja executada.

Quando um código tem ***aspecto não bloqueante***, apenas as operações que dependem exclusivamente de uma ação que demore tempo irão esperar, enquanto que as demais que independem dessa ação são executadas assim que possível.

Não deu pra entender ainda? Tudo bem, não se preocupe, esse assunto é relativamente confuso pra quem está entrando no barco assíncrono pela primeira vez (foi confuso pra mim também, já estava clandestino nesse barco por *muito* tempo até entender de verdade). Acredito que esse exemplo seguinte pode esclarecer a sua mente.

Pense na sua rotina de manhã antes de vir à faculdade. Você acorda, toma banho, toma café da manhã, escova os dentes, assiste um pouco do noticiário local para ver o trânsito, passa o fio dental e, por fim, passa o enxaguante bucal.
Se a sua rotina é diferente dessa de exemplo, **eu não ligo** (amo vocês ainda viu <3), finja que é para entender esse exemplo.

Se colocarmos essa rotina em linhas de código, podemos fazer dessa forma:

***obs:*** Os código abaixo são feios e ficam piores ainda no GitHub ou GitHub pages, recomendo que você copie eles e cole em um editor de texto à sua preferência para visualizá-los melhor.
Recomendo o Visual Studio Code, o Sublime, o Brackets ou algum outro parecido.

Temos nossa função que representa uma ação que demora um pouco de tempo:

```JavaScript
	function acaoDemorada(acao, duracao){
		console.log(acao);
		return new Promise(resolve =>{
			setTimeout(resolve, duracao)
		});
	}
```

E por fim, nossa rotina:

```JavaScript
console.log("Acordei!");
acaoDemorada("Tomando banho...", 3000).then(
	()=> {
	acaoDemorada("Tomando café...", 5000).then(
		()=>{
		acaoDemorada("Escovando dentes", 3000).then(
			()=>{
			acaoDemorada("Assistindo noticiário local...", 1000).then(
				()=>{
				acaoDemorada("Passando fio dental...", 2000).then(
					()=>{
					acaoDemorada("Passando enxaguante bucal...");
									}
								);
							}	
						);
					}
				);
			}
		);
	}
);
```

Ignorem o ***Callback Hell***, foquem na execução da função.

Vocês podem ver que cada função é executada apenas depois que a anterior termina.

Mas quando se está atrasado, você aprende a ser multi-tarefa. Vamos supor que você consegue assistir o noticiário enquanto está escovando os dentes, porém, todas as outras ações dependem das outras, veja:

* Você precisa acordar antes de tomar banho
* Você precisa tomar banho antes de comer
* Você precisa comer antes de escovar os dentes
* Você precisa escovar os dentes antes de passar o fio dental
* Você precisa passar o fio dental antes de passar o enxaguante bucal
* Você precisa apenas comer para poder ver o noticiário local

Dessa forma, vemos que assistir o noticiário local não depende que você tenha escovado os dentes, só depende que você tenha comido antes. Logo, assim que você termina de comer, pode assistir o noticiário. Inclusive enquanto escova os dentes. 
Foi modificado o código, logo, execute-o e veja a diferença:

```JavaScript
console.log("Acordei!");
acaoDemorada("Tomando banho...", 3000).then(
	()=> {
		acaoDemorada("Tomando café...", 5000).then(
			()=>{
				acaoDemorada("Assistindo noticiário local...", 1000);
				acaoDemorada("Escovando dentes", 3000).then(
					()=>{
						acaoDemorada("Passando fio dental...", 2000).then(
							()=>{
								acaoDemorada("Passando enxaguante bucal...");
							}
						);
					}
				);
			}
		);
	}
);
```

Perceba que ele não espera mais você terminar de escovar os dentes para assistir o noticiário. Isso é feito de maneira automática.

Agora, para finalizar, imagina que após você assistir o noticiário, você consegue postar um meme do ZAP. Essa ação independe das outras, apenas depende de assistir ao noticiário. Podemos adequar nosso código da seguinte forma:

```JavaScript
console.log("Acordei!");
acaoDemorada("Tomando banho...", 3000).then(
	()=> {
		acaoDemorada("Tomando café...", 5000).then(
			()=>{
				acaoDemorada("Assistindo noticiário local...", 1000).then(
					()=>{
						acaoDemorada("POSTANDO MEME NO ZAP");
					}
				);
				acaoDemorada("Escovando dentes", 3000).then(
					()=>{
						acaoDemorada("Passando fio dental...", 2000).then(
							()=>{
								acaoDemorada("Passando enxaguante bucal...");
							}
						);
					}
				);
			}
		);
	}
);
```

Execute o código acima e veja seu funconamento. Esse é um exemplo de código que possue partes de aspecto ***não bloqueante***, ou seja, funções e operações que independem de outras que levam tempo para serem feitas.

Caso ainda não tenha ficado claro, vale a pena a leitura do link abaixo:

[Bloqueante vs Não Bloqueante em NodeJS](https://dzone.com/articles/blocking-vs-non-blocking) - DZone
<br/>
*Tempo médio de leitura: 3 minutos*

[Visão geral de Bloqueante vs Não Boqueante](https://nodejs.org/de/docs/guides/blocking-vs-non-blocking/) - NodeJS
<br/>
*Tempo médio de leitura: 5 minutos*


Logo, temos como objetivo desse módulo, programar de modo ***não bloqueante***.


Tudo bem, voltando ao assunto de ***Async Functions***, sabemos que uma função assíncrona sabe esperar, então, vamos tentar executar o código abaixo e ver oque está acontecendo. Nosso objetivo é recriar o funcionamento de quando tinhamos a função ***sleepBlock***, ou seja, que ele só acorde depois de 3 segundos:

```JavaScript

	const sleep = (milliseconds) => {
		return new Promise(resolve =>{
			setTimeout(resolve, milliseconds);
		});
	}


	async function vamosDormirUmPouco(){
		console.log("Vou dormir por 3 segundos");
		sleep(3000);
		console.log("Acordei!");
	}

	vamosDormirUmPouco();
```

Execute o código e veja com os seus próprios olhos o que está acontecendo.

Não é o que estávamos esperando certo? Vocês devem estar dizendo "*Você mentiu pra mim! Disse que a função assícrona sabe esperar!*".

Mas na verdade, eu não menti. A função assíncrona **sabe** esperar. Só faltou dizermos a ela ***o que ela deve esperar***. Para isso, basta dizermos à ela com a palavra ***await***:

```JavaScript

	const sleep = (milliseconds) => {
		return new Promise(resolve =>{
			setTimeout(resolve, milliseconds);
		});
	}


	async function vamosDormirUmPouco(){
		console.log("Vou dormir por 3 segundos");
		await sleep(3000);
		console.log("Acordei!");
	}

	vamosDormirUmPouco();
```

Agora, a função sabe que precisa esperar que ***sleep*** seja retornada para que ela continue o resto do seu código.

Vale lembrar que apenas ***Async Functions*** estão preparadas para usar o ***await***. Não vá usando ele em qualquer função pois pode não funcionar!

Bom, agora que você sabe como as funções assíncronas funcionam, está na hora de descobrir como podemos fugir do Callback Hell usando elas como nossas salvadoras. Mas isso seria muito chato se eu simplesmente colocasse mais um código com a resposta para isso abaixo, então, coloque a cabeça para funcionar e use seus conhecimentos para resolver o ***EXERCÍCIO PROPOSTO*** abaixo:

***Exercício Proposto(5):***

Sua tarefa é transformar o último código do exemplo de bloqueantes (a rotina) usando os mais recentes conhecimentos adquiridos com esse módulo, ou seja, as ***Async Functions***.
Use o ***await*** para fazer com que algumas ações esperem outras.

***obs:*** Você não precisa utilizar a função "acaoDemorada" nesse exercício. Um *console.log* já quebra esse galho

***dica:*** Como essas ações tem uma dependência própria, tente usar uma função assíncrona para as ações de noticiário e de meme no ZAP e chamá-las na função assíncrona principal.

O exercício será considerado correto se ele reproduzir o último exemplo de rotina.

Código completo do exemplo:
```JavaScript
function acaoDemorada(acao, duracao){
	console.log(acao);
	return new Promise(resolve =>{
		setTimeout(resolve, duracao)
	});
}

console.log("Acordei!");
acaoDemorada("Tomando banho...", 3000).then(
	()=> {
		acaoDemorada("Tomando café...", 5000).then(
			()=>{
				acaoDemorada("Assistindo noticiário local...", 1000).then(
					()=>{
						acaoDemorada("POSTANDO MEME NO ZAP");
					}
				);
				acaoDemorada("Escovando dentes", 3000).then(
					()=>{
						acaoDemorada("Passando fio dental...", 2000).then(
							()=>{
								acaoDemorada("Passando enxaguante bucal...");
							}
						);
					}
				);
			}
		);
	}
);
```