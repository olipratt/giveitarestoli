## Recap

===

## Alternatives

- `Bravado` maintained by yahoo.

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
client = Client(Security(schema_path))
```

===

## Specifying Endpoints

- There are a couple of ways of doing it.

---

# Recap Overall

---

# Questions?

Go here for links:

http://giveitarest.olipratt.co.uk

---

# Plug

- Hypothesis testing
