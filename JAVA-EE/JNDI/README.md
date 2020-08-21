# JNDI

- [Referências de Recursos](#referências-de-recursos)
- [Lookup](#lookup)

## Referências de Recursos

Toda referência de recurso registrado no arquivo web.xml seja `resource-ref` ou `ejb-resource-ref` se torna disponível via o JNDI `java:comp/env/`. Desta forma o lookup pode ser realizado através do nome JNDI a partir de `java:comp/env/`.

## Lookup

http://blog.billcode.com.br/2011/04/entendendo-o-lookup-jndi.html
