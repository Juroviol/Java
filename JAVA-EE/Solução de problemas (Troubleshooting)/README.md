# Solucão de problemas ou Troubleshootig

#### Como descobrir que implementações estão sendo carregadas no ClassLoader da JVM

Incluir nos argumentos da JVM o seguinte

```
-versobe:class
```

Dessa forma, tanto na inicialização do servidor quanto em tempo de execução será logado quais são as classes e suas respectivas implementações. Isso é muito útil para descobrir problemas de conflitos de bibliotecas.