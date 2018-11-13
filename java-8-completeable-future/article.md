# Java 8: CompleteableFuture & CompleteableStage.

Depois de um longo tempo sem escrever (faltou inspiração) resolvi tentar voltar a escrever. Vamos ver no que dará essa nova empreitada.

Com a chegada do [Java 11](https://openjdk.java.net/projects/jdk/11/) e o [Java 12](https://openjdk.java.net/projects/jdk/12/) já no forno
acho que é importante utilizarmos algumas carecterísticas que foram lançados no já ofuscado [Java 8](https://openjdk.java.net/projects/jdk8/).

Com o **Java 8** tivemos algumas novidades e entre elas as *[expressões lambda](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)*. As 
*expressões lambdas* eu particularmente considero bastante úteis para a programação paralela. Explicando rapidamente, uma *expressão lambda* é um jeito fácil de escrever funções e as funções são úteis para a programação paralela pois a cada momento que você chama uma função você fornece uma entrada e espera um resultado. Se a função não faz uso de recursos compartilhados, ou seja, para cada entrada ocorre uma computação *fechada* e dessa computação há um resultado então você pode ver que não há problema em executar a mesma função com várias entradas ao mesmo tempo, vamos dar um exemplo bem simples. Imagine que você quer calcular a raiz quadrada de cada um dos elementos de uma lista de números, percebe que não há problema em fazer esses cálculos paralelamente? Infelizmente não é tão simples assim, há umas questões sobre imutabilidade mas esses problemas pretendo explicar ao longo do texto, mas não se preocupe não é algo muito complexo.

## CompleteableFuture é um CompletableStage.

Para escrever esse artigo usarei apenas o [javadoc](https://docs.oracle.com/javase/8/docs/api/) como referência. Falei de *expressão lambda* e ainda não falei do *CompleteableFuture* e *CompleteableStage*, vamos imaginar um problema mais real. Imagine que você precisa resolver um problema em três etapas, as duas etapas iniciais podem ser resolvidas em paralelo e a última etapa só pode ser resolvida após as duas primeiras. Pensando em termos matemáticos digamos que temos três funções:

```
f: A ⇒ B, b = f(a)

g: C ⇒ D, d = g(c)

h: (B, D) ⇒ Z, z = h(b, d)

```
