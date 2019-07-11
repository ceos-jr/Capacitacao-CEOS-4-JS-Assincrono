# Promises

Tudo bem, eu *prometo* que esse assunto não vai ser tão chato quando o anterior ( *piada_escrota.exe* ). Vimos no tópico anterior os problemas que podem ocorrer ao se trabalhar com ***Callbacks*** por si só, porém, nesse tópico, iremos explorar uma técnica que ao usada em conjunto com os ***Callbacks*** pode trazer um novo nível de organização ao nosso código. Chega de ***Callback Hell***. Isso não é uma ameaça, é uma *promessa* ( parei, agora é sério ).

Para quem não entendeu essas piadas maravilhosas até agora, nós iremos falar das Promises ( promessas em português ). Uma Promise é um objeto utilizado para ***processamento assíncrono*** e ela representa um valor que pode estar disponível agora, daqui a algum tempo, ou até mesmo, *nunca*.

Para quem tiver mais curiosidade sobre conceitos sobre promises e seu funcionamento mais profundo no JavaScript, vale a pena a leitura da página da MDN a respeito:

[Promises](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Promise) - MDN
*Tempo médio de leitura: 5 minutos*

Uma Promise tem o trabalho de executar uma função, contanto que essa função tenha algumas características desejáveis:

* Essa função é uma função que depende de um fator externo que **demore tempo**, afinal, se você quer tratar quando algo dá ou não certo e não tem nada a ver com tempo, você não deveria estar usando uma promise.
<br/>

* Essa função sabe o que fazer quando tudo dá certo, logo, essa função sabe o que fazer quando esta se *resolve*, logo ela aceita uma função que é executada chamada de ***resolve*** que faz o que está previsto quando tudo dá certo
<br/>

* Essa função sabe que algo pode dar errado durante sua execução (ex: conexão rejeitada por um firewall), logo ela deve estar preparada quando algo é *rejeitado*, portanto ela aceita uma função que é executada quando isso ocorre, chamada de ***reject***

Elas podem parecer funções (até por que eu chamei elas de funções, e tecnicamente eu não estou errado), mas na verdade ***resolve*** e ***reject*** são métodos de uma Promise.

E logicamente, uma Promise não é criada usando uma função qualquer, nada disso. Promises são criadas com o construtor de sua própria classe, assim sendo, para criarmos uma Promise:

```JavaScript
	const minhaPromessa = new Promise((resolve, reject) => {
		if(ALGODEUCERTO)
			resolve("Tá tudo bem :)");
		else
			reject("MEU DEUS NÃO TÁ TUDO BEM :(  :C  :O  :,(");
	});
```

O exemplo acima serve apenas para ilustrar mesmo como que uma Promise é construída, vamos desmembrar um pouco sobre o que cada uma dessas coisas significa:

* **new Promise**: O construtor da Promise.
<br/>

* **( ) =>**: Arrow function que será passada para a Promise.
<br/>

* **(resolve, reject)**: Métodos que serão passados para a função que a Promise executará. Desse modo a função que a Promise executa pode acessálos.
<br/>

* **resolve("...")**: Indica que tudo deu certo e manda uma mensagem ao usuário para indicar isso.
<br/>

* **reject("...")**: Indica que algo deu errado e manda uma mensagem ao usuário para indicar isso

Ou seja, a **Promise** é criada e a ela é passada uma **função** que será executada ( ou pelo menos tentará ). A Promise manda para essa função dois de seus métodos: **resolve** e **reject**. Desse modo, a função poderá notificar a nós sempre que algo der certo ou não, ela faz isso simplesmente chamando os métodos **resolve** e **reject** nos momentos oportunos para indicar isso.

## resolve & reject

Podemos perceber que as situações em que chamamos ***resolve*** e ***reject*** são relativamente diferentes, mas no final, eles são a mesma coisa: **uma resposta**.

Os métodos que são passados como parâmetro durante a criação das promisses irão retornar uma mensagem que indica o que foi feito e se isso deu certo ou não, mas, muitas vezes, apenas mandar a mensagem não é claro o suficiente, logo, geralmente se manda um objeto contendo dois campos:

* **code**: Código identificador da resposta
* **message**: Mensagem descritiva da resposta

***obs:*** Vale lembrar que tanto ***resolve*** quando ***reject*** aceitam apenas um parâmetro, de modo que se você quiser passar mais de um valor (como estamos fazendo acima) você precisará englobar os valores que serão passados em um Objeto.

Desse modo, podemos descrever e debugar muito melhor as nossas respostas em uma aplicação com nível de processamento assíncrono. Um exemplo de como podemos utilizar esse objeto de resposta pode ser visto abaixo:

```JavaScript
	const minhaPromessa = new Promise((resolve, reject) => {
		if(ALGODEUCERTO){
			resolve({
				code: 'aok',
				message: "Está tudo bem :)"
			});
		}
		else{
			reject({
					code: 'nok',
					message: "MEU DEUS NÃO TÁ TUDO BEM :(  :C  :O  :,("
				});
		}
	});
```

