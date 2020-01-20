# Relatórios com Jasper

JasperReports é a biblioteca de código aberto mais popular do mundo para geração de relatórios. É inteiramente escrita em Java e capaz de utilizar dados vindos de qualquer tipo de origem de dados e produzir documentos que podem ser visualizados, imprimidos ou exportas em uma variedade de formatos incluíndo HTML, PDF, Excel, OpenOffice e Word.

## TIBCO Jaspersoft Studio

TIBCO Jaspersoft® Studio é uma ferramenta de edição para construção dos relatórios que serão gerados. A ferramenta ajuda no design de templates de relatórios; construção de queries de relatório; escrever expressões complexas; construções de componentes visuais de layout como mais de 50 tipos de gráficos, mapas, tabelas e componentes customizáveis; e muito mais.

Tudo que é construído nessa ferramenta se gera como produto um ou mais arquivos do tipo JRXML.

## JRXML

O [TIBCO Jaspersoft Studio](#TIBCO-Jaspersoft-Studio) define um relatório através de um arquivo XML. Um arquivo jrxml é composto de diversas seções; algumas relacionadas a características físicas do relatório (como a dimensão da página, o posicionamento dos campos e a altura das bandas), e outras relacionadas a características lógicas (como a declaração de parâmetros e variáveos e da definição da query para seleção de dados).

## Build Path

Durante a construção de um relatório utilizando a ferramenta [TIBCO Jaspersoft Studio](#TIBCO-Jaspersoft-Studio) você verá a necessidade de referenciar propriedades que estarão nas suas classes JAVA, provavelmente POJOs, ou então, utilizar alguma biblioteca de terceiro como o commons-lang3 da Apache por exemplo, para realizar alguma operação com texto, o qual você não quer re-implementar. 

Para que a ferramenta seja capaz de [compilar](#compilação) o seu relatório é preciso que essas classes referenciadas no seu relatório estejam no ClassPath do projeto assim como uma aplicação JAVA qualquer. Para isso, clicar com o botão direito sobre o projeto que está trabalhando no workspace do [TIBCO Jaspersoft Studio](#TIBCO-Jaspersoft-Studio), e acessar o item de menu "Properties" ou Alt+Enter. Na janela que se abrirá navegar para "Java Build Path" e em "Libraries" clicar em "Add External JARs" e adicionar as bibliotecas o qual serão utilizadas pelo seu relatório. Se for as classes JAVA referente a alguma aplicação Web, você pode simplesmente clicar em "Add External Class Folder" e apontar para o diretório `WEB-INF/classes` da sua aplicação.

## Parâmetros

Os parâmetros são formas de se passar dados para um relatório. Isso se aplica também para passar dados de um relatório para um sub-relatório.

## Fields
