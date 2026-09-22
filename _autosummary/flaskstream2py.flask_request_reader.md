# flaskstream2py.flask_request_reader

FlaskRequest Source

### Classes

| [`FlaskRequestReader`](#flaskstream2py.flask_request_reader.FlaskRequestReader)([chk_size])   | A source reader that reads from an incoming HTTP request using Flask.   |
|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------|

### *class* flaskstream2py.flask_request_reader.FlaskRequestReader(chk_size=2048)

Bases: `SourceReader`

A source reader that reads from an incoming HTTP request using Flask.
Must be instantiated from a Flask request handler function to ensure
that the flask.request context is available.

```pycon
>>> from flask import Flask
>>> from flaskstream2py.flask_request_reader import FlaskRequestReader
>>> app = Flask(__name__)
```

```pycon
>>> def toy_callback(b: bytes, n_bytes_to_show=10):
...     print(f"Read {len(b)} bytes. First five {b[:n_bytes_to_show]}")
```

```pycon
>>> @app.route('/', methods=['POST'])
... def handle_stream():
...     reader = FlaskRequestReader()
...     with reader.open():
...         while True:
...             chk = reader.read()
...             toy_callback(chk)
```

#### close()

Close and clean up source reader.
Will be called when StreamBuffer stops or if an exception is raised during read and append
loop.

#### *property* info *: [dict](https://docs.python.org/3/builtins/stdtypes.html#dict)*

Returns the \_\_init_\_ arguments for the instance.

* **Returns:**
  dict

#### key(data)

Converts data into a comparable value to sort by

* **Parameters:**
  **data** – the return value of the ‘read’ method
* **Returns:**
  ComparableType

#### open()

Set up source to be read and set some source info affected by open time like the time of
open. Will be called in StreamBuffer immediately before first read.

#### read()

Reads raw bytes if the request body is not finished yet.
Raises a StopIteration exception if the request has finished.
