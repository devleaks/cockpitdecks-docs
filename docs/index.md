# Welcome to Cockpitdecks

Cockpitdecks is an application to use *decks* connected to a computer with X-Plane flight simulator.

![[deck-simulator2.png|600]]

The following make and models of decks have been tested and are known to work with Cockpitdecks:

- [Elgato Stream Deck](https://www.elgato.com/us/en/s/welcome-to-stream-deck) (all models work: Small, Mk.2, XL, Plus, and Neo)
- [Loupedeck Loupedeck Live](https://loupedeck.com/products/loupedeck-live/) (Loupedeck Live S, Loupedeck+, Loupedeck CT should also work but have not been tested; the LoupedeckLive «clone»  [Razer stream controller](https://www.razer.com/mena-en/gaming-accessories/razer-stream-controller) also seems to work.)
- [Berhinger X-Touch Mini](https://www.behringer.com/product.html?modelCode=0808-AAF)

Cockpitdecks also provides *Web Decks*, an emulator of the above decks in a web page, in the context of this application. Yes, you read it, you can now have any of the above deck, reproduced on a tablet, to use with X-Plane flight simulator. For free.

![[web-deck-intro.png|600]]

Cockpitdecks can run on the same computer as X-Plane, or a remote computer, network-connected to X-Plane.

> [!WARNING]
> The latest version of Cockpitdecks (release 15 and above) **requires** the latest version of X-Plane, 12.1.4, because Cockpitdecks now exclusively relies on the new Web API (and no longer on UDP). Please make sure you update X-Plane to the latest release before installing and using Cockpitdecks.

You can now proceed to the  [[Introduction|discovery of Cockpitdecks]], or directly jump to the [[Cockpit]] that holds all your decks. If you are impatient to try Cockpitdecks, head for [[Installation]]. If you want to learn more about Cockpitdecks and why it exists, head [[History|here]].

**Post Scriptum**

Cockpitdecks currently only connects to [X-Plane](https://www.x-plane.com) flight simulator. However, hooks are ready to connect to other simulators, like [Microsoft Flight Simulator](https://www.flightsimulator.com/). Packages like [pysimconnect](https://github.com/patricksurry/pysimconnect) can be used to bridge Cockpitdecks and MSFS. Unfortunately, these packages only run on Microsoft Window platform because they require MSFS SDK only available on the MS Window platform. If a developer want to create the MSFS Cockpitdecks plugin for Microsoft Flight Simulator, please contact me.

In this respect, X-Plane and Cockpitdecks, which run on MS Windows, MacOS, and Linux, and accept interaction through network ports, especially the recent Web API addition, are a far more open to extensions. This is also why Cockpitdecks can run on a remote host, send or receive its instructions to/from the simulator through network connections.
