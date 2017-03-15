# Recap

- Setup same as a basic Flask `app`, then wrap in `flask_restplus.Api`
- Define namespaces to group resources and collections
- Define resources and their operations
- Define models which specify the format of any data you will send or receive
- Add documentation, specify return codes, etc.

===

## Alternatives

- Standard seems to be to generate code from an OpenAPI spec
- Easier for me to write server and have server give the spec
- (Though I concede that will have advantages - better designed API)
- https://github.com/zalando/connexion seems to do it the other way if you prefer

===

## Testing

- Docs are great: https://flask-restplus.readthedocs.io/en/stable/index.html
- Builds directly on flask
- Easy to test: http://flask.pocoo.org/docs/0.11/testing/#the-testing-skeleton

---

# Gotchas

===

## Use an API Prefix

##### https://github.com/noirbizarre/flask-restplus/issues/210

```python
app = Flask(__name__)

# Use a non-empty 'prefix' (becomes swagger 'basePath') for
# interop reasons - if it's empty then the basePath is '/',
# which with an API enpoint appended becomes '//<endpoint>'
# (because they are always prefixed themselves with a
# '/') and that is not equivalent to '/<endpoint'.
api = Api(app,
          version='1.0',
          title='Simple Datastore API',
          description='A simple REST datastore API',
          prefix='/api')
```

Note:
- This is the actual code
- Raised an issue, but no traction.
- Probably never a problem on a real system.

===

## Expose the schema manually

```
schema_ns = api.namespace('schema',
                          description="This API's schema operations")

@schema_ns.route('')
class SchemaResource(Resource):
    """Resource allowing access to the
       OpenAPI schema for the entire API."""

    def get(self):
        """
        Return the OpenAPI schema.
        """
        return api.__schema__
```

===

## Input Validation isn't Enabled by Default

Either enable it per-operation:

```
@api.expect(AppData, validate=True)
@api.response(204, 'App successfully updated.')
def put(self, appid):
```

Or enable it globally:

```
# Globally enable input validation.
app.config['RESTPLUS_VALIDATE'] = True
```

---

# Writing a Swagger Client

===

## How will we do it?

- Standard seems to be code generation
- That sucks, just want to point client at a server

===

# Use pyswagger

- Other alternatives like `bravado`, but needs `twisted`
- Not so well documented, but does fully implement/adhere to specs
- Active
