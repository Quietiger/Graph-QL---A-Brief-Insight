---
description: Customizing the application to add more functionality
---

# Extending the example

Earlier it searched all available books now we will customize it to search only for books we want based on title \(I am not using ID or ISBN as primary key.. it can also be done just minor modifications to schema\)

{% code-tabs %}
{% code-tabs-item title="src/index.js" %}
```javascript
type Query { 
    allbooks: [Book]
    findbookbytitle(title: String!):[Book]
  }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Add the resolver

{% code-tabs %}
{% code-tabs-item title="src/index.js" %}
```javascript
findbookbytitle: (root,{title}) => {
      return books.filter(books => {
        return books.title === title;
      })
    },
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="success" %}
We can also extend the search based on different fields
{% endhint %}

