# Callbacks

Você já tem um conhecimento adequado sobre funções para estar situado neste módulo. Apenas para refrescar sua memória, vamos relembrar que:

 - Uma função realiza algum tipo de ação
 - Uma função pode receber um ou mais argumentos
 - Os argumentos de uma função podem ser inclusive **outras funções**
  
  Este último tópico será de extrema importância para nós durante esse módulo. Mas primeiro, vamos relembrar esse aspecto das funções poderem receber outras funções com um **EXERCÍCIO SUGERIDO**:
<br/>
<br/>
<br/>
**Exercício sugerido:**

Crie uma função que receba dois número e outra função, e que tenha como finalidade, aplicar a função recebida nos dois outros números e imprimir o valor de retorno da função passada na tela através de um *console.log*.

***ex:***
```JavaScript
    const funcaoSoma = (n1, n2) => {
	    return n1 + n2
    }
    
	const funcaoSubtracao = (n1, n2) => {
	    return n1 - n2
    }
	
	funcaoDoExercicioSugerido(6,4,funcaoSoma);
	// Deve imprimir 10
	
	funcaoDoExercicioSugerido(6,4,funcaoSubtracao);
	// Deve imprimir 2
```
<br/>
<br/>
<br/>
Agora que você deu aquela bela refrescada na memória, podemos voltar a explicar o que é um Callback.

Um callback nada mais é que uma função que é passada como argumento para outra função, que será executada em um certo momento conveniente.
Pode parecer estranho e algo que você nunca viu, mas você **já trabalhou com Callbacks** e talvez nem tenha se tocado disso ainda.

Duvida? Pois bem, se lembra dos métodos ***map*** e ***filter*** do **Array** JavaScript? Se não se lembrar, recomendo que dê uma pequena lida na documentação desses dois métodos apenas para se situar:

[Documentação:  Método Map - Array - JavaScript](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array/map) - MDN <br/>
*Tempo médio de leitura: 8 minutos*

[Documentação:  Método Filter - Array - JavaScript](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array/filtro) - MDN <br/>
*Tempo médio de leitura: 5 minutos*

Percebeu uma coisa em comum? Esses dois métodos precisam de um argumento que é uma função que será executada para algum tipo de decisão ou processamento de dados do array. Essa função é o nosso **Callback**.

Desse modo, vemos que callbacks estão presentes em uma parte enorme da programação, principalmente em JavaScript. Porém, vamos dar uma olhada também em outras duas funções que usam Callback, as famosas ***setInterval*** e ***setTimeOut***.

<br/>
<br/>

## setTimeOut( Callback, milisegundos)
Espera um certo número de **milisegundos** e chama uma função de **Callback** após isso.

## setInterval( Callback, milisegundos)
A cada intervalo de tempo de **milisegundos** chama uma função de Callback

### clearTimeOut()
Método padrão de setInterval e setTimeOut que 'para' a execução desses métodos. Ou seja:
Em **setInterval**, ele interrompe a repetição de tempo em tempo.
Em **setTimeOut**, caso o intervalo de tempo não tenha sido atingido, a função não é mais executado.

<br/>
<br/>
<br/>

Tente aplicar os métodos listados acima com esse **EXERCÍCIO SUGERIDO**:
<br/>
<br/>
<br/>
**Exercício sugerido:**

Use um ***setInterval*** para imprimir na tela usando *console.log* a string "Hello World" a cada 1 segundo.
Use um ***setTimeOut*** para encerrar o intervalo mencionado acima após 5 segundos usando ***clearTimeOut***.

**Dica:** Para usar o clearTimeOut, siga o modelo do exemplo:

Caso ainda tenha dúvida a respeito do funcionamento do métodos, a leitura abaixo é sugerida:

