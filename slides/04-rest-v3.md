## Platform REST API
## Present, time anomalies and future


### Past & present
REST API has not been our focus since 1.0.

- Stabilizing
- Catching up on missing features
- v2 won't receive significant improvements


### Future
Rework will have to happen one day

API Platform as a basis / inspiration
- Modern formats
- Rely on Symfony Serialization
- Dynamically self-documented


### Time anomalies
### GraphQL prototype


### GraphQL primer
  - Query language created by Facebook
  - Strongly typed
  - Allows you to get exactly what you query
  - Reduces number of queries, a defect of the v2 REST API


```
{
  hero {
    name
    # Queries can have comments!
    friends {
      name
    }
  }
}
```


```
{
  "data": {
    "hero":
    {
      "name": "R2-D2",
      "friends": [
        {
          "name": "Luke Skywalker"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}
```


### Based on
- webonyx/graphql-php
- overblog/graphqlbundle
- uses outdated versions
- API Platform uses webonyx/graphql-php


### Is it good ?

### Yes.


- Surprisingly easy to implement
- Great usability
  - List all content types, with field definitions and types
  - Browsing content is really convenient
  - Types can be inspected when browsing


![Come here](https://media.giphy.com/media/26BRzQS5HXcEWM7du/giphy.gif)


#### Great FieldTypes flexibility
- GraphQL interfaces
- Interface per fieldtype
  - Specific variation of an image
  - HTML version of richtext
  - Name of related content


![Come here](https://media.giphy.com/media/26BRzQS5HXcEWM7du/giphy.gif)


### Limitations
- Limited to reading, no writing (mutations)
  - Not adapted to managing content
  - Can apply to lightweight application use-cases
    - comments
    - ratings
- Not optimised

