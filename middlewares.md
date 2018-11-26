## Middlewares do EJ

Middlewares são uma camada em que toda request passa antes de chegar no processamento denido para determinada rota no servidor.
Ex.: acessar https://www.ejplatform.org/conversations/servicos-publicos-futuro/ antes de chegar na rota definida para esta url,
um middleware executa um determinado processamento.

O django oferece um sistema de middleware bastante interessante e de razoavelmente fácil utilização, onde no arquivo de configuração
existe uma lista de middlewares que o django utilizará.

### Por que usar middlewares?

Os middlewares são bastante interessantes quando se deseja fazer uma lógica diferenciada para o processamento das urls ou adicionar
alguma lógica pré-processamento de rotas.

### Por que usamos middlewares no EJ?

Nós temos algumas lógicas de url um pouco diferentes, por exemplo, os urls de conversa são case insensitive, o que significa que
se acessarmos https://www.ejplatform.org/conversations/servicos-publicos-futuro/ ou https://www.ejplatform.org/conversations/Servicos-Publicos-Futuro/
acessamos a mesma conversa.

Além de urls de conversa, os quadros de conversa também possuem link case insensitive https://www.ejplatform.org/semanadeinovacao/ e https://www.ejplatform.org/Semanadeinovacao/ dão no mesmo.

### Como conseguimos isso?

Nos middlewares de board e conversations, sempre que uma resposta for dar 404, a gente pega a url da um split por '/' aplicamos um slugify em todos os termos gerados e montamos uma nova url. Com a função ```resolve()``` do django conseguimos como retorno qual é a view_function responsável por esta url e assim conseguimos processar corretamente a url.
