Rendering occurs as follow.

Request to render always starts from the Button. The Button solely calls the procedure render() on itself.

To render a Page for example, the Page will call render() on each button in turn. It may then call a supplemental procedure to fill unused icons with images.

The Button render() call is quickly transferred to the Deck for specific rendering handling. So the Button asks the Deck to render itself:

```py
    # In the Button class
	def render(self) -> None:
		self.deck.render(self)
```

From the button index, the Deck will deduce rendering possibilities. It will then ask the button to supply the data for the rendering.

```py
	# In the Deck class
	def render(self, button: Button) -> None:
		button_representation = button.get_representation()
		render_data = button_representation.render()
		# transform render_data if necessary
		# send it to device driver for handling (display, sound, coloring...)
		self.device.handle_data(render_data)
```

In other words, the representation of the button is responsible to generate and provide whatever is necessary for the deck device driver.

Let us take a very practical example.

The deck device driver has a function to display an iconic image on a key. That function expects the index of the key to designate it unambiguously, and an image in JPG format with the proper size 90 × 90 pixels.

If we use a generic representation that produces an image, the resulting image will be a 256 × 256 pixel transparent PNG image. We need to convert and resize that image before we can send it to the device driver.

```
	def render(self, button: Button) -> None:
		brep = button.get_representation()
		render_image = brep.render()
		# transform render_data if necessary
		keyicon = render_image.resize([90, 90]).convert("RGB")
		# send it to device driver for handling (display, sound, coloring...)
		self.device.set_key_image(key=button.inedx, image=keyicon)
```

This is a very simple example, but it shows the flow of information and the sequence of calls to get the work done.

To emit a sound, the representation render() function would be responsible for generating, for example, a sound file in WAV format. The deck render function would then pass that sound to the device driver play_sound() function that expects a sound file to play.

To color a LED, the representation render() function would be responsible for generating a color and a light intensity (in 0..1 value range). THe deck render function would then pass those data to the device driver turn_led_on(color, intensity) function to submit appropriate instruction to the device to turn the light on with the desired color and intensity.

People familiar with internet web development knowledge can follow the process for (Virtual) Web Decks. The device driver of Web Deck (called VirtualDeck) is a relatively straightforward JSON message generator and ultimately sends the message to the browser for display. In the browser, JavaScript process decodes the message and interpret it to display an image, play a sound, or refresh an entire page. (Communication occurs in a WebSocket channel.)
