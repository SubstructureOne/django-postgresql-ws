django-postgresql-ws
--------------------

`django-postgresql-ws` is an alternative PostgreSQL backend for Django 
proxied over WebSockets. It will only work when configured to run against a 
WebSockets proxy that is then communicating with the PostgreSQL server. 
websockify is the standard WebSockets proxy service it is tested against.

The purpose of this backend is to allow a Django application to run against 
a PostgreSQL server in a WebAssembly (Pyodide) environment, where native 
sockets are 
not allowed but WebSockets are. The WebSocket communication is handled by 
the pgwasm library, which itself switches between using a the native 
websockets libray when being run in a native environment, and using the 
JS-proxied interface when run under Pyodide.

This backend is based on a copy of the PostgreSQL backend that 
ships with Django 4.1 with updates made to use `pgwasm` instead of 
`psycopg2`. Note that while Django does not natively support async in its 
implementation of its ORM, the database backend needs to be async in order 
to communicate over WebSockets in Pyodide, since the JS interface receives 
WebSockets messages via callbacks. The `async_to_sync` method provided by 
`asgiref` properly handle running the async methods in the proper event loop.
