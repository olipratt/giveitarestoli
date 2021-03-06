## Recap

- `pyswagger.App` wraps the schema to allow you to work with it
- `Client` takes a prepared request generated by an operation from the `App`

---

# Gotchas

===

## Testing

To load a schema from file - add this little snippet to it.

```json
{
  "host": "127.0.0.1:5000",
   "schemes":[
      "http"
   ],
   ...
}
```

===

## Security

- *"Oh but I'm not using any security"*
- Docs lie

```
client = Client(Security(App(schema_path)))
```

===

## Documentation

- Not great - esp. compared to Flask-RESTPlus
- I've spent a lot of time reading the source of the project
- Not particularly pythonic - e.g. the `App.s` operation
- But it does seem solid and does fully implement the specs

---

# Questions?

Go here for links:

http://giveitarest.olipratt.co.uk

Including:

- Source code for microstore and deckstore apps - copy-paste freely :)
- Articles on writing the apps
- Link to these slides
- More general REST API notes
- Article on running the apps in containers and joining them together with `docker-compose`
    - App repositories contain `Dockerfile`s

---

# Plug!

- https://github.com/olipratt/swagger-conformance
- Combine `pyswagger` and `hypothesis` to fully test and API
- Barely works but check it out if interested
