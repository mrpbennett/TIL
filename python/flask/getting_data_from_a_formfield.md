# Getting data from a form field with Flask-WTF


#### Simple form

I created this form so a user could submit a url into a `GET` request using the request module.

```python
from flask_wtf import FlaskForm
from wtforms import StringField, SubmitField
from wtforms.validators import DataRequired


class SubmitURL(FlaskForm):
    sellers_json_url = StringField("URL", validators=[DataRequired()])
    submit = SubmitField("Get Data")
```

I had trouble getting the url from the form field after the user clicked the submit button. I kept on getting this error:

```
requests.exceptions.InvalidSchema: No connection adapters were found for Markup('<input id="sellers_json_url" name="sellers_json_url" required type="text" value="https://www.adyoulike.com/sellers.json">')
```

It looked like the submit button was submitted HTML into the request. After much googling I found what i was looking for.

[wtforms.form.Form.data](https://wtforms.readthedocs.io/en/2.3.x/forms/?highlight=data#wtforms.form.Form.data)

`data`
A **dict** containing the data for each field.

Note that this is generated each time you access the property, so care should be taken when using it, as it can potentially be very expensive if you repeatedly access it. Typically used if you need to iterate all data in the form. If you just need to access the data for known fields, you should use form.<field>.data, not this proxy property.
  
---

So knowing this, this is what my request looked like:

```python
@app.route("/", methods=["GET", "POST"])
def get_data():

    form = SubmitURL()
    sellers = {}

    if form.validate():
        url = form.sellers_json_url.data

        r = requests.get(url)
        if r.status_code == 200:
            data = r.json()
            sellers = data.get("sellers")
        else:
            print(f"ERROR: {r.status_code}")
 ````
  
  I tried a couple of ways of getting what I wanted. First I used `url = form.data` to see what the dict was storing, the dict returned:
  
  ```bash  
  "{'sellers_json_url': 'https://cdn.pubmatic.com/sellers/data/sellers.json', 'submit': True, 'csrf_token': 'xxx'}"
  ```
  
  As per the last line in the docs _you should use form.<field>.data, not this proxy property._ I ended up using the following to get the domain from the dict.
  
  ```python
  url = form.sellers_json_url.data
  ```
  
  And BOOM! it all worked perfectly.
