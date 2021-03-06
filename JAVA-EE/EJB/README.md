# EJB

- [Módulo Maven](#módulo-maven)
- [Empacotamento em WAR e EAR](#empacotamento-em-war-e-ear)
  - [Como uma biblioteca no WAR](#como-uma-biblioteca-no-war)
  - [Como um módulo no EAR](#como-um-módulo-no-ear)
  - [Implementações dentro do WAR](#implementações-dentro-do-war)
- [Exemplos de implementação](#exemplos-de-implementação)
  - [3.X](#3x)
    - [Com necessidade de expor ejb remoto](#com-necessidade-de-expor-ejb-remoto)
    - [Sem necessidade de export ejb remoto](#com-necessidade-de-expor-ejb-remoto)
    - [Gerando JAR client](#gerando-jar-client)
  - [2.X](#2x)
- [Sessões de Bean](#sessões-de-bean)
  - [Stateless](#stateless)
  - [Stateful](#stateful)
  - [Singleton](#singleton)
  

### Módulo Maven

Um módulo EJB nada mais nada menos é que um artefato JAR que possui o código fonte da lógica de negócio de uma apliação implementadas de forma que seguem a especificação do EJB variando na presença de alguns arquivos dependendo da versão da especificação. 

Na versão 2.X alguns arquivos XML de configuração serão encontrados, já na versão 3.X as anotações nas classes podem ser encontradas substituindo a necessidade de configuração por XML. 

Sendo assim um módulo EJB pode ser definido com o Maven de duas formas de empacotamento: jar e ejb.

```
...
<packaging>jar</packaging>
...
```
```
...
<packaging>ejb</packaging>
...
```

No primeiro exemplo o código fonte simplesmente será empacotando conforme um JAR convencional, da mesma forma das bibliotecas de teceiros. 

Já no segundo exemplo o plugin [maven-ejb-plugin](http://maven.apache.org/plugins/maven-ejb-plugin/index.html) será automaticamente considerado pelo Maven realizando algumas validações, como a presença do arquivo `ejb-jar.xml` que é um arquivo obrigatório na versão 2.X do EJB conforme mencionado em http://maven.apache.org/plugins/maven-ejb-plugin/usage.html: *"The plugin doesn't do any EJB specific processing during the generation of the jar except for validating the existence of an EJB deployment descriptor if the EJB version is 2.0+ "*. Algumas ações também poderão ser configuradas para serem executadas durante a fase de geração do artefato JAR como por exemplo a geração das classes de interface que serão utilizadas pelas aplicações clientes no caso de disponibilização remoto.

Sendo assim caso a implementação siga a versão da espeficação do EJB 3.X+ com a utilização de anotações ao invés de arquivos XML e não havendo a necessidade de disponibilização de interfaces remotas em um artefato JAR separado, não há necessidade de se utilizar o segundo exemplo.

### Empacotamento em WAR e EAR

O empocotamento de implementações de EJB podem ser realizado de três maneiras distintas de acordo com a necessidade:

- [Como uma biblioteca no WAR](#como-uma-biblioteca-no-war)
- [Como um módulo no EAR](#como-um-módulo-no-ear)
- [Implementações dentro do WAR](#implementações-dentro-do-war)

#### Como uma biblioteca no WAR

No caso de um WAR a biblioteca do EJB será tratada como uma biblioteca comum que será incluída no diretório `WEB-INF/lib`. No caso da implementação da especificação EJB 2.X na presença de descritores de implemetação `ejb-jar.xml` o mesmo será  ignorado obrigando que seja definido um novo `ejb-jar.xml` no diretório `WEB-INF`. 

Na especificação EJB 3.X ou superior como não existe a necessidade de descritores de implementação por conta da utilização de anotações, nenhuma configuração a mais é necessária, pois o container EJB irá realizar o "escaneamento" das classes automaticamente.

#### Como um módulo no EAR

No caso de um EAR o módulo EJB deverá estar definido como um módulo filho gerado como artefato JAR na raíz do diretório e especificado no arquivo `META-INF/application.xml` semelhantemente a um WAR. Ao contrário do WAR neste caso o container EJB irá dectectar, se for o caso, a presença do descritor de implementação `ejb-jar.xml` dentro do artefato JAR do módulo EJB.

#### Implementações dentro do WAR

Um módulo WAR pode ter alguns códigos de bean colocados livremente na estrutura de diretório `WEB-INF/classes` e outros códigos de bean dentro de arquivos JAR no diretório `WEB-INF/lib`. Isso também é válido para um modulo WAR para que todo o código de bean esteja na estrutura de diretório `WEB-INF/classes` e nada no diretório `WEB-INF/lib` ou para que todo o código de bean esteja nos arquivos JAR no diretório `WEB-INF/lib` e nada no `WEB-INF/classes`.`

Indendentemente de estar colocado dentro da estrutura de diretório `WEB-INF/classes` ou estar dentro do diretório `WEB-INF/lib`, no caso da especificação EJB 2.X é obrigatório que o descritor de implementação seja declarado para os beans de ambos os casos, mesmo que o artefato JAR dentro do `WEB-INF/lib` já o declare, conforme explicado em [Módulo EJB como uma biblioteca no WAR](#como-uma-biblioteca-no-war).

### Exemplos de implementação

#### 3.X

##### Com necessidade de expor EJB Remoto

Uma interface genérica que servirá tanto para utilização local quanto remoto:

```
public interface HelloWorld {
    String sayHello();
}
```

Uma interface para utilização remoto:

```
public interface HelloWorldRemote extends HelloWorld {}
```

Uma classe que implementa a interface genérica com a lógica de negócio. A partir da versão 3.1 foi adicionado a anotação `@LocalBean` que dispensa a necessidade de criação de uma interface com a anotação `@Local`. Com a utilização da anotação `@LocalBean` a classe deverá ser injetada utilizando a anotação `@EJB`. Caso seja a versão anterior a 3.1, é preciso criar também uma interface para o uso local com a anotação `@Local` e injeção da classe se dará através da anotação `@Inject`. 

```
@Stateless
@Remote(HelloWorldRemote.class)
@LocalBean
public class HelloWorldBean implements HelloWorld {

    public String sayHello() {
        return "Hello";
    }

}
```

##### Sem necessidade de expor EJB remoto

No caso de se utilizar EJB e não houver necessidade de expor remotamente algum método, não é preciso criar uma interface genérica e a interface remota. Apenas criar a classe com a lógica de negócio e anotá-la com: `@Stateless` ou `@Stateful` ou `@Singleton`. A injeção desta classe sem interface local sempre será através de `@EJB`.

```
@Stateless
public class HelloWorldBean {

    public String sayHello() {
        return "Hello";
    }

}
```

---

Se tudo for configurado com sucesso será possível visualizar na log de inicialização do servidor o nome JNDI das inaterfaces local e remote conforme exemplo abaixo:

Local

```
CNTR0167I: O servidor está ligando a interface br.com.playground.ejb.HelloWorldLocal do enterprise bean HelloWorldBean no módulo ejb-1.0-SNAPSHOT.jar do aplicativo ear_ear_exploded.  O local de ligação é: java:global/ear-1.0-SNAPSHOT/ejb-1.0-SNAPSHOT/HelloWorldBean!br.com.playground.ejb.HelloWorldLocal
```

Remote

```
CNTR0167I: O servidor está ligando a interface br.com.playground.ejb.HelloWorldRemote do enterprise bean HelloWorldBean no módulo ejb-1.0-SNAPSHOT.jar do aplicativo ear_ear_exploded.  O local de ligação é: java:global/ear-1.0-SNAPSHOT/ejb-1.0-SNAPSHOT/HelloWorldBean!br.com.playground.ejb.HelloWorldRemote
```

Aplicações que residem na mesma JVM podem realizar lookup da interface local através do exemplo de código abaixo desde que possuam o jar client ejb:

```
InitialContext ic = new InitialContext();
HelloWorldLocal helloWorld = (HelloWorldLocal) ic.lookup("java:global/ear-1.0-SNAPSHOT/ejb-1.0-SNAPSHOT/HelloWorldBean!br.com.playground.ejb.HelloWorldLocal");
```

É possível também configurar uma referência de recurso EJB no web.xml da aplicação web para que o lookup possa ser realizado através do JNDI `java:comp/env/`.

### Gerando JAR Client

Uma vez configurado os beans EJB locais ou remotos, é preciso prover um artefato JAR com as classes JAVA das interfaces locais e remotas para que as aplicações clientes que irão consumir os métodos possam realizar as chamadas. Para isto na aplicação onde os beans EJB foram implementados adicionar o seguinte plugin no pom.xml:

```
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-ejb-plugin</artifactId>
  <version>2.2.1</version>
  <configuration>
      <ejbVersion>3.2</ejbVersion>
      <generateClient>true</generateClient>
  </configuration>
</plugin>
```
Após o build da aplicação via maven, será gerado um artefato JAR na pasta target da aplicação. Este JAR deve ser fornecido para que as aplicações que irão consumir os métodos incluam o mesmo no seu ClassPath.

### Sessões de Bean

#### Stateless

Bean instanciado a cada requisição de cliente, ou seja, quando um cliente invoca método do bean, as variáveis do bean manterão estado somente durante a invocação do método. Quando o método for finalizado, o estado associado ao cliente não é mantido.

#### Stateful

Bean instanciado e associado a um único cliente, ou seja, para cada cliente que realiza requisições uma nova instância do bean é criada e o estado deste bean é mantido entre invocações de métodos pois está associado a um único cliente.

#### Singleton

Um bean de sessão que é instanciado uma única vez por aplicação e pelo tempo de vida da aplicação. São utilizados em circunstâncias em que uma única instância do bean pode ser compartilhado entre múltiplos clientes concorrentes.

