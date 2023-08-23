# :books: TIL (Today I Learnt)

Inspired by @jbranchaud's [repo](https://github.com/jbranchaud/til) and
@thoughtbot's [repo](https://github.com/thoughtbot/til)

This project is a collection of small things I learn about different languages,
frameworks and technologies. It consists of short markdown documents summarising
concepts or tips I'd like to share. The goal is to write something new everyday
or as often as possible.

- [Categories](#categories)

---

# Categories

- [:books: TIL (Today I Learnt)](#books-til-today-i-learnt)
- [Categories](#categories)
- [Applications](#applications)
  - [DataGrip](#datagrip)
- [Automation](#automation)
- [Build / Containers](#build--containers)
  - [Docker](#docker)
  - [Kubernetes](#kubernetes)
- [CCNA](#ccna)
- [Command Line](#command-line)
  - [BASH](#bash)
- [Databases](#databases)
  - [TimescaleDB](#timescaledb)
  - [SQL](#sql)
    - [MS SQL](#ms-sql)
    - [Postgres](#postgres)
    - [Presto](#presto)
  - [NoSQL](#nosql)
    - [Redis](#redis)
- [Datatypes](#datatypes)
- [DevOps](#devops)
  - [Github Actions](#github-actions)
- [Excel](#excel)
- [Linux](#linux)
  - [Ubuntu](#ubuntu)
- [Git](#git)
- [Github](#github)
- [Go 🐹](#go-)
- [GraphQL](#graphql)
- [HTML 5](#html-5)
- [CSS](#css)
- [Java ☕️](#java-️)
- [JavaScript](#javascript)
  - [Node](#node)
  - [React](#react)
  - [React Router](#react-router)
  - [Vite](#vite)
  - [Vue](#vue)
- [Markdown](#markdown)
- [Monitoring](#monitoring)
  - [Grafana](#grafana)
  - [Prometheus](#prometheus)
- [Python](#python)
  - [Django](#django)
  - [FastAPI](#fastapi)
  - [Flask](#flask)
  - [Pandas 🐼](#pandas-)
  - [SQLAlchemy](#sqlalchemy)
  - [Testing](#testing)
- [Rust](#rust)
- [Terminal](#terminal)
  - [BASH](#bash-1)
  - [Vim](#vim)
- [Sys Admin](#sys-admin)

---

# Applications

### DataGrip

- [Setting up for multple schemas](applications/datagrip/multiple_schemas.md)

# Automation

- [Adding a Chronos job](automation/chronos.md)

# Build / Containers

- [Creating a Minified CSS file from Scss](builds-containers/minicss-from-sass.md)
- [Deploying to prod at work](builds-containers/deploying-to-prod.md)

### Docker

- [Basic build / push process](containers/docker/build_process.md)
- [Using Docker with Flask](containers/docker/flask_docker.md)
- [Building using x86](https://blog.jaimyn.dev/how-to-build-multi-architecture-docker-images-on-an-m1-mac/#tldr;)
- [Using `pipenv` with Docker images](containers/docker/using-pipenv-with-docker.md)
- [Using Docker Compose for local development](containers/docker/docker-compose.md)
- [Docker hub my way](containers/docker/docker_hub.md)
- [How to SSH into a Docker Container](https://www.freecodecamp.org/news/how-to-ssh-into-a-docker-container-secure-shell-vs-docker-attach/)

### Kubernetes

# CCNA

- [Switches](ccna/switches.md)
- [TCP/IP OSI](ccna/tcpip-osi.md)

# Command Line

- [Creating a new file](command-line/creating-new-files.md)

### BASH

- [Using arguments in bash](command-line/bash/arguments.md)
- [Doing math in bash](command-line/bash/math.md)
- [Using variables in bash](command-line/bash/variables.md)
- [If statements in bash](command-line/bash/if_statements.md)
- [Getting user input with `read` in bash](command-line/bash/read.md)
- [Running Linux commands within bash script](command-line/bash/running-cmds.md)
- [Conditionals in bash](https://linuxconfig.org/bash-scripting-conditionals)

# Databases

- [Skill requirements for all level of db engineers](databases/skill_requirements.md)
- [Database Developer journey](databases/data_engineer.md)
- [What is ETL?](https://www.ibm.com/topics/etl)

### TimescaleDB

- [Installing TimescaleDB in Docker](databases/timescaledb/timescaledb.md)

### SQL

- [Install mySQL via Homebrew](databases/sql/installing-mysql-via-homebrew.md)
- [Changing a users password](databases/sql/changing-password.md)
- [Setting a default value](databases/sql/setting-default-values.md)
- [Inserting data into tables](databases/sql/inserting-data-into-tables.md)
- [Auto increment IDs](databases/sql/auto-increment-primary-ids.md)
- [Deleting a database](databases/sql/deleting-a-database.md)
- [Finding & Deleting db users](databases/sql/finding-users-and-removing.md)
- [Using `SET` in SQL](databases/sql/set.md)
- [`CAST` function](databases/sql/cast-func.md)
- [`CASE` function](databases/sql/case-func.md)
- [`HAVING` clause](database/sql/having-clause.md)
- [`SERIAL` in SQL](databases/sql/serial.md)
- [JOINs in SQL](databases/sql/joins.md)
- [Decimal data type](databases/sql/decimal-type.md)
- [Sub Queries](databases/sql/sub-queries.md)
- [SQL functions in supabase](databases/sql/sql_functions.md)
- [What's an upsert and how to use one](https://www.cockroachlabs.com/blog/sql-upsert/#what-is-an-upsert-in-sql)
- [Install ODBC on Ubuntu](https://www.imaginelinux.com/install-odbc-on-ubuntu/#install-odbc-on-ubuntu-20-04-debian-11)
- [Counting uniques in SQL](databases/sql/count_unique.md)
- [Visualising the SQL query execution order](databases/sql/visualising-query.md)

#### MS SQL

- [Naming convention for MS SQL](databases/sql/mssql/naming_convention.md)
- [COALESCE usage](databases/sql/mssql/coalesce.md)
- [Python SQL Driver - pyodbc](https://learn.microsoft.com/en-us/sql/connect/python/pyodbc/python-sql-driver-pyodbc?view=sql-server-ver16)

#### Postgres

- [Naming convention for Postges](databases/sql/postgres/naming_convention.md)
- [Defining a FK when creating a table](databases/sql/postgres/defining_fk.md)
- [Installing just `psql` on MacOS](databases/sql/postgres/psql_mac.md)
- [Connecting to postgres via psql inside a Docker container](databases/sql/postgres/psql_inside_container.md)

#### Presto

- [Connecting to PrestoDB via presto-python-client](databases/sql/presto/connecting_to_presto.md)
- [Use of `regexp_like()`](databases/sql/presto/regexp_like.md)

### NoSQL

- [ELI5 how NoSQL stores data](databases/nosql/how-nosql-stores-data.md)

#### Redis

- [How Redis stores data (simple version)](databases/nosql/redis/how-redis-stores-data.md)
- [Redis data types](databases/nosql/redis/redis-data-structures.md)
- [KEYS](databases/nosql//redis/redis-keys.md)

# Datatypes

- [JS / Python Classes](datatypes/classes.md)

# DevOps

- [90 Days of DevOps](devops/90-days-of-devops.md)

### Github Actions

- [Python, Pipenv and lint with Ruff](devops/github/python-actions.md)

# Excel

- [Using the FV formula](excel/fv-formula.md)
- [Checking for an INT](excel/is-an-int.md)

# Linux

- [Creating folders and files](https://ubuntu.com/tutorials/command-line-for-beginners#4-creating-folders-and-files)
- [Moving and manipulating files](https://ubuntu.com/tutorials/command-line-for-beginners#5-moving-and-manipulating-files)
- [Showing hidden files](linux/showing-hidden-files.md)
- [Yum basics - CentOS](linux/yum.md)
- [Getting the latest sofware center](linux/software-center.md)
- [Stop using sudo or change user to `root`](linux/admin.md)
- [How to change a user password](https://www.tomshardware.com/how-to/change-passwords-in-linux)
- [Changing the shell of user](https://www.thegeekdiary.com/centos-rhel-how-to-change-the-login-shell-of-the-user/)
- [How to manage Linux permissions for users, groups, and others](https://www.redhat.com/sysadmin/manage-permissions)
- [Manage file permissions on Unix-like systems](https://kb.iu.edu/d/abdb#:~:text=To%20change%20file%20and%20directory,%2C%20write%2C%20and%20execute%20permissions.)
- [Checking IP addresses on VMs](linux/ip-addresses.md)
- [Connecting to SFTP via command line](linux/connecting_to_sftp.md)
- [Basic SFTP commands](https://www.uppmax.uu.se/support/user-guides/basic-sftp-commands/#:~:text=Exit%20sFTP%20Shell,can%20see%20sftp%3E%20prompt%20return.)
- [How to Use SFTP Command to Transfer Files](https://linuxize.com/post/how-to-use-linux-sftp-command-to-transfer-files/#before-you-begin)
- [`grep` command](https://youtu.be/Tc_jntovCM0)

### Ubuntu

- [Reset the dock to standard](https://www.omgubuntu.co.uk/2023/06/reset-ubuntu-dock-default-settings)
- [How to Customize the Ubuntu Dock to Look Like macOS](https://www.makeuseof.com/configure-ubuntu-dock-to-look-like-macos/)
- [How to run commands as root without a password](https://www.linuxfordevices.com/tutorials/linux/sudo-nopasswd)
- [How to Install Synaptic on Ubuntu](https://linuxhint.com/install-synaptic-ubuntu/)
- [My fav extensions](linux/ubuntu/fav_extentions.md)

# Git

- [Creating & Switching branches](git/creating-and-switching-branches.md)
- [Pushing to a new remote repo](git/pushing-local.md)
- [Using git reset](git/git-reset.md)
- [How to find which commit introduced a bug](git/git-bisect.md)
- [Cleaning up branches in Git](git/cleaning_branches.md)
- [Cloning from a certain branch](git/clone_from_branch.md)

# Github

- [Setting permissions](github/permissions.md)

# Go 🐹

- [Basic data types in Go](go/basic-data-types.md)
- [Basic understanding of pointers](go/pointers.md)
- [Using the `fmt` package](go/fmt.md)

# GraphQL

- [Switching GraphQL IDEs](graphql/switching-graphQL-IDEs.md)
- [Making required fields in a schema](graphql/making-schema-fields-required.md)

# HTML 5

- [Bullet point](html/bullet.md)
- [What is a polyfill](html/polyfill.md)
- [Accessibility labels](html/accessibilityLabels.md)

# CSS

- [box-sizing](css/box-sizing.md)
- [animation-fill-mode](css/animation-fill-mode.md)
- [animation-iteration-count](css/animation-iteration-count.md)
- [Hiding scrollbars in TailWindCSS](css/hiding-scrollbars.md)
- [Giving even rows a different bg color in TW](css/setting_even_rows.md)
- [Safelisting classes so they can be passed dynamically in TW](https://tailwindcss.com/docs/content-configuration#safelisting-classes)

# Java ☕️

- [Data types](https://www.w3schools.com/java/java_data_types.asp)
- [Arrays](java/arrays.md)
- [Ternary operator](java/ternary.md)

# JavaScript

- [Default arguments](javascript/default-arguments.md)
- [Comparing / checking arrays](javascript/checking_arrays.md)
- [Difference between `const` & `let`](javascript/difference-let-const.md)
- [Objects with functions](javascript/objects-with-functions.md)
- [String methods](javascript/string-methods.md)
- [Do While loop](javascript/do-while.md)
- [Number methods](javascript/number-methods.md)
- [`.forEach` and it's usage](javascript/forEach.md)
- [`.splice()` and it's usage](javascript/splice.md)
- [`.filter()` and it's usage](javascript/filter-array.md)
- [`.map()` and it's usage](javascript/map-array.md)
- [`sort()` and it's usage](javascript/sort-method.md)
- [DOM Query selectors](javascript/query-selectors.md)
- [Callback functions](javascript/callback-functions.md)
- [Getting current url of a page](javascript/getting-current-url.md)
- [How `this` works in arrow funcs](javascript/this-arrow-funcs.md)
- [Using JSON](javascript/stringify-and-parse.md)
- [`.toLocaleString`](javascript/tolocalestring.md)
- [Replacing `\n` in html](javascript/replacing_newline_jsonstring.md)
- [Disabling Web Security (CORS) for Chrome](javascript/disable_web.md)
- [Using `setTimeout()`](javascript/set_timeout.md)
- [`keydown` events](javascript/key-down.md)
- [reloading the page](javascript/reloading_the_page.md)
- [Handle Vercel preview and Supabase auth](javascript/supabase-auth.md)
- [Creating a dynamic pixel](javascript/creating-dynamic-pixel.md)

### Node

- [automatically restarting the node application](javascript/node/nodemon.md)
- [Using local env variables](javascript/node/local-enviroment-variables.md)
- [Calling Presto from Node](javascript/node/presto-on-node.md)
- [Handling CORS in express](javascript/node/express-cors.md)
- [Using Joi for schema validation](javascript/node/joi-validation.md)
- [Docker image for Node](https://docs.docker.com/language/nodejs/build-images/)

### React

- [Rendering lists using `.map()`](javascript/react/rendering-lists-with-map.md)
- [How to use inline styles](javascript/react/using-inline-styles.md)
- [Creating a star rating](javascript/react/star-rating.md)
- [Creating a TailWind Button](javascript/react/easy_button.md)
- [Getting current URL](javascript/react/getting_url.md)
- [Calling APIs when needed with conditionally `useEffect`](javascript/react/calling_apis_with_useeffect.md)
- [Posting form data to an API](https://www.techomoro.com/submit-a-form-data-to-rest-api-in-a-react-app/)
- [PropTypes NPM package](https://www.npmjs.com/package/prop-types)
- [Rending Multiple checkboxes](https://www.geeksforgeeks.org/how-to-get-multiple-checkbox-values-in-react-js/)
- [`useState` lazy initialization](https://kentcdodds.com/blog/use-state-lazy-initialization-and-function-updates)
- [Using GA4 in an React App](javascript/react/using_ga4.md)

### React Router

- [Nested routes](https://reactrouter.com/docs/en/v6/getting-started/overview#nested-routes)
- [Index routes](https://reactrouter.com/docs/en/v6/getting-started/overview#nested-routes)
- [How to use Navigate](https://reactrouter.com/docs/en/v6/getting-started/overview#nested-routes)

### Vite

- [resolving global is not defined at node_modules](javascript/react/vite/global-nodes.md)
- [Changing the public base path](javascript/react/vite/base-path.md)
- [Enviroment variables](https://vitejs.dev/guide/env-and-mode.html#env-files)
- [Handling secondary pages in Vercel](javascript/react/vite/handling-secondary-pages.md)

### Vue

- [Adding TailwindCSS to Vue.js](javascript/vue/adding-tailwindcss-to-vue.md)
- [Required Key for `v-for`](javascript/vue/required-key.md)
- [Using Filters to format output](javascript/vue/using-filters.md)

# Markdown

- [Adding a anchor link](markdown/adding-an-achor-link.md)
- [Adding an image](markdown/adding-an-img.md)

# Monitoring

### Grafana

- [Grafana fundamentals](https://grafana.com/tutorials/grafana-fundamentals/)

### Prometheus

# Python

- [Advance set operations](python/advance-set-operations.md)
- [Append vs Extend with lists](python/append-vs-extend.md)
- [\*args and \*\*kwargs](python/args-and-kwargs.md)
- [Arithmetic operators](python/arithmetic-operators.md)
- [Difference between `is` & `==`](python/booleans.md)
- [Class inhertiance](python/class_inheritance.md)
- [Classes and instances](python/class-instances.md)
- [Class methods](python/class-methods.md)
- [Class variables](python/class-variables.md)
- [Connecting to SFTP via Paramiko](python/connecting-to-sftp.md)
- [Prevent Conda from activating the base environment by default](python/conda_deactivation.md)
- [Use of `continue` in a while loop](python/continue_while_loop.md)
- [Creating random numbers randomly](python/creating_random_numbers.md)
- [Pipenv](python/pipenv.md)
- [Using `.env` files within Python](python/using-env-files.md)
- [f-strings](python/f-strings.md)
- [Flow control with a while loop](python/flow_control.md)
- [Using the `global` statement](python/global_variable.md)
- [Using `enumerate()` in loops](python/enumerate_in_loops.md)
- [Use of `range()`](python/use_of_range.md)
- [Using `random.sample()`](python/random-sample.md)
- [Working with JSON](python/working_with_json.md)
- [End parameter in `print()`](python/end-parameter-in-print.md)
- [Dictionaries](python/dictionaries.md)
- [List Comprehension](python/list-comprehension.md)
- [Dictionary Comprehension](python/dict-comprehension.md)
- [Lambra functions](python/lambra-functions.md)
- [`map()` function](python/map-function.md)
- [Floor division](python/floor-division.md)
- [Dunder methods for classes](python/dunder_methods.md)
- [Instance, Class, and Static Methods](python/instance_class_static_methods.md)
- [Files using `with` statement](python/with_statement.md)
- [Uploading files via requests POST](https://docs.python-requests.org/en/latest/user/quickstart/#post-a-multipart-encoded-file)
- [Get pip installed executables into the asdf path](python/pip-installed-exe-into-the-asdf-path.md)
- [Handing a ZeroDivision Error](python/zero_division_error.md)
- [How to compare two files and out put difference](python/comparing_two_files.md)
- [Writing data on a single line to a txt file](python/writing_singleline_to_txt.md)
- [Connecting to PrestoDB via presto-python-client](databases/presto/connecting_to_presto.md)
- [Removing duplicates from dictonaries](python/removing_dupes_from_dicts.md)
- [Formatting a float](python/formatting_a_float.md)
- [Fixing UnboardLocalError](python/fixing_unboundlocalerror.md)
- [`logging` module basics](python/logging.md)
- [Creating a progess bar](python/progress_bar.md)
- [Disabling pylint warnings](python/disabling-pylint-warnings.md)
- [Using TOML with Python](python/using-toml-with-python.md)
- [Extracting substring with Regex](python/extracting-substring.md)
- [Returning the difference between two strings](https://www.educative.io/answers/how-to-return-the-difference-of-two-strings-in-python)
- [Disable Pylance warnings in VSC](python/disable-pylance.md)
- [Uploading files via a boto3 session](python/uploading_files_boto3.md)
- [Walrus operator](python/walrus.md)
- [Using placeholders for SQL in Python](databases/sql/placeholders.md)

### Django

- [Using the safe filter](python/django/using-safe-filter.md)

### FastAPI

- [Preventing 5xx errors with SessionLocal / Depends](python/fastapi/5xx_erros.md)
- [Serving static files](python/fastapi/serve-static-files.md)

### Flask

- [Fully reloading a browser to see CSS changes](python/flask/css-changes.md)
- [Getting user input from a form field](python/flask/getting_data_from_a_formfield.md)
- [Generated a SECRET_KEY with secrets](python/flask/secret_token_with_secrets.md)
- [Using `render_kw` to pass attributes in form fields](python/flask/using_render_kw.md)
- [Rendering `\n` from json in Jinja2](python/flask/rendering_newlines.md)

### Pandas 🐼

- [Installing Anaconda and using Pandas (M1 Mac)](python/pandas/conda_pandas.md)
- [Differences between DataFrame & Series](python/pandas/dataframes_series.md)
- [Use of `value_count()`](python/pandas/using_value_count.md)
- [Creating Dataframes with multi-dimensional lists](python/pandas/creating_dataframe_multi-dimensional-lists.md)
- [Access JSON in a Panads dataframe](python/pandas/accessing_json.md)
- [Merging Dataframes and removing duplicates](python/pandas/merge_remove_duplicates.md)
- [Renaming cols in dataframes](python/pandas/rename_cols.md)
- [Replacing null values with None](python/pandas/using_none_for_null_values.md)
- [Replacing `NaN` / `NA` with a value](python/pandas/replace_NaN.md)
- [Scraping table data from sites](python/pandas/scraping_tables.md)
- [Creating a pivot table](python/pandas/creating_pivot_table.md)
- [Using `to_numpy` with `tolist`](python/pandas/to-numpy-list.md)
- [Plotting Pivots with Plotly](python/pandas/plotting_pivots_ploty.md)
- [How to prevent scientific notation in pandas](python/pandas/scientific_notations.md)
- [Filtering out a DF](python/pandas/filtering_a_df.md)

### SQLAlchemy

- [Creating a database](python/sqlalchemy/setting_up_sqlalchemy.md)
- [Joins to a Target with an ON Clause](python/sqlalchemy/joins_target_on_claus.md)

### Testing

- [`assert` in Python](python/testing/assert.md)

# Rust

- [Using cargo](https://doc.rust-lang.org/book/ch01-03-hello-cargo.html)
- [Printing to the console](rust/printing.md)
- [Variables in Rust](rust/variables.md)

# Terminal

- [Creating a new file](terminal/creating-new-files.md)

### BASH

- [Using arguments in bash](terminal/bash/arguments.md)
- [Doing math in bash](terminal/bash/math.md)
- [Using variables in bash](terminal/bash/variables.md)
- [Using IF statements in bash](terminal/bash/if_statements.md)
- [Getting user input with `read` in bash](terminal/bash/read.md)
- [Running Linux commands within bash script](terminal/bash/running-cmds.md)
- [Conditionals in bash](https://linuxconfig.org/bash-scripting-conditionals)

### Vim

- [My cheatsheet](terminal/vim/cheatsheet.md)
- [Multiline edit](terminal/vim/multiline-edit.md)

# Sys Admin

- [Raid configs explained](sys-admin/raid.md)
