# Joi Validation for Express

When building out POST requests for APIs, they require validation. Preventing
the user from added something that's not valid into a database for example. This
is where [Joi](https://joi.dev/) comes in. This is a handy module that allows
you to create a simple validation for your POST requests.

```javascript
import Joi from 'joi'

// book schema
const schema = Joi.object({
  title: Joi.string().min(3).required(),
  author: Joi.string().min(3).required(),
  pageCount: Joi.number().integer().required(),
  isbn: Joi.string().required(),
})

app.post('/api/add-book', cors(corsOptions), async (req, res) => {
  // check for validation against book schema
  const result = schema.validate(req.body)

  // check for validation if no validation return 400
  if (result.error) {
    res.status(400).send({message: `${result.error.details[0].message}`})
    return
  }

  const book = {
    author: req.body.author,
    title: req.body.title,
    pagecount: parseInt(req.body.pagecount),
    isbn: req.body.isbn,
  }

  // insert book object into supabase db
  const {data, error} = await supabase.from('books-express').insert(book)

  if (error)
    res
      .status(400)
      .send({message: `ERROR CODE: ${error.code}: ${error.message}`})

  // return valid book object back to client
  if (data) res.send(book)
})
```

The above is a very simple use of Joi using a book object that only has four
datapoints. The Joi [docs](https://joi.dev/api/?v=17.6.0#introduction) are alo
very well laid out.
