If you are already using Wortelus' [ X-Plane Elgato Stream Deck Connector (ZIBO + more)](https://forums.x-plane.org/index.php?/files/file/82031-x-plane-elgato-stream-deck-connector-zibo-more/&page=3&tab=comments#comment-380838), installation and use of Cockpitdeck is simplified.
If we assume you have one or more Streamdeck devices (not the Streamdeck +) already working with the above software, you just need to install Cockpitdeck next to  *«X-Plane Elgato Stream Deck Connector (ZIBO + more)»*, in the same python environment but you need to add `Pillow` and `ruamel.yaml` Python packages:

```
$ pip install ruamel.yaml pillow
```

If you want to use the Metar/Weather icon, you need to add:
```
$ pip install 'avwx-engine[scipy]' suntime timezonefinder
```


Wortelus's application works in a similar way as Cockpitdeck and use the same other packages. Cockpitdeck should start right away.


First install or copy the deckconfig folder provided for Airbus A321 to X-Plane Airbus aircraft folder. Then start cockpitdeck:

```
$ cd cockpitdecks
$ python bin/start_cockpitdeck_udp.py path-to-folder-where-deckconfig-resides
```