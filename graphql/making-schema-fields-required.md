# Make fields in a scheme required.

You're able to make some of the schema fields required by adding an exclamation point:

```javascript
const typeDefs = gql`
  type Highlight {
    id: ID!
    content: String!
    ...
  }
`;
```