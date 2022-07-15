# Using pipenv with Docker

I prefer to use [`pipenv`](https://pipenv.pypa.io/en/latest/) with my Python projects. Coming from JS I think it's similar to NPM. Therfore when using VSC to create a Docker file, the default comes with requirements. 

Like so:

```docker
COPY requirements.txt .
RUN python -m pip install -r requirements.txt
```


To use pipenv within your image just replace the above with the below.

```docker
COPY Pipfile Pipfile.lock ./
RUN python -m pip install --upgrade pip
RUN pip install pipenv && pipenv install --dev --system --deploy
```

This will copy your `Pipfile` and `Pipfile.lock` and run the install commands, installing all your dependencies. 