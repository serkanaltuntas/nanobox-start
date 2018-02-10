# nanobox-start
initial seed to start a brand new nanobox.io based django project

## Initializing the project

Raplace the `PROJECT_NAME` with your actual project name without angle brackets on the following command:

```bash
git clone https://github.com/serkanaltuntas/nanobox-start.git PROJECT_NAME
cd PROJECT_NAME
nanobox run django-admin startproject PROJECT_NAME .
```

Please pay attention to the `dot`, at the end of startproject command.

## Setting Up Local Server URL

Configure `ALLOWED_HOSTS` in settings.py:

````
ALLOWED_HOSTS = [
    '*',
]
````

Also later, you can include nanobox URL of your deployed app and read domain name of the app. Make sure they are all comma separated and in quotes. 


## Setting Up Templates Directory (Optional)

Create a templates directory inside main project directory. This is not related to nanobox setup but it is good to remember at this stage for convinience.

```bash
mkdir templates
```

Inside the setting file, under the TEMPLATES setting, replace this:
````
'DIRS': [],
````

with this:
````
'DIRS': [os.path.join(BASE_DIR, 'templates'),],
````

## Setting Up Static Root Directory (Optional)

Create a static directory inside main project directory. If there will be no static files like CSS, javascript or, images you do not have to do apply this step. This is not related to nanobox setup but it is good to remember at this stage for convinience.

```bash
mkdir static
```

Add this line to the very end of settings.py:

````
STATIC_ROOT = os.path.join(BASE_DIR, 'static/')
````

## Setting Up Database Connection

Find this:
````
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
````


Replace it with this:
````
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'gonano',
        'USER': os.environ.get('DATA_DB_USER'),
        'PASSWORD': os.environ.get('DATA_DB_PASS'),
        'HOST': os.environ.get('DATA_DB_HOST'),
        'PORT': '',
    }
}
````

## Modify boxfile.yml

Find:
````
nginx: nginx -c /app/etc/nginx.conf
django: gunicorn -c /app/etc/gunicorn.py app.wsgi
````

Replace the `app` with `PROJECT_NAME`:
````
nginx: nginx -c /app/etc/nginx.conf
django: gunicorn -c /app/etc/gunicorn.py PROJECT_NAME.wsgi
````

## Run Migrations (Locally)

```bash
# Run initial migrations:
nanobox run python manage.py migrate
```


## Delopy to Nanobox

```bash
# Run initial migrations:
nanobox remote add <nanobox-app-name>
nanobox deploy
```

## Localhost
```bash
# Add a convenient way to access your app from the browser
nanobox dns add local django.local

# Run django as you would normally, with Nanobox
nanobox run python manage.py runserver 0.0.0.0:8000
```
Visit http://django.local:8000
