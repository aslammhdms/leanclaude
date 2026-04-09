# Django Patterns (Python)

## App Structure

Apps are **feature-bounded**, not layer-bounded. Each app owns its models, views, services, and URLs.

```
myproject/
  users/                # Feature app
    models.py
    views.py
    services.py         # Business logic (not a Django default — add it)
    urls.py
    admin.py
    tests/
      test_views.py
      test_services.py
  orders/               # Another feature app
    models.py
    ...
  myproject/
    settings/
      base.py
      development.py
      production.py
    urls.py
    wsgi.py
```

## Business Logic in Services, Not Views

```python
# WRONG — business logic in the view
def create_order(request):
    order = Order.objects.create(user=request.user, ...)
    send_confirmation_email(order)        # business logic in view
    update_inventory(order.items.all())  # business logic in view
    return redirect('orders:detail', pk=order.pk)

# CORRECT — delegate to a service
def create_order(request):
    order = order_service.create(user=request.user, data=request.POST)
    return redirect('orders:detail', pk=order.pk)

# services.py
def create(user, data):
    order = Order.objects.create(user=user, ...)
    send_confirmation_email(order)
    update_inventory(order.items.all())
    return order
```

## Queryset Optimization

Always use `select_related` and `prefetch_related` to avoid N+1:

```python
# WRONG — N+1 query (one query per order item)
orders = Order.objects.all()
for order in orders:
    print(order.user.email)  # Extra query for each order

# CORRECT
orders = Order.objects.select_related('user').all()
```

## Migrations

- Never edit an applied migration — create a new one
- Use descriptive migration names: `python manage.py makemigrations --name add_user_email_index`
- Always review the generated SQL: `python manage.py sqlmigrate appname 0005`
- Data migrations go in separate files from schema migrations

## Settings

```python
# settings/base.py — shared settings
# settings/development.py — local overrides
# settings/production.py — production-only settings

# Never hardcode secrets in settings files
import os
SECRET_KEY = os.environ['DJANGO_SECRET_KEY']
DATABASES = {'default': {'URL': os.environ['DATABASE_URL']}}
```
