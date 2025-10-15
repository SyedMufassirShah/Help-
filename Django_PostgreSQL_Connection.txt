# STEP#1:   In settings.py replace the default DATABASE dictionary with the below code:

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'project',  #  Your Project Name
        'USER': 'postgres',
        'PASSWORD': 'Admin123',  # your password
        'HOST': 'localhost',
        'PORT': '5432',
    }
}





# STEP#2:  Install the Packages Required for Database (PostgreSQL) Integration using the IDE's Terminal


--->  pip install psycopg2-binary
PostgreSQL database adapter for Python — allows your code to talk to the PostgreSQL server.



--->  pip install sqlalchemy
ORM (Object Relational Mapper) — helps define database models and query data in Pythonic syntax.



--->  pip install alembic
Database migrations — used for managing schema changes (optional but professional).