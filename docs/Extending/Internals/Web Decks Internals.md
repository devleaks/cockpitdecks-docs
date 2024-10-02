

*Web Decks* are designed with simple standard web features, are rendered on an HTML Canvas, uses standard events to report interaction through basic JavaScript functions.
The application that serves them is a very simple Flask application with Ninja2 templates. The Flask application also runs the WebSocket proxy.

![[webdecks.svg|600]]

## Web Deck Messages

### Cockpitdecks to Web Deck Messages

#### Send code

```js
{
    "code": code,
    "deck": name,
    "meta": {
        "ts": datetime.now().timestamp()
    }
}
```

Code is interpreted by the deck. Codes are:

- 0: Display (attached) image on deck/key
- 1: Reload yourself (deck)
- 2: Emit (attached) sound for deck

`meta` data is a python dictionary serialized into JSON (mostly name, value pairs). It can contain any arbitrary serialisable items.

#### Send Image

It can be a key image, or a «hardware» image.

```js
{
    "code": 0,
    "deck": name,
    "key": key,
    "image": base64.encodebytes(content).decode("ascii"),
    "meta": {
        "ts": datetime.now().timestamp()
    }
}
```

#### Send Sound

It can be a key image, or a «hardware» image.

```js
{
    "code": 2,
    "deck": name,
    "sound": base64.encodebytes(content).decode("ascii"),
    "type": "wav",
    "meta": {
        "ts": datetime.now().timestamp()
    }
}
```

### Web Decks to Cockpitdecks Messages

#### Send code

```js
{
    "code": code,
    "deck": name,
}
```

Code values:

- 0: Report event (see below)
- 1: New deck, expect reload/refresh data

#### Send Event

```js
{
	"code": 0,
	"deck": deck,
	"key": key,
	"event": value,
	"data": data
}
```
