It started with a standard Stream Deck device. I found it nice to be able to see result of interaction with the deck right away on the deck.

I only fly Airbus, so I wanted to see annunciators switches on my deck. On panel at a time.

So I started to cut icons from an overhead screen dump. I quickly noticed some repetitive annunciators. But also numerous wordings changes. Label above annunciators would also change of course. I did not want to end up with a thousand icons, named by some clever convention. So I gave up fairly quickly on image processing and static icons and headed for a mechanism to dynamically create images with words laid over. I elaborated a set of design conventions that resulted in image generation.

Convention evolved quickly as I wanted to not only be able to generate annunciators but also switches, rotary knobs, and display some numbers like FCU data, fuel tank levels, etc.

Then I discovered the Loupedeck device. More elaborate with encoders and touch screens. I wanted to be able to handle those devices as well.

I am the happy owner of an inexpensive Behringer X-Touch Mini that I wanted to handle with my development too, even if it was not capable of showing images, it had other LED to communicate.

This led, one button at a time to what Cockpitdecks is today.

One, a gentleman approached me with questions about the configuration of a deck. I was first surprised to see that somebody else was successfully using my code. But above all, this gentleman who was designing cockpits for other aircrafts needed a way to speed up its development process and suggested rendering decks on screen. This led, in a first iteration in what we called « virtual decks », but as the screen rendering package used lacked features for interaction and brought in significant complexity, we decided to change for lightweight rendering in a browser window. Virtual decks became web decks (but in the code or wording, both terms still co-exist.) In a nutshell, rather than sending generated images to decks for display on their small LCD screens, we send them to a web page for display on a Canvas. Couldn’t be simpler!

That, together with numerous code refactoring, abstractions, and conventions, lead to a flexible, configurable, autonomous application that now allow to use common deck models with X-Plane fight simulator. As an illustration of code refactoring, Cockpitdecks was first designed to work with X-Plane only, using X-Plane terminology of Datarefs, Commands, etc. Refactoring brought in a new abstractions for Simulator applications, with neutral generic terminology and allow now for different simulation software to be used with Cockpitdecks.

Cockpitdecks underwent numerous refactoring processes. Some basic, like name changes, some easy, like abstractions (insertion of intermediate often abstract classes), and some heavy (computer value from data with formula). But, through all these changes, the syntax of config.yaml files for buttons rarely changed. Vocabulary get richer because of addition of activations or representations, but older existing definitions kept working with minimal changes. Pages developed a year ago still work surprisingly well after all these internal changes!

Everything was created with fun and heart and is freely available to other to enjoy.

Towards the end, we added very basic straightforward user interfaces to interactively create buttons on web pages without coding. While the core functions are working as expected, a web user interface specialist could build a friendlier application from the feasibility rough cut we laid over. My knowledge of web user interface disappeared over time as things were getting highly specialized and refined. I’m sure there is room for a little svelt or vue application if someone is tempted by collaborating. (Backend already exists.) This web-based user interface would allow any one to design buttons, button pages and deck layouts to use with any simulation software. Next to that, built in extension mechanism allow developers to bring their own creations to Cockpitdecks.

# See Also

[[History]]

![[a2fs2.jpg|center]]