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

===

# Basic Intro to REST

- Resources and collections
- Always plural

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

- Discoverability: https://vimeo.com/20781278

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
- Standard demo is [here](http://petstore.swagger.io/) *MAYBE SKIP*
- Lets you try out an API in your browser

---

# DEMO

- Simple Datastore
- Exposes SwaggerUI

http://127.0.0.1:9000

https://github.com/olipratt/microstore

===

<iframe style="background: #FFFFFF; width: 100%; height: 70vh;"
        src="http://127.0.0.1:9000">
</iframe>

---

## Setup

```python
from flask import Flask, request
from flask_restplus import Resource, Api, fields

app = Flask(__name__)

api = Api(app,
          version='1.0',
          title='Simple Datastore API',
          description='A simple REST datastore API',
          prefix='/api')

# This collects the API operations into named groups under a root URL.
apps_ns = api.namespace('apps', description='App data related operations')
schema_ns = api.namespace('schema', description="This API's schema operations")
```

<iframe style="background: #FFFFFF; width: 100%; min-height: 15vh;"
        src="http://127.0.0.1:9000">
</iframe>



Note:
- Flask stuff is standard for flask app
- The point is that everything you see is configured by you in the code.

===

## Simple Get

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

<iframe style="background: #FFFFFF; width: 100%; height: 20vh;"
        src="http://127.0.0.1:9000">
</iframe>

===

## Collection

```
@apps_ns.route('')
class AppsCollection(Resource):
    """ Collection resource containing all apps. """

    @api.marshal_list_with(AppName)
    def get(self):
        """
        Returns the list of apps.
        """
        log.debug("Listing all apps")
        apps_list = kvstore.keys(KVSTORE_NAMESPACE_APPS)
        return [{'name': app_name} for app_name in apps_list]
```

===

## PUT

```python
@apps_ns.route('/<appid>')
@api.response(404, 'App not found.')
class AppsResource(Resource):
    """ Individual resources representing an app. """

    @api.expect(AppData, validate=True)
    @api.response(204, 'App successfully updated.')
    def put(self, appid):
        """
        Updates an app.
        Use this method to add, or change the data stored for, an app.
        * Send a JSON object with the new data in the request body.
        `` `
        {
          "data": {
            "any_data": "you_like_here"
          }
        }
        `` `
        * Specify the name of the app to modify in the request URL path.
        """
        kvstore.store(KVSTORE_NAMESPACE_APPS,
                      appid,
                      request.get_json()['data'])
        return None, 204
```

===

## Models

```python
# Specifications of the objects accepted/returned by the API.
AppName = api.model('App name', {
    'name': fields.String(required=True,
                          description='App name',
                          example="My App"),
})

AppData = api.model('App data', {
    'data': fields.Raw(required=True,
                       description='App data',
                       example={"any_data": "you_like_goes_here"}),
})

AppWithData = api.inherit('App with data', AppName, AppData)
```

---

# Recap

- Setup same as a basic Flask `app`, then wrap in `flask_restplus.Api`
- Define namespaces to group resources and collections
- Define resources and their operations
- Define models which specify the format of any data you will send or receive
- Add documentation, specify return codes, etc.

===

## Alternatives

- Standard seems to be to generate from a spec
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

- Standard seems to be code generation
- That sucks, just want to point client at a server

===

# Use pyswagger

- Other alternatives like bravado, but needs twisted
- Not so well documented, but does fully implement/adhere to specs
- Active

---

# Simple client

===

## App

```
from pyswagger import App, Security
from pyswagger.contrib.client.requests import Client
# pyswagger makes INFO level logs regularly by default, so lower its logging
# level to prevent the spam.
logging.getLogger("pyswagger").setLevel(logging.WARNING)


log = logging.getLogger(__name__)


class DeckStore:
    """Definition of the datastore which client connections are made to."""

    def __init__(self, schema_path):
        """Create a deck store.

        :param schema_path: The url or file path of the swagger schema of the
                            datastore to use.
        :type schema_path: str.
        """
        # Load Swagger schema.
        self._app = App.create(schema_path)

        # Expose references to API operations that will be used.
        # These are objects that when called return correctly formatted
        # requests for client instances to send on to the server.
        # There are two ways to reference these - by name in the schema, or by
        # path within the API and operation - both are shown for completeness.
        # self.get_decks_list = self._app.op['get_apps_collection']
        self.get_decks_list = self._app.s('apps').get
        # self.put_deck_data = self._app.op['put_apps_resource']
        self.put_deck_data = self._app.s('apps/{appid}').put
        # self.get_deck = self._app.op['get_apps_resource']
        self.get_deck = self._app.s('apps/{appid}').get
        # self.delete_deck = self._app.op['delete_apps_resource']
        self.delete_deck = self._app.s('apps/{appid}').delete
```

===

## Client

```
class DeckStoreClient:

    def __init__(self, deck_store):
        """Create a new client to access the deck store.
        :param deck_store: The definition of the store to connect to.
        :type schema_path: DeckStore.
        """
        # Load Swagger schema.
        self._deck_store = deck_store

        # Create a client which will send requests
        self._client = Client(Security(self._deck_store))

    def new_deck(self):
        """Create a new deck, and return its ID."""
        # Just give each deck a truncated integer UUID as an ID for now.
        # Use uuid4 as that's random, and hopefully won't collide in test use
        # which is all this is really for.
        deck_id = uuid.uuid4().int % 100000000
        log.debug("Creating new deck with ID: %r", deck_id)

        prepared_request = self._deck_store.put_deck_data(appid=deck_id,
                                                          payload={"data": {}})
        resp = self._client.request(prepared_request)
        assert resp.status == 204
        log.debug("Deck created successfully")

        return deck_id
```

---

# Gotchas

===

- Testing
- How to specify endpoints
- Security

===

- Use Flask-RESTPlus
- Easy to test: http://flask.pocoo.org/docs/0.11/testing/#the-testing-skeleton

---

# Questions?

Go here for links:

http://giveitarest.olipratt.co.uk
