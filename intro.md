# Give it a REST Oli...

### Agenda
- Basic Intro to REST
- Simple Server
- Simple Client

Note:
This is to me

---

# Basic Intro to REST

- Different things to different people
- Good way to start an internet argument
- I'm going to butcher it here

===

## For Our Purposes...

- You make an HTTP request
    - (maybe sending some JSON)
- Get back JSON
- ...
- Profit

===

## More Basics

- Stateless
- Resources and collections
- Always nouns
- Always plural

---

## Basic Intro to REST -  Verbs

- GET
- POST
- PUT
- PATCH
- DELETE

===

## Verbs

### GET
- Get data
- What your web browser does

```
GET /api/kittens/1

200

{
    "name": "Boris",
    "fuzzy": true
}
```

===

## Verbs

### PUT/POST
Create / Update / Replace

|                | Create | Update/Replace
|----------------|-----|-----
|Server Names    | `POST /kittens` | `PUT /kittens/12345`
|Client Names    | `PUT /kittens/boris` | `PUT /kittens/boris`


===

## Verbs

### DELETE
Delete something

```
DELETE /api/kittens/boris

200
```

===

## Verbs

### PATCH
- Change something
- You define how
- Don't care for this talk

---

# That skips a lot...

===

# ...Like

- Discoverability
    - HATEOAS
    - https://vimeo.com/20781278
- Idempotency

---

# What is Swagger?

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
