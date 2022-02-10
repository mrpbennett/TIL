# [Setting up sqlalchemy](https://docs.sqlalchemy.org/en/14/orm/tutorial.html#object-relational-tutorial-1-x-api)

Object relation databases

## [Connecting to the DB](https://docs.sqlalchemy.org/en/14/orm/tutorial.html#connecting)

To connect to the db we use `create_engine()` which creates out connection to what ever DB we choose. In the case here we're using an SQLite 3 database. The return value of `create_engine()` is an instance of `Engine`, and it represents the core interface to the database

```python
# db.py
from sqlalchemy import create_engine
engine = create_engine('sqlite:///db.sqlite3', echo=True)
```

When using the ORM, the configurational process starts by describing the database tables we’ll be dealing with, and then by defining our own classes which will be mapped to those tables.

Classes mapped using the Declarative system are defined in terms of a base class which maintains a catalog of classes and tables relative to that base.

```python
# db.py
from sqlalchemy.orm import declarative_base

Base = declarative_base()
```

## [Setting up our models](https://docs.sqlalchemy.org/en/14/orm/tutorial.html#declare-a-mapping)

Now that we have a “base”, we can define any number of mapped classes in terms of it. We will start with just a single table called users. A new class called `User` will be the class to which we map this table. Within the class, we define details about the table to which we’ll be mapping, primarily the table name, and names and datatypes of columns:

```python
# models.py
from sqlalchemy import Column, Integer, String

class User(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True)
    name = Column(String)
    fullname = Column(String)
    nickname = Column(String)

    def __repr__(self):
       return "<User(name='%s', fullname='%s', nickname='%s')>" % (self.name, self.fullname, self.nickname)
```

> ☝🏼 The User class defines a `__repr__()` method, but note that is **optional**; this is only implemented here so that our examples show nicely formatted User objects.

A class using Declarative at a minimum needs a `__tablename__` attribute, and at least one Column which is part of a primary key `1`. 

## [Creating our database](https://docs.sqlalchemy.org/en/14/orm/tutorial.html#create-a-schema)

Now the SQLAlchemy tutorial mentions you can run the following from the Python repl but I ran into issues.

```zsh
>>> Base.metadata.create_all(engine)
```

Therefore I simply created a small module called `create_db.py` with the following:

```python
#create_db.py
from db import Base, engine

print("creating database...")

Base.metadata.create_all(engine)
```

## [Creating a session](https://docs.sqlalchemy.org/en/14/orm/tutorial.html#creating-a-session)

We’re now ready to start talking to the database. The ORM’s “handle” to the database is the `Session`. When we first set up the application, at the same level as our `create_engine()` statement, we define a `Session` class which will serve as a factory for new Session objects:

```python
# db.py
from sqlalchemy import Column, Integer, String, create_engine
from sqlalchemy.orm import declarative_base, sessionmaker

engine = create_engine("sqlite:///db.sqlite3", echo=True)

Base = declarative_base()

# binding our engine to sessionmaker
Session = sessionmaker(bind=engine)
```

This custom-made `Session` class will create new Session objects which are bound to our database. Other transactional characteristics may be defined when calling `sessionmaker` as well; these are described in a later chapter. Then, whenever you need to have a conversation with the database.

We can the import our `sessionmaker()` instance into our `app.py` like so. 

```python
# app.py
# ...

db = Session()
```

## [Adding and Updating Objects](https://docs.sqlalchemy.org/en/14/orm/tutorial.html#adding-and-updating-objects)

Now we can add data into our database. We can do this by using `db.add()` and then `db.commit()`

As an example:

```python
# app.py
db.add_all(
    [
        User(name="wendy", fullname="Wendy Williams", nickname="windy"),
        User(name="mary", fullname="Mary Contrary", nickname="mary"),
        User(name="fred", fullname="Fred Flintstone", nickname="freddy"),
    ]
)

db.commit() # commits above data to our database
db.close() # closes our session with our database
```

PS: Yes this is more or less the official docs from SQLAlchemy but I did learn it 🤣