Cockpitdecks uses X-Plane UDP connection to perform two tasks:

1. Issue commands ()
2. Collect or set dataref values ()

Here are some remarks about the use of X-Plane UDP for these purposes.

# Volume

Cockpitdecks has a two-way communication channel with X-Plane. When a user presses a button on a deck, a command is issued to X-Plane and/or a dataref value is changed. In the other direction, Cockpitdecks received values of datarefs it monitors, and if a value has changed, it notifies the deck to change its appearance accordingly.

Decks have sometimes 30 buttons, and simulator pilots sometimes have several decks. During developments, Cockpitdecks controlled up to 6 decks. This led to the monitoring of an average of 80 different datarefs to change the appearance of 80 buttons or LED encoders.

We only request dataref values to be sent at the lowest emission rate, that is once per second.

A UDP packet of values contains at most 14 or 15 values. It takes a few packets to get all dataref values.

We experimentally observed that there is a performance limit of about ~100 datarefs that can be requested from X-Plane. After that, UDP packets will be emitted, but with some noticeable delay (up to a few seconds). And since they will be emitted late, their value will no longer reflect the current X-Plane value.


To request more datarefs, you can request them by batch of less than 100 datarefs and switch batches of requested datarefs at regular interval. It works very well.

Cockpitdecks created a dataref batch collector.

# Collecting Arrays

Horizontal and Vertical Arrays 

# UDP Interaction Improvments

## Collection of arrays

Add ability to collect an array of values, especially arrays of characters (I.e. strings).

This is not easy give UDP restrictions (packet size), and X-Plane UDP choices (it only returns float).

May be an exception should be make for strings with restrictions, like ASCII characters only, limited to the maximum size of a UDP packet.

Getting a 40 or 80 character string in one operation would be a major improvement.

Getting arrays of maximum ~14-15 floats in one operation too.

## Begin/End Command

Commands that are initiated with beginCommand and terminated with endCommand should get some support too. Like, for example, begining the command with a timeout, if the end of the command is not received within the timeout, the command is automagically ended. (I did a plugin that does that, or something similar.) 
