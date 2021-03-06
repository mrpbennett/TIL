# :books: TIL (Today I Learnt)

Inspired by @jbranchaud's [repo](https://github.com/jbranchaud/til) and @thoughtbot's [repo](https://github.com/thoughtbot/til)

This project is a collection of small things I learn about different languages, frameworks and technologies.
It consists of short markdown documents summarising concepts or tips I'd like to share.
The goal is to write something new everyday or as often as possible.

-   [Categories](#categories)

---
# Categories

- [:books: TIL (Today I Learnt)](#books-til-today-i-learnt)
- [Categories](#categories)
- [Applications](#applications)
    - [DataGrip](#datagrip)
- [Automation / build](#automation--build)
- [Command Line](#command-line)
- [Containters](#containters)
    - [Docker](#docker)
- [Databases](#databases)
    - [Presto](#presto)
    - [TimescaleDB](#timescaledb)
    - [SQL](#sql)
- [Git](#git)
- [GraphQL](#graphql)
- [HTML 5](#html-5)
- [CSS](#css)
- [JavaScript](#javascript)
    - [Node](#node)
    - [React](#react)
    - [Vue](#vue)
- [Markdown](#markdown)
- [Python](#python)
    - [Django](#django)
    - [Flask](#flask)
    - [Pandas](#pandas)
    - [Matplotlib](#matplotlib)

---

# Applications

### DataGrip
- [Setting up for multple schemas](applications/datagrip/multiple_schemas.md)

# Automation / Build / Containers

-   [Creating a Minified CSS file from Scss](automation-build/minicss-from-sass.md)
-   [Adding a Chronos job](automation-build/chronos.md)

### Docker
- [Basic build / push process](containers/docker/build_process.md)
-   [Using Docker with Flask](containers/docker/flask_docker.md)
-   [Building using x86](https://blog.jaimyn.dev/how-to-build-multi-architecture-docker-images-on-an-m1-mac/#tldr;)

# Command Line

-   [Creating a new file](commandLine/creating-new-files.md)
-   [zsh-syntax-highlighting](commandLine/installing-zsh-syntax-highlighting.md)
-   [Creating new ssh files (outside of github)](commandLine/ssh-file-gen.md)

# Databases

### Presto

- [Connecting to PrestoDB via presto-python-client](databases/presto/connecting_to_presto.md)
- [Use of `regexp_like()`](databases/presto/regexp_like.md)

### TimescaleDB

- [Installing TimescaleDB in Docker](databases/timescaledb/timescaledb.md)

### SQL

-   [Install mySQL via Homebrew](databases/sql/installing-mysql-via-homebrew.md)
-   [Changing a users password](databases/sql/changing-password.md)
-   [Setting a default value](databases/sql/setting-default-values.md)
-   [Inserting data into tables](databases/sql/inserting-data-into-tables.md)
-   [Auto increment IDs](databases/sql/auto-increment-primary-ids.md)
-   [Deleting a database](databases/sql/deleting-a-database.md)
-   [Finding & Deleting db users](databases/sql/finding-users-and-removing.md)

# Git

-   [Creating & Switching branches](git/creating-and-switching-branches.md)
-   [Using git reset](git/git-reset.md)
-   [How to find which commit introduced a bug](git/git-bisect.md)
-   [Cleaning up branches in Git](git/cleaning_branches.md)

# GraphQL

-   [Switching GraphQL IDEs](graphql/switching-graphQL-IDEs.md)
-   [Making required fields in a schema](graphql/making-schema-fields-required.md)

# HTML 5

-   [Bullet point](html/bullet.md)
-   [What is a polyfill](html/polyfill.md)
-   [Accessibility labels](html/accessibilityLabels.md)

# CSS

-   [box-sizing](css/box-sizing.md)
-   [animation-fill-mode](css/animation-fill-mode.md)
-   [animation-iteration-count](css/animation-iteration-count.md)

# JavaScript

-   [Default arguments](javascript/default-arguments.md)
-   [Difference between `const` & `let`](javascript/difference-let-const.md)
-   [Objects with functions](javascript/objects-with-functions.md)
-   [String methods](javascript/string-methods.md)
-   [Number methods](javascript/number-methods.md)
-   [`.forEach` and it's usage](javascript/forEach.md)
-   [`.splice()` and it's usage](javascript/splice.md)
-   [`.filter()` and it's usage](javascript/filter-array.md)
-   [`.map()` and it's usage](javascript/map-array.md)
-   [`sort()` and it's usage](javascript/sort-method.md)
-   [DOM Query selectors](javascript/query-selectors.md)
-   [Callback functions](javascript/callback-functions.md)
-   [How `this` works in arrow funcs](javascript/this-arrow-funcs.md)
-   [Using JSON](javascript/stringify-and-parse.md)
-   [`.toLocaleString`](javascript/tolocalestring.md)
-   [Replacing `\n` in html](javascript/replacing_newline_jsonstring.md)
-   [Disabling Web Security (CORS) for Chrome](javascript/disable_web.md)

### Node

-   [automatically restarting the node application](javascript/node/nodemon.md)
-   [Using local env variables](javascript/node/local-enviroment-variables.md)

### React

-   [Rendering lists using `.map()`](javascript/react/rendering-lists-with-map.md)
-   [How to use inline styles](javascript/react/using-inline-styles.md)
-   [Creating a star rating](javascript/react/star-rating.md)
-   [Creating a TailWind Button](javascript/react/easy_button.md)

### Vue

-   [Adding TailwindCSS to Vue.js](javascript/vue/adding-tailwindcss-to-vue.md)
-   [Required Key for `v-for`](javascript/vue/required-key.md)
-   [Using Filters to format output](javascript/vue/using-filters.md)

# Markdown

-   [Adding a anchor link](markdown/adding-an-achor-link.md)
-   [Adding an image](markdown/adding-an-img.md)

# Python

-   [Pipenv](python/pipenv.md)
-   [Using `.env` files within Python](python/using-env-files.md)
-   [f-strings](python/f-strings.md)
-   [Flow control with a while loop](python/flow_control.md)
-   [Using the `global` statement](python/global_variable.md)
-   [Using `enumerate()` in loops](python/enumerate_in_loops.md)
-   [Use of `continue` in a while loop](python/continue_while_loop.md)
-   [Use of `range()`](python/use_of_range.md)
-   [Working with JSON](python/working_with_json.md)
-   [End parameter in `print()`](python/end-parameter-in-print.md)
-   [Arithmetic operators](python/arithmetic-operators.md)
-   [Difference between `is` & `==`](python/booleans.md)
-   [Dictionaries](python/dictionaries.md)
-   [List Comprehension](python/list-comprehension.md)
-   [Dictionary Comprehension](python/dict-comprehension.md)
-   [Lambra functions](python/lambra-functions.md)
-   [`map()` function](python/map-function.md)
-   [\*args and \*\*kwargs](python/args-and-kwargs.md)
-   [Floor division](python/floor-division.md)
-   [Classes and instances](python/classes-instances.md)
-   [Class variables](python/class-variables.md)
-   [Class methods](python/class-methods.md)
-   [Class inhertiance](python/class_inheritance.md)
-   [Dunder methods for classes](python/dunder_methods.md)
-   [Instance, Class, and Static Methods](python/instance_class_static_methods.md)
-   [Advance set operations](python/advance-set-operations.md)
-   [Files using `with` statement](python/with_statement.md)
-   [Static methods](python/static-methods.md)
-   [Get pip installed executables into the asdf path](python/pip-installed-exe-into-the-asdf-path.md)
-   [Handing a ZeroDivision Error](python/zero_division_error.md)
-   [How to compare two files and out put difference](python/comparing_two_files.md)
-   [Writing data on a single line to a txt file](python/writing_singleline_to_txt.md)
-   [Connecting to PrestoDB via presto-python-client](databases/presto/connecting_to_presto.md)
-   [Removing duplicates from dictonaries](python/removing_dupes_from_dicts.md)
-   [Formatting a float](python/formatting_a_float.md)
-   [Fixing UnboardLocalError](python/fixing_unboundlocalerror.md)
-   [logging module basics](python/logging.md)

### Django

-   [Using the safe filter](python/django/using-safe-filter.md)

### Flask

- [Fully reloading a browser to see CSS changes](python/flask/css-changes.md)
- [Getting user input from a form field](python/flask/getting_data_from_a_formfield.md)
- [Generated a SECRET_KEY with secrets](python/flask/secret_token_with_secrets.md)
- [Using `render_kw` to pass attributes in form fields](python/flask/using_render_kw.md)
- [Rendering `\n` from json in Jinja2](python/flask/rendering_newlines.md)

### Pandas

- [Installing Anaconda and using Pandas (M1 Mac)](python/pandas/conda_pandas.md)
- [Differences between DataFrame & Series](python/pandas/dataframes_series.md)
- [Use of `value_count()`](python/pandas/using_value_count.md)
- [Creating Dataframes with multi-dimensional lists](python/pandas/creating_dataframe_multi-dimensional-lists.md)
- [Access JSON in a Panads dataframe](python/pandas/accessing_json.md)
- [Merging Dataframes and removing duplicates](python/pandas/merge_remove_duplicates.md)
- [Replacing null values with None](python/pandas/using_none_for_null_values.md)
- [Replacing `NaN` / `NA` with a value](python/pandas/replace_NaN.md)

### Matplotlib
