# deploy taiga in docker via one command:

```pip3 install jinja2
python3 setup.py
```

inspired by https://github.com/douglasmiranda/docker-taiga

-change the docker-compose.yml to  v3

-generate install scripts via rendering template

-remove "python /home/taiga/taiga-back/manage.py loaddata initial_role"  follow offical answer 

3.1.0 Install : CommandError: No fixture named 'initial_role' found.
https://github.com/taigaio/taiga-back/issues/958


and src taiga-back need fix:

add "from . import celery_local" @ taiga-back/settings/__init__.py

# Deploy using the above method, issue encountered and solutions:

- Backend connection refused error:
  In frontend config file conf.json, change  "api": "http://{{TAIGA_HOSTNAME}}/api/v1/",  to "api": "http://{{TAIGA_HOSTNAME}}:{{BACKEND_PORT}}/api/v1/"
  
- Error in building backend images:
  Build backend images with python3.5 instead of the default python2.7
  In backend dockerfile change 'FROM python' to 'FROM python3.5' then all the modules would be successfully built.
  
- All images, attachment cannot be accessed:
  in backend/setting/local.py change """MEDIA_URL = "{}://{{TAIGA_HOSTNAME}}/media/".format(_HTTP)""" to """MEDIA_URL = "{}://{{TAIGA_HOSTNAME}}:{{BACKEND_PORT}}/media/".format(_HTTP)""" 
   change STATIC_URL accordingly.
 
