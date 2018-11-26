## Hyperpython

[Hyperpython](https://github.com/fabiommendes/hyperpython) é uma biblioteca que fornce maneiras de se escrever HTML utilizando python (assim como o hyperscript faz na linguagem javascript).

Esta biblioteca foi criada pelo professor (Fabio)[https://github.com/fabiommendes] com o intuito de reduzir a utilização de templates, tendo em vista que templates separam tecnologias e não preocupações. Assim apenas separar html ou linguagem de template do código em si não resolve alguns problemas de reuso de código e arquitetura do projeto.

Com o hyperpython nós conseguimos criar funções puramente python que servem como componentes view da aplicação, possibilitando a execução de toda uma lógica dentro desta função.

Para visualizar melhor, vamos pensar em um exemplo:

Imagina que seria legal ter um componente que renderiza uma lista de itens como uma lista que colapsa, essa lista é utilizada em vários lugares da nossa aplicação, modificando os itens e talvez o estilo.

Se fossemos utilizar uma linguagem de template como o jinja2, teríamos que criar uma macro que recebe uma lista de itens, fazer um for, talvez um if, tudo isso utilizando a syntaxe de template que não é muito amigável para contemplar toda essa lógica.

Utilizando o hyperpython, podemos usar uma função comum do python:

```python
def collapsible_list(item_list, title=None, **kwargs):
  return div(
    class_='CollapsibleList',
    is_component=True
  )[
    div(class_='CollapsibleList-data')[
      html_list(data),
    ]
  ]
```
