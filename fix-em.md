# Fix Em!

### Query Depth

In order to limit Query Depth in our Example we can do some changes to our code and add some constraints on it as follows

Install `graphql-depth-limit` and `express-graphql`

{% code-tabs %}
{% code-tabs-item title="install dependencies" %}
```bash
npm i graphql-depth-limit --save
npm i express-graphql --save
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Dept Limit" %}
```javascript
app.use('/graphql', graphqlHTTP((req, res) => ({
  schema,
  validationRules: [ depthLimit(5) ]
})))
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now we will execute same query from previous example and this is what we get

![Depth Limit Fixed](.gitbook/assets/screenshot-from-2019-03-20-19-20-26.png)



