# Relatórios com Jasper

JasperReports é a biblioteca de código aberto mais popular do mundo para geração de relatórios. É inteiramente escrita em Java e capaz de utilizar dados vindos de qualquer tipo de origem de dados e produzir documentos que podem ser visualizados, imprimidos ou exportas em uma variedade de formatos incluíndo HTML, PDF, Excel, OpenOffice e Word.

- [TIBCO Jaspersoft Studio](#TIBCO-Jaspersoft-Studio)
- [JRXML](#JRXML)
- [Build Path](#Build-Path)
- [Parâmetros](#Parâmetros)
- [Fields](#Fields)

## TIBCO Jaspersoft Studio

TIBCO Jaspersoft® Studio é uma ferramenta de edição para construção dos relatórios que serão gerados. A ferramenta ajuda no design de templates de relatórios; construção de queries de relatório; escrever expressões complexas; construções de componentes visuais de layout como mais de 50 tipos de gráficos, mapas, tabelas e componentes customizáveis; e muito mais.

Tudo que é construído nessa ferramenta se gera como produto um ou mais arquivos do tipo JRXML.

## JRXML

O [TIBCO Jaspersoft Studio](#TIBCO-Jaspersoft-Studio) define um relatório através de um arquivo XML. Um arquivo jrxml é composto de diversas seções; algumas relacionadas a características físicas do relatório (como a dimensão da página, o posicionamento dos campos e a altura das bandas), e outras relacionadas a características lógicas (como a declaração de parâmetros e variáveis e da definição da query para seleção de dados).

## Build Path

Durante a construção de um relatório utilizando a ferramenta [TIBCO Jaspersoft Studio](#TIBCO-Jaspersoft-Studio), você verá a necessidade de referenciar propriedades que estarão nas suas classes JAVA, provavelmente classes do tipo POJOs, ou então, utilizar alguma biblioteca de terceiro como o commons-lang3 da Apache por exemplo, para realizar alguma operação com texto, o qual você não quer re-implementar. 

Para que a ferramenta seja capaz de [compilar](#compilação) o seu relatório é preciso que essas classes referenciadas no seu relatório estejam no Class Path do projeto assim como uma aplicação JAVA qualquer. Para isso, clicar com o botão direito sobre o projeto que se está trabalhando no workspace do [TIBCO Jaspersoft Studio](#TIBCO-Jaspersoft-Studio), e acessar o item de menu "Properties" ou Alt+Enter. Na janela que se abrirá navegar para "Java Build Path" e em "Libraries" clicar em "Add External JARs" e adicionar as bibliotecas o qual serão utilizadas pelo seu relatório. Se for as classes JAVA referente a alguma aplicação Web, você pode simplesmente clicar em "Add External Class Folder" e apontar para o diretório `WEB-INF/classes` da sua aplicação.

## Parâmetros

Os parâmetros são formas de se passar dados para um relatório e referencia-los a fim de exibi-los. Isso se aplica também para passar dados de um relatório para um [sub-relatório](#Sub-relatório).

Por exemplo: imagine que no seu relatório você deseje exibir informações referente aos dados de uma pessoa. Para exibir as informações precisaremos referenciar o parâmetro que foi criado no relatório, o qual será alimentado externamente. Dessa pessoa iremos utilizar diversas referências como: `$P{pessoa}.getNome()`, `$P{pessoa}.getIdade()`. Como pode ser visto, a notação `$P{}` se refere a uma informação que foi passada via parâmetro. No caso exemplificado estamos referenciado o valor `pessoa` o qual é o nome dado ao parâmetro.

Para criar um parâmetro basta abrir seu arquivo `.jrxml` na ferramenta [TIBCO Jaspersoft Studio](#TIBCO-Jaspersoft-Studio) e na aba "Outline" no item "Parameters" do nome do seu relatório clicar em "Create Parameter". Será aberta outra aba onde será possível modificar o nome desse parâmetro e definir o seu tipo. O tipo pode ser desde classes nativas do Java, como os wrappers: String, Integer, Double, BigDecimal como classes customizadas, como POJOs por exemplo. Desde que essa classe customizada esteja presente no [Build Path](#Build-Path) do projeto.

## Fields

Um relatório é dividido em diversos setores como: Page Header, Column Header, Footer entre outros. Cada setor tem uma função específica. Por exemplo: o setor Page Header é exibido em todas as páginas caso o relatório exija uma nova página para exibir o conteúdo. 

Um relatório, na maioria dos casos, exibe diversos dados que são repetidos na página como uma lista. **A área no relatório responsável por renderizar a lista de dados é o setor "Detail**. Nesse setor, escrevemos uma expressão que irá renderizar os atributos que cada registro dessa lista possui. Para renderizar esses atributos são utilizados os "fields".

O setor "Detail" é populado através de um Data Source que deve ser fornecido ao relatório. Isso pode ser através de uma query SQL de banco de dados ou uma lista de objetos JAVA.

Para criar um field basta abrir o arquivo `.jrxml` na ferramenta [TIBCO Jaspersoft Studio](#TIBCO-Jaspersoft-Studio) e na aba "Outline" no item "Fields" do nome do seu relatório e clicar em "Create Field". Semelhante ao parâmetro, será aberta outra aba onde será possível modificar o nome desse parâmetro e definir o seu tipo. O tipo pode ser desde classes nativas do Java, como os wrappers: String, Integer, Double, BigDecimal como classes customizadas, como POJOs por exemplo. Desde que essa classe customizada esteja presente no [Build Path](#Build-Path) do projeto.
