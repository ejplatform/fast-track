# Apps django

O framework **django** utiliza apps para construção de projetos. Um projeto é uma aplicação web em django, e estes apps podem ser pacotes externos, que podem ser instalados e adicionados, 
bem como podem ser criações internas compostas de models, views (ou rotas, a depender da implementação), templates, urls, middlewares, arquivos estáticos, entre outros.

## Diagrama de pacotes dos apps do ej

![](https://github.com/ejplatform/fast-track/blob/master/Package%20Diagram.png?raw=true)


## Configuração dos apps do ej

```python
# Reprodução do trecho de código em src/ej/settings/apps.py

project_apps = [
        # Math
        'ej_math',
        'ej_reports',
        'ej_clusters',

        # Conversations
        'ej_boards',
        'ej_conversations',

        # Core apps
        'ej_help',
        'ej_configurations',
        'ej_profiles',
        'ej_users',

        # Gamification
        'ej_powers',
        'ej_notifications',
    ]

 third_party_apps = [
        # Third party apps
        'taggit',
        'rules',
        'allauth',
        'allauth.account',
        'allauth.socialaccount',
        'allauth.socialaccount.providers.facebook',
        'allauth.socialaccount.providers.twitter',
        'allauth.socialaccount.providers.github',
        'allauth.socialaccount.providers.google',
        'django_filters',
        'rest_framework',
        'rest_framework.authtoken',
        'rest_auth',
        'rest_auth.registration',
        # 'corsheaders',
        'constance',
        'constance.backends.database',
    ]

```

## Apps do EJ


Os apps do EJ possuem uma estrutura dividida basicamente em:

- Models: Classes de domínio da aplicação, são responsáveis pela interface com o banco de dados;
- Routes: Responsáveis por gerenciar a troca de informações entre models e templates;
- Templates jinja2: Coleta as informações do usuário e também exibe informações recebidas do sistema.

### EJ

Neste app constam as configurações principais do ej, as urls da aplicação, bem como os templates de base, rotas básicas e alguns componentes reutilizáveis.

### Apps do Math

Esses apps possuem models, rotas, templates e outros componentes que fazem suporte aos cálculos utilizados no EJ para construção de esteriótipos e clusterização de acordo com a interação de usuários com as conversas.

### Apps de Conversa

Esses apps possuem models, rotas, templates e outros componentes que fazem suporte à criação e interação com conversas, por meio de votos e comentários, sendo que a conversa pode ou não fazer parte do mural de um usuário.

### Apps Core

Esses apps possuem models, rotas, templates e outros componentes que fazem suporte à interação dos usuários, que podem ter perfils com diversas configurações
### Apps da Gamificação

Esses apps possuem a parte principal da lógica de gamificação construída para o ej, possuindo os poderes, que podem ser atribuidos para usuários, como por exemplo o poder de promover comentários, e também as notificações que o usuário pode receber de poder recebido, ou comentário promovido.

# Django boogie
