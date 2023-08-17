# Using Docker with Flask

I ran through the following
[docs](https://code.visualstudio.com/docs/containers/quickstart-python) to get
started using Docker within VSC.

I used the Flask example, as it's the framework I prefer using. However I don't
use a `requirments.txt` file as I much prefer to use `pipenv` which works
similar to npm IMO. Therefore I had to modify the dockerfile which VSC produced
with the following:

```bash
COPY Pipfile Pipfile.lock ./
RUN python -m pip install --upgrade pip
RUN pip install pipenv && pipenv install --dev --system --deploy
```

The above does the following:

- copies both the `Pipfile` and the `Pipfile.lock` which is kinda similar to
  `packages.json` and `packages.lock`
- then downloads pip within the container and installs `pipenv`
- lastly runs `pipenv` and installs the dependencies.

### Final Docker file.

I am sure there is tones more to using a docker file but this will do me just
fine for the moment.

```Dockerfile
# For more information, please refer to https://aka.ms/vscode-docker-python
FROM python:3.8-slim-buster

EXPOSE 5000

ENV VAR1=10

# Keeps Python from generating .pyc files in the container
ENV PYTHONDONTWRITEBYTECODE=1

# Turns off buffering for easier container logging
ENV PYTHONUNBUFFERED=1

# Install & use pipenv
COPY Pipfile Pipfile.lock ./
RUN python -m pip install --upgrade pip
RUN pip install pipenv && pipenv install --dev --system --deploy

WORKDIR /app
COPY . /app

# Switching to a non-root user, please refer to https://aka.ms/vscode-docker-python-user-rights
RUN useradd appuser && chown -R appuser /app
USER appuser

# During debugging, this entry point will be overridden. For more information, please refer to https://aka.ms/vscode-docker-python-debug
CMD ["/usr/local/bin/gunicorn", "--bind", "0.0.0.0:5000", "app:app"]
```
