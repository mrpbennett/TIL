# Switching GraphQL IDEs

When using Gatsby I like to switch my GraphQL IDEs from [GraphiQL](https://github.com/graphql/graphiql) to [GraphQL Playground](https://github.com/prisma-labs/graphql-playground) as I prefer the UI and for me it's easier to navigate through the documentation. 

To do this you simple add `GATSBY_GRAPHQL_IDE=playground` into your `.env` file. You will be need to have the NPM package `dotenv` install within your enviroment though.

For Gatsby I add `require('dotenv').config();` to the top of my `gatsby-config.js` file.