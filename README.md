# 2-stage-arcade-stick
a method of having 2 stages of movement speed for smash bros in a traditional fight stick controller format

It all began with a silly idea.

I had previously purchased a cheap fight stick off amazon called the PXN-0082. I had bought it to play some old school 2d fighting games and some classic arcade games that had been released for the nintendo switch but after a few times using it the novelty had worn off and I put it up on a shelf.

One day while dusting off that shelf I picked it up and noticed that it had 2 settings for directions. D-pad and analog stick.

Some background on me. I am a 41 year old father of a 9 year old daughter. When she was 4 years old she began playing Kirby games on my DS lite and her older half brothers 3DS because Kirby is adorable. When she was 5 years old 2020 happened and we quickly needed to find ways to entertain her during the lockdowns. She had seen on youtube that Kirby was in smash bros so I bought her a Switch Lite and a copy of Smash Bros Ultimate. Two years later when she was 7 she was watching footage of a smash bros tournament and realized she wanted to try competing in smash bros tournaments so we found a local weekly here in town and we have been going ever since (lets go bubblegum!). I however have never been a big player of smash bros. I have played it a few times over the years at friends houses/parties but it never clicked with me until I started playing it with my daughter. It is a very fun game and since she wanted me to compete alongside her I started taking it more seriously I can encourage her to also keep trying to get better at the game. That being said I am NOT GOOD at this game so my top priority when it comes to smash is having fun and encouraging my child to pursue her goals. 

Back to the controller, I started playing smash ultimate with this pxn controller and it was very clear from the start that it was NOT a good controller. It mimics analog signals but since it is digital switches there is no walking or variable movement speeds. It is full run no matter what. That being said it was a LOT of fun to play on. It felt great to use an arcade stick that I grew up using in arcades and skating rinks to play this game that I had grown to love so dearly but it's limitations began to make me start asking questions.

How does this stick mimic analog values?
how could I use this same form factor but have full analog controls?
If full analog controls in this form factor are not possible how could I have walk and run options with this same form factor?

I have seen all button controllers in the smash community for a couple years now and have tried to learn how to use them but my old brain never could get it. I do think that it is an amazing way to play smash once you get the muscle memory down but personally they never interested me. That being said along the way I had joined a few discord server's for custom controllers and I began asking questions in them. I must give huge shoutouts to members of the Crane's Lab and Rana Labs discord servers for being absolute legends of humanity.

After much googling I discovered that most likely the circuitry in this stick uses resistors to send artificial analog values when the arcade stick microswitches are pressed. This got me thinking:

how could I have walk and run using a traditional arcade stick?

I would need a switch for each stage and then use either a microcontroller such as the Raspberry Pi Pico or do a padhack of a Gamecube controller PCB and using different resistor values for each stage.

For sinplicity I used the second method. I had already been testing using an analog arcade stick (the sj@jx analog arcade stick from aliexpress) but was not pleased with the feel and responsiveness of the stick. It seems that when you size up an analog stick to be arcade stick sized it does not feel good to use. They have horrific snapback due to the mass of the stick, they have issues returning to center reliably and overall they feel very loose and mushy. I kept going back to the fight stick because it just felt "right". The decision was clear, I had to find a way to make walk and run happen on a microswitch based arcade stick.

I purchased a knock off version of a Sanwa JLF arcade stick off amazon. (EG starts arcade stick https://www.amazon.com/dp/B01N0DO631?psc=1&ref=ppx_yo2ov_dt_b_product_details)

This stick is identical to a sanwa JLF which is an industry standard stick and felt like a good candidate to go with. I wanted to know how the microswitches worked so I popped off the lid and discovered that there is a large amount of travel that occurs after the switch is activated. This gave me the idea that 2 switches could be stacked together and with the right spacer allow you to gently press the inner switch for run and then push furether to activate the outer switch for run.

![image](https://github.com/heyitsryan/2-stage-arcade-stick/assets/2439341/e1c3a673-ad96-481f-a882-79c3c246901c)

I discovered that if you cut a hole in the back of a microswitch you can put the plunger of the second switch inside that hole and use a spacer to actuate both switches. This was my initial proof of concept video I posted to the Crane's lab discord.

https://photos.app.goo.gl/pXye6X9tDYSrckT88

So now with this idea in hand I had to figure out the right resistor values and fabricate a rough prototype.

A Gamecube controller's analog sticks are 10k ohm potentiometers. This means that the baseline level of resistance that they are expecting between the 2 end pins and the middle signal pin is 10K ohm resistance.

Middle value for walk is 5k ohm and full run is 3k ohm or lower (I found 2.6415k ohm is a good value for full run)

so to achieve these values PER DIRECTION we need a 10k ohm resistor to set the analog values to dead center, another 10k ohm resistor that is triggered by the first microswitch and a 5.6k ohm resistor triggered by the second microswitch.

I drew up a schematic for how to make this.

![x axis final](https://github.com/heyitsryan/2-stage-arcade-stick/assets/2439341/9b6f1280-1d2c-4e23-a30f-22bde1cbe6ad)

I pad hacked a gamecube controller pcb I bought off ebay (https://www.ebay.com/itm/325720453073) and then assembled my prototype stick.I decided to do the X axis directions first as I only had a couple extra microswitches available.

![IMG_20230909_203902537](https://github.com/heyitsryan/2-stage-arcade-stick/assets/2439341/c4524d67-09e8-4a17-9df3-f2dfb519c41d)
![IMG_20230910_113507325](https://github.com/heyitsryan/2-stage-arcade-stick/assets/2439341/323d5d5b-465c-442f-bf2e-cbc24ae565ed)

I used 5 minute epoxy to "glue" the outside switch to the inside switch. I also realized that since the inner switches were soldered to the pcb that they came on I would need to fille the pcb traces for ground because they do not all have common ground in this scenario. Each direction has seperate ground pins as we are replicating an analog signal.

I then began breadboard testing. I put the stick into my test enclosure and began trying the stick on my nintendo switch. 

(smash ultimate movement test video) https://photos.app.goo.gl/1Ei1XSQQ6XDE9ebN6

I initially thought that I had done something wrong. It seemed that walk was working perfectly but when the second switch was activated i just walked faster instead of going to full run.

(switch menu analog stick test menu video) https://photos.app.goo.gl/YKhRPbahzpo25nkXA

So I stepped back and grabbed a regular pro controller and did some movement testing with a stock nintendo controller and I discovered that the analog stick behaved the same way! If you tilt the stick slightly you get walk and if you tilt all the way you get faster walk but you cannot run until you let the stick return to center and then push the stick fully all the way to one direction. I HAD SUCCESSFULLY REPLICATED ANALOG MOVEMENT WITH DIGITAL SWITCHES!

I did encounter issues however. Since I had only made a second stage for the X axis directions I simply copied the resistance values for full run I was using for X axis to a perfboard pcb for Y axis. My theory was that this would give me perfect 45 degree diagonals since the resistance values are identical. This however did not turn out to be the case. It seems that I will need to add a second stage to the Y axis and replicate the schematic for that as well. This should potentially give me several diagonal values depending on which combinations of switches are pressed. 

That is currently where this project resides. I am waiting on more microswitches to be delievered to modify the Y axis in the same way as the X axis and will be posting all of my testing results here.

Thank you for your time if you read this and if this inspired you to try this for yourself please let me know! I know of at least one other person that is making their own spin on the concept using different parts and I am so excited to see what they come up with.
