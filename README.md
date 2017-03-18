sessions
========
[![GoDoc](https://godoc.org/github.com/gorilla/sessions?status.svg)](https://godoc.org/github.com/gorilla/sessions) [![Build Status](https://travis-ci.org/gorilla/sessions.png?branch=master)](https://travis-ci.org/gorilla/sessions)
[![Sourcegraph](https://sourcegraph.com/github.com/gorilla/sessions/-/badge.svg)](https://sourcegraph.com/github.com/gorilla/sessions?badge)


guotie/sessions provides cookie and filesystem and redis sessions and infrastructure for
custom session backends. This package is just for gin-gonic/gin, if you are looking
for some more generic implement, use gorilla/sessions instead.

The key features are:

* Simple API: use it as an easy way to set signed (and optionally
  encrypted) cookies.
* Built-in backends to store sessions in cookies or the filesystem.
* Flash messages: session values that last until read.
* Convenient way to switch session persistency (aka "remember me") and set
  other attributes.
* Mechanism to rotate authentication and encryption keys.
* Multiple sessions per request, even using different backends.
* Interfaces and infrastructure for custom session backends: sessions from
  different stores can be retrieved and batch-saved using a common API.

Let's start with an example that shows the sessions API in a nutshell:

```go
	import (
		"net/http"

		"github.com/guotie/sessions"
		"github.com/gin-gonic/gin"
	)

	var store = sessions.NewCookieStore([]byte("something-very-secret"))
	
	rediStore, err := NewRediStore(10, "tcp", ":6379", "", []byte("secret-key"))

	func MyHandler(c *gin.Context) {
		// Get a session. We're ignoring the error resulted from decoding an
		// existing session: Get() always returns a session, even if empty.
		session, _ := store.Get(c, "session-name")
		// Set some session values.
		session.Values["foo"] = "bar"
		session.Values[42] = 43
		// Save it before we write to the response/return from the handler.
		session.Save(c, w)
	}
```

First we initialize a session store calling `NewCookieStore()` and passing a
secret key used to authenticate the session. Inside the handler, we call
`store.Get()` to retrieve an existing session or a new one. Then we set some
session values in session.Values, which is a `map[string]interface{}`.
And finally we call `session.Save()` to save the session in the response.

More examples are available [on the Gorilla
website](http://www.gorillatoolkit.org/pkg/sessions).

## License

BSD licensed. See the LICENSE file for details.
