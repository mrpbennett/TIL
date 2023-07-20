# Using pipenv with Docker

I prefer to use [`pipenv`](https://pipenv.pypa.io/en/latest/) with my Python
projects. Coming from JS I think it's similar to NPM. Therfore when using VSC to
create a Docker file, the default comes with requirements.

Like so:

```docker
# Use the official Python image as the base image
FROM python:3.10-slim

# Set environment variables to avoid buffering of Python output
ENV PYTHONUNBUFFERED 1

# Install system dependencies required for some Python packages
RUN apt-get update && apt-get install -y \
    build-essential \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# Set the working directory inside the container
WORKDIR /app

# Copy the Pipfile and Pipfile.lock into the container
COPY Pipfile Pipfile.lock /app/

# Install pipenv
RUN pip install --no-cache-dir pipenv

# Install Python dependencies using pipenv
RUN pipenv install --system --deploy

# Copy the rest of the application files into the container
COPY . /app/

# Set the entry point to main.py
ENTRYPOINT ["python", "main.py"]
```

Make sure to place this Dockerfile in the same directory as your Pipfile,
Pipfile.lock, and main.py files.

Explanation of the Dockerfile:

1. We use the official Python 3.9 image as the base image to build our
   container.
2. The environment variable PYTHONUNBUFFERED is set to 1 to ensure that Python
   output is not buffered and displayed immediately.
3. We install some system dependencies required by certain Python packages.
4. The working directory inside the container is set to /app.
5. We copy the Pipfile and Pipfile.lock into the container.
6. Pipenv is installed inside the container using pip.
7. The Python dependencies are installed inside the container using pipenv
   install --system --deploy, which ensures that only production dependencies
   are installed.
8. The rest of the application files (including main.py) are copied into the
   container.
9. Finally, the entry point of the container is set to main.py, so when the
   container starts, it will execute main.py as the main script.