Tudo bem, a partir disso você já deve estar se perguntando: 

*Então basta eu jogar uma promisse e depois acessar o valor do código pra saber se deu certo ou não?*

Isso não é o suficiente, sabe, uma vez que nossa Promise só possui a funçao de executar o código, não cabe a ela nenhuma responsabilidade a respeito do que fazer com o resultado de suas ações. Para isso, usaremos algo que irá **consumir** a nossa Promise. Essa coisa que irá consumir a Promise como o próprio nome sugere, será o nosso ***consumer***, e um consumer a priore pode ser 3 coisas:

* ***then***
* ***catch***
* ***finally***

Cada um deles com sua especialidade e responsabilidade. Vamos ver um pouco sobre cada um deles e então entender como podemos manipular as nossas Promises.

### then:
O ***then*** (*então* em português) faz realmente o que ele parece fazer, ele executa o código assim que a sua Promise retornar.

O ***then*** é muito útil quando precisamos fazer algo imediatamente após uma Promise nos retornar algum valor.

Imagine esse caso de aplicação, você precisa mostrar ao usuário um dado que você recebe de um banco de dados. Você recebe esse dado e então manda um *console.log* ou um *alert* ou qualquer outra coisa, e tudo que o usuário vê é:

```
	undefined
```
ou então
```
	null
```
talvez até um
```
	NaN
```
esse eu já até enjoei de ver
```
	[Object][object]
```

Isso por que você não conseguiu com que a sua aplicação esperasse que a Promise se resolvesse. Porém, agora que você conhece o ***then***, você pode fazer isso:

Inicialmente você tinha esse problema:
```JavaScript
	let meuresultado
	const pegandoDados = new Promise((resolve, reject)=>{ setTimeout(()=>{
		meuresultado = Math.floor(Math.random()*1000)
	}, 2000)});

	console.log(meuresultado);

```

E recebia essa linda mensagem no seu terminal:

```
	undefined

```

Vamos ver então como podemos resolver esse problema do jeito certo:

```JavaScript
	let meuresultado

	const pegandoDados = new Promise((resolve, reject)=>{ setTimeout(()=>{
		meuresultado = Math.floor(Math.random()*1000)
		resolve({
			code: "ok",
			message: "O resultado foi recebido com sucesso"
		});
	},1000)});

	pegandoDados.then(
		resolve => console.log(`${resolve.message}: ${meuresultado}`)
	);

```

Desse modo, estaremos esperando que os dados estejam prontos. Vale lembrar que ***then*** é um método padrão para toda Promise, por isso podemos acessá-lo, assim como os campos que passamos para o ***resolve***, sendo eles ***code*** e ***message***.

***obs***: Logicamente estamos usando ***setTimeout*** para simular uma requisição ao banco de dados e ***Math.random*** para simular a geração de um dado.


Vimos como podemos simular uma Promise assíncrona com ***setTimeout*** e como podemos utilizar seu dado depois dela se cumprir. Porém, fica a seu cargo como podemos utilizar o reject para sinalizar que algo deu errado, e isso vai ser cobrado com um ***EXERCÍCIO PROPOSTO***

<br/>
<br/>
<br/>

***Exercício Proposto (3):***

Use o código abaixo (espelhamento do código acima) para se basear durante esse exercício:

```JavaScript
	let meuresultado

	const pegandoDados = new Promise((resolve, reject)=>{ setTimeout(()=>{
		meuresultado = Math.floor(Math.random()*1000)
		resolve("O resultado foi recebido com sucesso");
	},1000)});

	pegandoDados.then(
		resolve => console.log(`${resolve}: ${meuresultado}`)
	);

```

Sua tarefa é supor que sempre que a variável ***meuresultado*** tiver um valor inferior à 300 quando seu valor for gerado. Assim sendo, trate o caso onde isso acontece usando o ***reject***.

O seu reject deverá notificar ao usuário que um erro ocorreu devido ao baixo valor de ***meuresultado*** e após isso, notificar também o valor que foi recebido.

***dica:*** O método padrão das Promises ***then*** recebe dois argumentos, sendo eles duas funções ***callback***. Um para o ***resolve*** e outro para o ***reject***.

Envie o código contendo a solução para o exercício proposto acima no repositório correspondente após a sua conclusão.

<br/>
<br/>
<br/>

### catch

Se o nosso objetivo é tratar erros de uma Promise, uma das opções é passar uma função tratadora de erros no segundo argumento do nosso ***then***. A outra que também pode ser usada, usa uma coisa que já vimos antes no módulo de JavaScript, o ***catch***.

Não confunda o ***catch*** com o ***catch*** de um ***try ... catch ... finally***, embora suas funcionalidades sejam parecidas. O ***catch*** do qual estamos falando é, na verdade, um método da Promise (***MUITO*** semelhante ao ***then***).