[Funções de Timing - JavaScript](https://www.w3schools.com/js/js_timing.asp) W3Schools <br/>
*Tempo médio de leitura: 3 minutos*

***ex:***
```JavaScript
    const meuIntervalo = setInterval(..., ...);
	...
    clearInterval(meuIntervalo);
```
<br/>
<br/>
<br/>
Com o exercício sugerido acima, podemos ver diante de nós uma das formas que temos para lidar com o tempo no JavaScript com as funções mais simples para isso.
Mesmo esse tipo de aplicação é um pouco irreal ainda e pouco aplicado,setInterval e setTimeOut são muito úteis para entender um pouco sobre Callbacks, logo eles serão utilizados no decorrer deste módulo.

Tente dominar um pouco mais de como podemos flexibilizar nossos **Callbacks** usando funções anônimas com um **EXERCÍCIO PROPOSTO**:
<br/>
<br/>
<br/>
**Exercício proposto (1):**

Primeiramente execute o código abaixo para entender o seu funcionamento caso este não esteja muito claro ainda.
Sua tarefa é converter o código abaixo, de modo que você substitua a função ***imprimeMilissegundos*** por uma função anônima passada dentro do callback.

***obs:*** Caso não se lembre, uma função anônima é uma função que não possui nome na sua declaração.

***Código:***
```JavaScript
    function  imprimeMilissegundos(){
		console.log(new  Date().getTime())
	}
	setInterval(imprimeMilissegundos, 1000);
```
<br/>
<br/>

## Callbacks & Closures

Você já passou pelo módulo JavaScript básico, logo, você sabe como funcionam as closures. Se não lembra muito bem, a leitura abaixo é recomendada:

[Desmistificando Callbacks e Closures](https://www.sitepoint.com/demystifying-javascript-closures-callbacks-iifes/) - SitePoint <br/>
Tempo médio de leitura: 7 minutos

Para refrescar melhor tudo isso, recomendamos que você execute esse **CÓDIGO DE EXEMPLO**:

```JavaScript
    function  carregar(firstName, lastName, callback){
		console.log("Processando dados...");
		callback(firstName, lastName);
	}
	
	const  saudacao  = (nome, sobrenome) =>  setTimeout(
		()=>console.log(`Bem vindo senhor(a) ${nome}  ${sobrenome}`)
	, 1000);
	carregar("Vinícius", "Sabariz", saudacao);
```
<br/>
Recomenda-se que você execute e entenda tudo que está acontecendo nesse código. Brinque e mexa com as funções e desvende os funcionamentos dos **Callbacks**.

<br/>
<br/>

 ## O grande problema dos callbacks ( Callback Hell )
 Agora que você já viu como podemos usar callbacks para resolver vários tipos de problemas, vamos agora com um pequeno **CÓDIGO DE EXEMPLO**. Execute-o e veja o seu funcionamento:

```JavaScript
	console.log("Acessando banco de dados...");
	setTimeout(()=>{
		console.log("Processando dados de entrada...");
		setTimeout(()=>{
			console.log("Realizando operações com os dados...");
			setTimeout(()=>{
				console.log("Encerrando conexão com o banco de dados...");
				setTimeout(()=>{
					console.log("Encerrando programa...");
				} ,1000);
			}, 1000);
		} , 1000);
	} ,1000);
```
<br/>
Bonito demais esse código de exemplo não é mesmo? Eu sei que não, e muitas outras pessoas também sabem que não. Um código como esse que parece com um triangulo de lado é uma das principais características do problema que chamamos de ***Callback Hell***. 
Esse problema é causado pelo aninhamento de vários **Callbacks**, de modo que, mesmo que ele consiga resolver o seu problema, seu código fica mais poluído e de difícil manutenção. Muitas vezes não fica claro o que o seu código está querendo fazer.

 Recomendamos uma leitura a respeito desse problema para seu melhor entendimento:
 
[Evitando Callback Hell no Node JS](https://udgwebdev.com/evitando-callback-hell-no-node-js) - SitePoint <br/>
*Tempo médio de leitura: 7 minutos*

[Callback Hell](http://callbackhell.com) - Callback Hell <br/>
*Tempo médio de leitura: 10 minutos*
<br/>
<br/>
E finalmente, para finalizar esse tópico, fique com esse **EXERCÍCIO PROPOSTO**:
<br/>
<br/>
<br/>
**Exercício proposto (2):**

Sua tarefa é de ler o conteúdo de um arquivo de texto e escrevê-lo em outro arquivo de texto. Para isso utilize as funções do ***File System*** do Node cuja documentação completa segue abaixo:

[Documentação File System](https://nodejs.org/api/fs.html)

Vale lembrar que operações no disco levam tempo para serem executadas.

Utilize as funções de readFile e writeFile como mostradas abaixo para conseguir os dados corretamente.

***obs:*** Você **NÃO PODE** usar as funções readFileSync ou writeFileSync. Para este exercício, elas estão estritamente proibidas.

***obs:*** Você **NÃO PODE** usar o setTimeOut ou setInterval.

***Dica:*** Use Callbacks para não sofrer com dados não processados devido ao tempo. Lembre-se, pode parecer instantâneo, mas não é.

Segue abaixo o código que exemplifica o uso das funções listadas acima:

***Código:***
```JavaScript
    // Importação do FileSystem
	const fs = require("fs");
	
	// Função de leitura de arquivo
	fs.readFile("teste.txt", 'utf8', (err, data)=>{
		...
	});
	...
	
	// Função de escrita de arquivo
	fs.writeFile("destino.txt", DADOSAQUI, ()=>{
		...
	});
```
<br/>
<br/>
