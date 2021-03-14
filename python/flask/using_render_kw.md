# Using `render_kw` to add attributes into form fields

When you're forms fields they look like this when using Flask-WTF.

```html
{{ form.email(class="form-control is-invalid") }}
```

It's hard to see where you can add attributes like placeholers.

This is where `render_kw` comes in.

**render_kw (dict)** – If provided, a dictionary which provides default keywords that will be given to the widget at render time.

```python
email = StringField(
        "Email",
        validators=[DataRequired(), Email()],
        render_kw={"placeholder": "Email Address", "type": "email"},
    )
```

So now the email field within the form looks like this, using the `render_kw` to past other attributes. A simple login form could look like this:

```python
class LoginForm(FlaskForm):
    email = StringField(
        "Email",
        validators=[DataRequired(), Email()],
        render_kw={"placeholder": "Email Address", "type": "email"},
    )
    password = PasswordField(
        "Password",
        validators=[DataRequired()],
        render_kw={"placeholder": "Password", "type": "password"},
    )
    remember = BooleanField("Remember Me")
    submit = SubmitField("Login")
```

more [info](https://wtforms.readthedocs.io/en/2.3.x/fields/#the-field-base-class) here
