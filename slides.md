# Give it a REST Oli...

### Agenda
- Basic Intro to REST
- Simple Server
- Simple Client

Note:
This is to me

---

# Basic Intro to REST

- You make a HTTP request
    - (maybe sending some JSON)
- Get back JSON
- ...
- Profit
- Different things to different people
- Cause of arguments

---

# Basic Intro to REST

## Verbs

- GET
- POST
- PUT
- PATCH
- DELETE

===

## Verbs

### GET
- Get data

===

## Verbs

### PUT/POST
- Create / Update / Replace / Modify
- This could be a table.

===

## Verbs

### DELETE
- Delete something

===

## Verbs

### PATCH
- Change something
- You define how
- Don't care for this

---

# That skips a lot

===

- discoverability: https://vimeo.com/20781278

---

# What is Swagger?

===

## It's a Spec

===

## It's a UI

---

# Writing a SwaggerUI Server

===

- Standard seems to be to generate from a spec
- Easier for me to write server and have server give the spec
- https://github.com/zalando/connexion seems to do it the other way if you prefer

===

- Use Flask-RESTPlus
- Easy to test: http://flask.pocoo.org/docs/0.11/testing/#the-testing-skeleton

---

# DEMO

http://127.0.0.1:5000

---

# Gotchas

===

- Prefix
- Expose schema
- Expect

===

- Use Flask-RESTPlus
- Easy to test: http://flask.pocoo.org/docs/0.11/testing/#the-testing-skeleton

---

# Writing a Swagger Client

===

- Standard seems to be code generation
- That sucks, just want to point client at a server

===

# Use pyswagger

---

# Questions?

Go here for links:

http://giveitarest.olipratt.co.uk
