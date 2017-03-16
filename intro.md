# Give it a REST Oli...

### Agenda
- Basic Intro to REST APIs
- Simple Server
- Simple Client

Note:
- Start two command prompts and both servers (ports 9000 and 9001)
- Refresh the presentation
---
- I saw swaggerUI in company talks
- Going to share how with you
- You come away with knowledge and code
- Warning of puns :( Also Qs at end.
- But we'll look at one shortly, for now...

---

# Basic Intro to REST APIs

- Should be well defined
- Google, Microsoft, etc. have design guides
- Different things to different people
- Good way to start an internet argument
- I'm going to butcher it here

Note:
- There are definitions but still seems controversial
- You can go and read about them if you want specifics
- I'm just going to give a really quick 'what you need to know now' - *not how to build a perfect one*

===

## For Our Purposes...

- You make an HTTP request to e.g. `http://example.com/orders`
    - (maybe sending some JSON)
- Get back return code (and maybe JSON)
- ...
- Profit

Note:
- Assuming you all know what HTTP is (what you use to view web pages) and JSON (hope you know Python, but you can think of it as a Python dictionary/lists and basic types like ints / strings)

===

## More REST API Basics

- Client/Server
- Stateless
- *Resources* and *collections*
    - `/orders/123`
    - `/orders`
- Always named with nouns
- Collections always plural

Note:
- Communication is stateless between requests (each request contains all the data needed to handle the request)
- Two main endpoint types - resources and collections

---


## Basic Intro to REST -  Verbs

- `GET`
- `POST`
- `PUT`
- `DELETE`
- `PATCH`

Note:
- So you know about resources and collections - there are these 5 main things you can do to them.

===

## Verbs:  `GET`
- Get data
- What your web browser does

`GET` a collection:

```
GET /api/kittens
200

[1, 2, 3, 4]
```

`GET` a resource:

```
GET /api/kittens/1
200

{
    "name": "Boris",
    "fuzzy": true
}
```

Note:
- Perform a GET

===

## Verbs: `PUT`/`POST`

- Create / Update / Replace
- Different depending on who names things

|                | Create | Update/Replace
|----------------|-----|-----
|**Server Names**    | `POST /orders` | `PUT /orders/12345`
|**Client Names**    | `PUT  /kittens/boris` | `PUT /kittens/boris`


===

## Verbs: `DELETE`
Delete something

```
DELETE /api/kittens/boris

204
```

===

## Verbs: `PATCH`

- Change something
- You define how
- Don't care for this talk
- Just `GET` data, change it yourself, then `PUT` it back

---

# That skips a lot...

===

# ...Like

- Discoverability
    - HATEOAS
    - https://vimeo.com/20781278
- Idempotency
- [Best practices](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api)

---

# What is Swagger?

Two things

===

## It's a Spec

- Formally OpenAPI - [schema here](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#openapi-specification)
- Version 2 is current, version 3 was supposed to come out end Feb
- Specifies how to document your JSON API with a JSON schema
- We'll see one shortly.

===

## It's a UI

- SwaggerUI
- Standard demo is of a petstore: http://petstore.swagger.io/
- Lets you try out an API in your browser
