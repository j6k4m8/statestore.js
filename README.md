# statestore.js
Store page data persistently across sessions, updated in realtime.

## Usage
- Create a new storage:
```javascript
let state = new LocalStorageStateMachine();
```

- Now we can load data into it:
```javascript
state.setState("name", "Zaphod");
state.setState("location", "Milliways");
```

- And get data out of it!
```javascript
state.getState("name")          // → "Zaphod"
```

- We can see everything in the store:
```javascript
state.saveState()               // → { name: "Zaphod", location: "Milliways" }
```

- Now we can create a new store, cloning the old, by passing in a dictionary (here, the one we just saved from the `.saveState` call) to the constructor:
```javascript
let newState = new URLStateMachine(
    state.saveState()
)
```

- Now the state will be represented in the URL bar, instead of in LocalStorage. That is, the URL will now look like this:
```
example.com/?name=Zaphod&location=Milliways
```

- When you save state to the URL store, the URL bar will be updated in realtime, with no refresh.

## Half-Hearted Documentation Effort

### `StateMachine`
A `StateMachine` stores key-value pairs in its backend. The default backend is simply a dictionary, and is pretty useless (not to mention, a really annoying way of dealing with a dictionary). 

It posesses the following functions:

- `setState(key, val) → undefined`: Essentially adds the entry `{ key: value }` to storage.
- `getState(key) → value`: Returns the value stored under `key`
- `loadState(dict) → undefined`: Loads all keys from a dictionary into the store, overwriting them if they already exist
- `saveState() → dictionary`: Returns all values that were held in the store. Good for exporting JSON :)

### `LocalStorageStateMachine`
Has all the same functions as above, but stores the 'backend' in `LocalStorage` in the browser, so that it's persistent across page refreshes. Works for all URLs under the same domain, so `example.com/foo` can set state, and `example.com/bar` can read from the same state. Possibly annoying, but also possibly ok.

### `URLStateMachine`
All the same functions as the above, but stores the backend in the URL of the current page. This is useful if you want users to be able to share their current session with a friend simply by sharing their URL. Mimics the realtime URL-update pattern seen in tools like Google Maps, where the URL updates to reflect the currents screen as soon as the user makes a change.
