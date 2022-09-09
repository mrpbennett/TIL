# Setting up express with CORS

Setting up
[CORS](https://expressjs.com/en/resources/middleware/cors.html#configuration-options)
is pretty simple with Express. Below is how I have managed to do it, with
allowing any address call my endpoints. For testing purpose this is the easiest
way for me.

```javascript
const express = require('express')
const cors = require('cors')
const app = express()
const PORT = 3000

app.use(cors())

const corsOptions = {
  origin: '*',
  methods: 'GET',
  optionsSuccessStatus: 200,
}

const user = [
  {
    id: 1,
    name: 'Paul Bennett',
    age: 37,
    jobTitle: 'Senior Solutions Architect',
    location: 'Remote',
  },
]

app.get('/api/user', cors(corsOptions), (req, res) => {
  res.status(200).json(user)
})

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`)
})
```
