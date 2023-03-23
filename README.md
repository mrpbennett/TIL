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
- [Databases](#databases)
    - [Presto](#presto)
    - [TimescaleDB](#timescaledb)
    - [SQL](#sql)
- [Datatypes](#datatypes)
- [DevOps](#devops)
- [Excel](#excel)
- [Linux](#linux)
    - [Kali](#kali)
    - [CentOS](#centos)
- [Git](#git)
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
- [Rust](#rust)
- [Vim](#vim)

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

### Kubernetes

# CCNA

- [Switches](ccna/switches.md)
- [TCP/IP OSI](ccna/tcpip-osi.md)

# Command Line

- [Creating a new file](command-line/creating-new-files.md)
- [Using variables in bash](command-line/variables.md)
- [Getting user input with `read` in bash](command-line/read.md)
- [Using arguments in bash](command-line/arguments.md)
- [Running Linux commands within bash script](command-line/running-cmds.md)
- [Doing math in bash](command-line/read.md)
- [Conditionals in bash](https://linuxconfig.org/bash-scripting-conditionals)

# Databases

### Presto

- [Connecting to PrestoDB via presto-python-client](databases/presto/connecting_to_presto.md)
- [Use of `regexp_like()`](databases/presto/regexp_like.md)

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
- [Sub Queries](databases/sql/sub-queries.md)

# Datatypes

- [JS / Python Classes](datatypes/classes.md)

# DevOps

- [90 Days of DevOps](devops/90-days-of-devops.md)

# Excel

- [Using the FV formula](excel/fv-formula.md)

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
- [How to Customize the Ubuntu Dock to Look Like macOS](https://www.makeuseof.com/configure-ubuntu-dock-to-look-like-macos/)

### Kali

- [ProxyChain](linux/kali/proxuy-chaining.md)

### CentOS

# Git

- [Creating & Switching branches](git/creating-and-switching-branches.md)
- [Pushing to a new remote repo](git/pushing-local.md)
- [Using git reset](git/git-reset.md)
- [How to find which commit introduced a bug](git/git-bisect.md)
- [Cleaning up branches in Git](git/cleaning_branches.md)
- [Cloning from a certain branch](git/clone_from_branch.md)

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

### React Router

- [Nested routes](https://reactrouter.com/docs/en/v6/getting-started/overview#nested-routes)
- [Index routes](https://reactrouter.com/docs/en/v6/getting-started/overview#nested-routes)
- [How to use Navigate](https://reactrouter.com/docs/en/v6/getting-started/overview#nested-routes)

### Vite

- [resolving global is not defined at node_modules](javascript/react/vite/global-nodes.md)
- [Changing the public base path](javascript/react/vite/base-path.md)

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

- [Pipenv](python/pipenv.md)
- [Using `.env` files within Python](python/using-env-files.md)
- [f-strings](python/f-strings.md)
- [Flow control with a while loop](python/flow_control.md)
- [Using the `global` statement](python/global_variable.md)
- [Using `enumerate()` in loops](python/enumerate_in_loops.md)
- [Use of `continue` in a while loop](python/continue_while_loop.md)
- [Use of `range()`](python/use_of_range.md)
- [Using `random.sample()`](python/random-sample.md)
- [Working with JSON](python/working_with_json.md)
- [End parameter in `print()`](python/end-parameter-in-print.md)
- [Arithmetic operators](python/arithmetic-operators.md)
- [Difference between `is` & `==`](python/booleans.md)
- [Dictionaries](python/dictionaries.md)
- [Append vs Extend with lists](python/append-vs-extend.md)
- [List Comprehension](python/list-comprehension.md)
- [Dictionary Comprehension](python/dict-comprehension.md)
- [Lambra functions](python/lambra-functions.md)
- [`map()` function](python/map-function.md)
- [\*args and \*\*kwargs](python/args-and-kwargs.md)
- [Floor division](python/floor-division.md)
- [Classes and instances](python/classes-instances.md)
- [Class variables](python/class-variables.md)
- [Class methods](python/class-methods.md)
- [Class inhertiance](python/class_inheritance.md)
- [Dunder methods for classes](python/dunder_methods.md)
- [Instance, Class, and Static Methods](python/instance_class_static_methods.md)
- [Advance set operations](python/advance-set-operations.md)
- [Files using `with` statement](python/with_statement.md)
- [Uploading files via requests POST](https://docs.python-requests.org/en/latest/user/quickstart/#post-a-multipart-encoded-file)
- [Static methods](python/static-methods.md)
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
- [Creating random numbers randomly](python/creating_random_numbers.md)
- [Disabling pylint warnings](python/disabling-pylint-warnings.md)
- [Using TOML with Python](python/using-toml-with-python.md)
- [Connecting to SFTP via Paramiko](python/connecting-to-sftp.md)
- [Extracting substring with Regex](python/extracting-substring.md)

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

### SQLAlchemy

- [Creating a database](python/sqlalchemy/setting_up_sqlalchemy.md)
- [Joins to a Target with an ON Clause](python/sqlalchemy/joins_target_on_claus.md)

# Rust

- [Using cargo](https://doc.rust-lang.org/book/ch01-03-hello-cargo.html)
- [Printing to the console](rust/printing.md)
- [Variables in Rust](rust/variables.md)`

# Vim

- [My cheatsheet](vim/cheatsheet.md)
- [Multiline edit](vim/multiline-edit.md)
