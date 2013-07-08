Wind Potion - REST API for use with Tornado and SQLAlchemy Elixir
===========

##About
A simple structure to build RESTful handler

##Dependeces
Tested in python 2.7
*   Tornado 3
*   Elixir

##Installing
```
python setup.py install
```

##How to use
First create a service, (Here we'll create a user service), and annotates like a service of the
entity user, (the elixir Entity):
```python
from windpotion.annotations import service

@windpotion.annotations.service(User)
class UserService(object):
    pass

```

Ok, now simply create a REST handler which inherits from RestHandler, annotates like a REST using as a service
the previous made UserService
```python
from windpotion.annotations import REST
from windpotion.handlers import RestHandler

@windpotion.annotations.REST(UserService)
class UserRESTHandler(RestHandler):
    """
    User REST handler
    """
    def __init__(self, application, request, **kwargs):
        RestHandler.__init__(self, application, request, **kwargs)
```

If you want to permit only a logged user to use the REST methods from handler, it's just use the
SecureRestHandler:
```python
from windpotion.annotations import REST
from windpotion.handlers import SecureRestHandler

@REST(UserService)
class UserRESTHandler(SecureRestHandler):
    """
    User REST handler
    """
    def __init__(self, application, request, **kwargs):
        SecureRestHandler.__init__(self, application, request, **kwargs)
```

If only a few REST methods must be secured by authentication, (in this exmaple the DELETE) simply do:
```python
from windpotion.annotations import REST
from windpotion.handlers import RestHandler

@REST(UserService)
class UserRESTHandler(RestHandler):
    """
    User REST handler
    """
    def __init__(self, application, request, **kwargs):
        RestHandler.__init__(self, application, request, **kwargs)

    @windpotion.annotations.authenticated
    def delete(self, id):
        RestHandler.delete(id)
```

You want override the service default methods? I'll show a example of all users created has the named
turned to upper case:
```python
from windpotion.annotations import service

@service(User)
class UserService(object):

    def create(self, dict_args):
        dict_args['name'] = dict_args['name'].upper()
        self._meta.create(self, dict_args)
```
Note the *_meta* property, this is because the original create method is from a metaclass,
so you can access all the ServiceMeta methods using this property.

#Author
Rubens Pinheiro Gonçalves Cavalcante
email: [rubenspgcavalcante@gmail.com](mailto:rubenspgcavalcante@gmail.com)

##License & Rights
*Using GNU LESSER GENERAL PUBLIC LICENSE *Version 3, 29 June 2007*
[gnu.org](http://www.gnu.org/copyleft/lgpl.html,"LGPLv3")
