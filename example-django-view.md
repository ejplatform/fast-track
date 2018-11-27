https://github.com/rodrigocam/gold-penny-api/blob/master/goldpenny/users/views.py

```python
def index(request):
    if request.user.is_authenticated:
        total_earnings = 0
        products_sold = []

        event = list(Event.objects.filter(user=request.user))
        if event:
            event = event[0]
            products_sold = Product.objects.filter(event=event)
            for product in products_sold:
                total_earnings += product.price * product.total_sold
            
            total_earnings = format_currency(total_earnings, 'BRL', locale='pt_BR')

        ctx = {
            'event': event,
            'total_earnings': total_earnings,
            'products_sold': products_sold,
        }

        return render(request, 'users/home.jinja2', ctx)

return HttpResponseRedirect('/login')
```



https://github.com/rodrigocam/gold-penny-api/blob/master/goldpenny/users/urls.py
```python 
urlpatterns = [
    path('', index, name='index'),
    path('login/', auth_views.login, {'template_name': 'users/login.jinja2'}, name='login'),
    path('logout/', auth_views.logout, {'next_page': 'index'}, name='logout'),
]
```
