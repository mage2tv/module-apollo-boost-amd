# Apollo-Boost AMD module

The popular [apollo-boost](https://www.apollographql.com/docs/react/essentials/get-started#apollo-boost) library
transpiled to AMD and wrapped in a Magento 2 module.

## Usage:

* Install `mage2tv/module-apollo-boost-amd`

* In your JavaScript AMD modules, require `'apollo-boost'`

* Use the "exported" `ApolloClient` and `gql` function:

```
const client = new ApolloAmd.ApolloClient();
const query = ApolloAmd.gql(my_graphql_query);

// or: 

const {ApolloClient, gql} = ApolloAmd;
```

## Example:


```js
define(['uiComponent', 'apollo-boost'], function (Component, ApolloAmd) {
    'use strict';

    const {ApolloClient, gql} = ApolloAmd;

    const client = new ApolloClient({url: '/graphql'});
    const query = gql(`
                query exampleProducts($count: Int = 1) {
                  products(filter: {} pageSize: $count sort: { name: DESC }) {
                    total_count
                    items {
                      id
                      type_id
                      name
                      sku
                    }
                  }
                }
    `);


    return Component.extend({
        defaults: {
            tracks: {
                result: true
            }
        },
        initialize: function () {
            client.query({
                query: query,
                variables: {
                    count: 3
                }
            })
            .then(data => {
                this.result = data;
            })
            .catch(console.error);
            return this._super();
        }
    });
});
```