Para usá-lo, podemos simplesmente fazer algo desse jeito:


```JavaScript

	const minhaPromise = new Promise((resolve, reject)=>{ 
		...
		reject(...);
	);

	minhaPromise.catch( reject => console.log("Opa, algo deu errado!") );

```

Vimos que sua sintaxe é extremamente parecida com a nossa sintaxe do ***then***...

Mas, na verdade, ***catch*** e ***then*** são ***a mesma coisa***.

O ***catch*** é apenas uma outra forma de escrever:


```JavaScript

	minhaPromise.then(null, reject => console.log("Opa, algo deu errado!") );

```

Que nos serve muito bem quando precisamos tratar apenas os erros de uma Promise e não precisamos fazer nada imediatamente após ela, já que estamos passando ***null*** como o primeiro argumento do método ***then***.

### finally

E *finalmente* ( foi mal, foi mal ), temos o nosso método ***finally*** que vai conter uma função que será executada **independentemente** da nossa Promise ser resolvida ou rejeitada:


```JavaScript

	minhaPromise.finally(
		() => FAZ_ALGUMA_COISA_INDEPENDENTE_DO_RESULTADO
	);
```

Podemos inclusive combinar o nosso ***finally*** com o ***then***, como vemos no exemplo abaixo:

```JavaScript

	minhaPromise.finally(
		() => FAZ_ALGUMA_COISA_INDEPENDENTE_DO_RESULTADO
	).then(
		resolve => QUANDO_ALGO_DA_CERTO,
		reject => QUANDO_ALGO_DA_ERRADO
	);

```

Bem parecido com o ***then*** de novo não é mesmo? Tão parecido que, novamente, ***finally*** é apenas uma outra forma de escrever:

```JavaScript

	minhaPromise.then(
		resolve => FAZ_ALGUMA_COISA_INDEPENDENTE_DO_RESULTADO,
		reject => FAZ_ALGUMA_COISA_INDEPENDENTE_DO_RESULTADO,
		
	)
```

Ou seja, independente de ser um ***resolve*** ou ***reject***, iremos fazer a mesma coisa após a Promise retornar.

Logo, podemos escrever analogamente o primeiro código dessa forma:

```JavaScript

	minhaPromise.then(
		resolve => FAZ_ALGUMA_COISA_INDEPENDENTE_DO_RESULTADO,
		reject => FAZ_ALGUMA_COISA_INDEPENDENTE_DO_RESULTADO,
		
	).then(
		resolve => QUANDO_ALGO_DA_CERTO,
		reject => QUANDO_ALGO_DA_ERRADO
	);

```
<br/>

Porém, vocês concordam que fica relativamente estranho não é mesmo?

Realmente fica utilizando apenas  ***then***, pois ***finally*** está explicitamente dizendo à você, programador, que a função passada para ele será executada independentemente do resultado. Assim como ***catch*** indica que você só está interessado em tratar erros.

Esse significado semântico é tão importante quanto o significado semântico que estudamos nos workshops de ***HTML5***.

Depois de ver tudo isso nas nossas Promises, acredito que você tenha conseguido entender bem sobre elas, como elas funcionam e como podemos trabalhar com elas. Caso contrário, recomendo a leitura do link abaixo:

[O básico de Promises](https://javascript.info/promise-basics) - JavaScript Info<br/>
*Tempo médio de leitura: 12 minutos*

E finalmente, não podemos encerrar esse módulo sem mais um ***EXERCÍCIO PROPOSTO***

***Exercício Proposto (4):***

Reescreva o código do exercício proposto anterior fazendo as seguintes mudanças:

* Ao invés de ser um número menor que 300, um número será inválido se este for ***menor que 100, maior que 900, primo ou múltiplo de 7, 13, 21 ou 23***
<br/>

* Use um ***catch*** para tratar esses erros
<br/>

* O catch deverá imprimir na tela usando um *console.log* o tipo do erro (Múltiplo de 7, Menor que 100, etc). Basta um tipo (não precisa imprimir ex: Múltiplo de 7 e Menor que 100).
<br/>

* Dessa vez, mande para o reject um objeto do tipo ***Error***, você já deve ter visto ele no módulo de JavaScript, caso não tenha, eis sua documentação: [Documentação Objeto Error](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Error) - MDN

<br/>

* Após isso, você deverá imprimir na tela usando *console.log* o número que foi gerado, **independente** se foi um erro ou não.

<br/>

* Utilize para o tópico acima, um ***finally***

***Código do exercício anterior:***
```JavaScript
	let meuresultado

	const pegandoDados = new Promise((resolve, reject)=>{ setTimeout(()=>{
		meuresultado = Math.floor(Math.random()*1000)
		resolve("O resultado foi recebido com sucesso");
	},1000)});

	pegandoDados.then(
		resolve => console.log(`${resolve}: ${meuresultado}`)
	);

```
