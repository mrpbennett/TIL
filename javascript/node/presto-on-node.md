# Presto Node Client

This is how you can set up Presto via Node, below is using the [presto-node-client](https://github.com/tagomoris/presto-client-node) from [tagomoris](https://github.com/tagomoris)

One issue I ran into was not implementing the user within the `Client` without this the client will use your ENV variables, mainly taking your computers user name. Therefore you must add a user within the client like below:

```javascript
var client = new presto.Client({user: 'myname'});
```

You can see that I have built it out in the client below...

```javascript
import {Client} from 'presto-client'

const client = new Client({
  host: 'lga-presto-adhoc.xxx.com',
  ssl: {
    rejectUnauthorized: true,
  },
  port: 8443,
  catalog: 'gridhive',
  schema: 'rpt',
  source: 'nodejs-client',
  user: 'pbennett',
  basic_auth: {user: 'pbennett', password: 'xxx'},
})

const query = 'some query'

function getPrestoData(query) {
  return new Promise((resolve, reject) => {
    client.execute({
      query: query,
      data: (error, data) => {
        if (error) {
          reject(error)
        } else {
          resolve(data)
        }
      },
      callback: error => {
        if (error) reject(error)
      },
    })
  })
}
```

### How to call the Promise

You could potentially use this promise in React like so.

```javascript
function App() {
  const [data, setData] = useState([])

  const getData = async () => {
    await getPrestoData(query)
      .then(data => setData(data))
      .catch(error => console.log(error))
  }
```

OR just as a plain Promise

```javascript
getPrestoData(query)
    .then(data => {
        //do something with data
    }
).catch(error => console.log(error))
```
