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

Nos apps do EJ existem algumas simplificações em relação aos apps django, isto ocorre pois é utilizado o framework [django-boogie](https://github.com/fabiommendes/django-boogie). Algumas facilidades são:

- Marcação de models(@rest_api) para criação da api rest;

```python
# Exemplo de trecho de código em src/ej_users/models.py
from boogie.rest import rest_api


@rest_api(['id', 'display_name'])
class User(AbstractUser):
    """
    Default user model for EJ platform.
    """

    display_name = models.CharField(
        _('Display name'),
        max_length=140,
        unique=True,
        default=random_name,
        help_text=_(
            'A randomly generated name used to identify each user.'
        ),
    )
    email = models.EmailField(
        _('email address'),
        unique=True,
        help_text=('Your e-mail address')
    )

    objects = UserManager()

    @property
    def username(self):
        return self.email.replace('@', '__')

    USERNAME_FIELD = 'email'
    REQUIRED_FIELDS = ['name']

    @property
    def profile(self):
        profile = rules.get_value('auth.profile')
        return profile(self)

    class Meta:
        swappable = 'AUTH_USER_MODEL'
```



- Rotas, que são uma facilitação entre views e urls;


```python
# Exemplo de trecho de código em src/ej_profiles/routes.py
from boogie.router import Router


app_name = 'ej_profiles'
urlpatterns = Router(
    template=['ej_profiles/{name}.jinja2', 'generic.jinja2'],
    login=True,
)


@urlpatterns.route('edit/')
def edit(request):
    profile = request.user.profile
    if request.method == 'POST':
        form = ProfileForm(request.POST, instance=profile, files=request.FILES)
        name_form = UsernameForm(request.POST, instance=request.user)
        if form.is_valid() and name_form.is_valid():
            form.save()
            name_form.save()
            return redirect('/profile/')
    else:
        form = ProfileForm(instance=profile)
        name_form = UsernameForm(instance=request.user)

    return {
        'form': form,
        'name_form': name_form,
        'profile': profile,
    }

```

- Regras para apps, facilitando a organização de permissões em rotas e implementação de regras de negócio;



```python
# Configuração das regras é necessário no aplicativo (código adaptado de src/ej_conversations/apps.py
from django.apps import AppConfig
from django.utils.translation import ugettext_lazy as _


class EjConversationsConfig(AppConfig):
    name = 'ej_conversations'
    verbose_name = _('Conversations')
    rules = None
    api = None
    roles = None

    def ready(self):
        from . import rules, api, roles

        self.rules = rules
       
# Código extraído do arquivo src/ej_conversations/rules.py

@rules.register_perm('ej.can_vote')
def can_vote(user, conversation):
    """
    User can vote in a conversation if there are unvoted comments.
    """
    if user.id is None:
        return False
    return bool(
        conversation.approved_comments
            .exclude(votes__author_id=user.id)
    )
    
 @rules.register_perm('ej.can_promote_conversation')
def can_promote_conversation(user):
    """
    Can promote a conversation of a board to the list of promoted conversations.
    """
    return (
        user.is_superuser
        or user.has_perm('ej_conversations.can_publish_promoted')
    )


"""
As permissões podem ser acessadas, em um objeto de usuário de acordo com o nome que ela foi registrada, 
Exemplo: user.has_perm('ej.can_vote')
"""
```
