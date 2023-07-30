## **The Binding Problem**

People, when they look at the world around them, see in terms of objects- a tree, a bench, a table, a cloud.  This is a great way to look at the world and most people take their ability to do it for granted.  Computers can't do this at a human level, yet, but there are several approaches that people have tried.  One way is to train the computer on a certain class of object, say a fire hydrant.  And if you do this enough times for enough objects, you can eventually get the computer to see a lot of objects.  Some computers can also use segmentation algorithms that break the entire visual field into objects and are trained on images with the objects labeled, like Segment Anything. Some of these algorithms even learn to see the world of objects using unlabeled video image training data (the same way a human would.)  The most promising of these are based on VAEs, like Genesis, SPACE, etc. but they all have shortcomings which I will talk about in a later post.  (For more information on these algorithms, the binding problem, and more, read this paper by Greff et al: [On the Binding Problem in Artificial Neural Networks](https://arxiv.org/abs/2012.05208)) 

How does a human go from not understanding the visual feed to seeing distinct objects that are independent from each other?  I previously mentioned that this is so natural for humans that many take it for granted.  In fact, humans learn to break the visual world into objects before they learn to talk or form memories.  Learning to break the world into objects is core to the human experience, and also difficult to study because it’s hard to ask a baby about its learning experience before it can talk.  It is such a universal experience that we can question whether the ability to break the visual world into objects is a learned ability, or whether it’s something intrinsic, something we’re just born with.  I believe that it is a learned ability, but that the learning process is intrinsic.

I believe it is a learned process because of three evidences:
Infants slowly develop their perceptive abilities over the first two years of their life.
For example, when born, humans have no object perceptive ability, but after two months have learned to track moving objects with their eyes.  
Human infants gradually learn to see past larger areas of occlusion.
Colorblind people are better at perceiving certain types of camouflage than color sighted people, suggesting a learned process that is affected by color perception.  Note that camouflage is a rare human fail case for object perception: an object is there, but it is not perceived.
We have evidence from people who have been blind for most of their lives, but, due to medical advances, have had an operation later in life that enables them to see.  These people can see, but they cannot perceive.  The best detailed case I have found of this is of Virgil, from the Oliver Sacks book An Anthropologist on Mars.  Virgil progresses from seeing a whirl of color and light to faces, letters, shapes, edges, movement and whole objects.

The learning process itself is intrinsic because it happens to most humans.  It is like language development, or learning to walk.

Even though this is a learned process, there are still priors, at least for humans.  For example, humans have a predisposition for detecting the human face; after getting his sight back, Virgil almost immediately discovered the face of his doctor.  But I believe that general object perception is possible without priors.

Having evidence that this is a learned process in humans gives us confidence that we can have the computer learn the same process.  However, even though we know the object binding process is learned, we don’t know how it is done (either the binding process itself, or the learning of the process).  One natural follow-up is to turn inwardly, look at an object, and say to ourselves, “How do I know that what I’m looking at is indeed an object?  What’s an object, anyway?”

What is an object?
In real life, an object is a discrete chunk of stuff that acts on its own.  For example, a ball.  I could conceivably throw the ball, and it would move on its own through real space, possibly interacting with other objects.  
However, not all objects necessarily move. Something like a fire hydrant you would never see move, but still counts as an object.  However, due to the magic of parallax, we can still visually distinguish movement of the fire hydrant, not because of the hydrant moving, but because of the way the observing eye moves.
In the visual world, background elements like clouds, mountains, sky can also be objects.  This is a convenient definition because it lets us assign any pixel in our visual world to an object, even if it’s part of the background.
If we are using movement as our basis for distinguishing objects, we may not be able to determine the objectness of background elements that don’t move.  However, I still believe that motion is important for distinguishing objects, as it can be a “giveaway” when one object moves independently of the rest of the visual world.
As a side note, some datasets such as coloured Multi-dSprites (Burgess et al., 2019) and CLEVR only use static images, whereas others such as the GQN dataset (Eslami et al., 2018), and ShapeStacks (Groth et al., 2018) use video. Our algorithm should be good at both.

Why is it an advantage to see the visual world in terms of objects?  It is an advantage to lump stuff together based on like properties.  It’s easier on the brain to think of a thousand objects, each with their own consistent properties, than one large visual field-sized object or one hundred million pixel-sized objects.

What are some properties that apply to all objects?  If we didn’t know that objects exist in the first place, how could we find them with no prior knowledge?  

It is surprisingly difficult to find properties that apply to all objects, so let’s relax our standards and try to find properties that apply to *almost all* objects.  We should be able to live our lives for several months without coming across an object that violates these properties.  I should also warn you that this is not a topic that I have found studied anywhere, so a lot of this is just guesswork by me.

Consider two objects, a rose on a rosebush and a distant mountain.

<p float="left">
  <img src="https://jamesrichter.github.io/docs/assets/images/image2.jpg" width="300" height="250" />
  <img src="https://jamesrichter.github.io/docs/assets/images/image1.png" width="300" height="250" /> 
</p>


When you’re looking at an object, you generally recognize it because it is a different color, shape, or texture than its surroundings.  However, if you're learning to separate the visual world into objects, these are less helpful.  Color and texture distinguish a rose from the leaves of the rosebush, but they don't distinguish the distant mountain from the horizon.  Shape distinguishes the mountain; in other words, the mountain is shaped differently than the horizon.  However, if we're trying to discover objects from an image of pixels, we don't yet know which pixels comprise which objects.  So we need a definition of shape that doesn’t preclude that we know which pixels comprise the mountain.

One property we can get is this: 
1. Objects have their own color, and while those colors can change over time, they generally don’t change very quickly.

Objects that change their color include leaves on a tree, and indeed, a rosebush.  However, these colors generally don’t change while we’re looking at them; the exception being when an object is undergoing some kind of chemical reaction, or is a chameleon.  I believe that texture and shape follow a similar vein as color:

2. Objects have their own shape and texture, and while those can change over time, they generally don’t change very quickly.

Once you comprehend the color, shape, and texture of an object, you’ve got the object down.  This doesn’t really help us learn to see objects for the first time, though many traditional segmentation algorithms (ex. Felzenszwalb-Huttenlocher segmentation) end up separating color and shape.

I've already discussed the following property, but I think it’s worth repeating:

3. Objects are useful for comprehending the world around us.  The entire visual world can be comprised of objects.

This is more about what objects are and what they’re used for, but I think it’s worth mentioning since it is a property of all objects: every object we see helps us comprehend the visual world, and if you take one away, then we understand a little less.  This may sound a little tautological.

Here’s another property that has to do with movement:

4. Objects move slowly (an object in one frame contains most of the object in the next frame)

How many FPS a human can see is a subject of debate, especially among gamers, but let’s say the human sees at 144 Hz.  Most objects don’t move this quickly.  Exceptions are things like lightning, and possibly a fastball? (On the other hand, there are people who get paid to track and hit fastballs with a wooden bat, so maybe it’s not outside the realm of human visualization after all)  Generally, the eye moves much faster than most objects, which is good for our object perception because the eye’s movement can be predicted perfectly.

5. Objects move together, and they move independently of the outside environment.

This is the one I’m least sure of.  Well, I am sure that the pixels in an object move together, and that how an object moves is a distinctive characteristic that humans take advantage of, since the structure in the brain for distinguishing the movement of an object is the same as for distinguishing personality. But I’m not sure that objects move independently of the visual environment.  For the rose, and the fire hydrant, a human moving around the object can determine the object through parallax.  For a 2D video game like Super Mario Bros, parallax is useless, but most of the objects in the game still move on their own. For both the parallax case and the video game, the human can stimulate independent motion of the object.  But what about the mountain in the distance?

![Mountain](https://jamesrichter.github.io/docs/assets/images/image1.png)

If the human moves their eye a very small amount parallel to the horizon, and we take the difference between two frames, we get this:

![Mountain](https://jamesrichter.github.io/docs/assets/images/image5.png)

This is the absolute difference between two frames in RGB color space.  Because most of the pixels are the same, the difference is (0,0,0), or black.  But the area around the mountain is the light blue color minus the dark blue color, which ends up being (41, 71, 44).  This frames the mountain perfectly.
For a toy example like this, frame subtraction works great, but what about a more realistic mountain background?


![Mountain](https://jamesrichter.github.io/docs/assets/images/image6.png)

It could still be possible with more sophisticated eye movement, which is why I’m not sure.

The last two properties are the crux of our algorithm, and the first one I’m almost positive applies to all objects. (The second one I can think of some counterexamples, but they are all abstract objects like “words” or "collections")

6. It is easier to predict a masked part of the object given the rest of the object, than given the rest of the outside world.

For example, predicting a small region (shown in red and blue) inside a single rose flower given the rest of the rose is easy, but predicting the same region given the rest of the rosebush is hard.  However, it is possible to make a good guess from the “outside world” due to the fact that context clues can help. In this case, the outside world gives us the stem leading up to the rose, and the color of other roses on the bush, and whether those roses are in bloom.  From here, we could guess that the masked region would be “pink,” but we don’t really know exactly which shade of pink.



<p float="left">
  <img src="https://jamesrichter.github.io/docs/assets/images/image3.jpg" width="300" />
  <img src="https://jamesrichter.github.io/docs/assets/images/image4.jpg" width="300" /> 
</p>

In this set of two images, the region covered by the blue square is much easier to predict in the first image than in the second.



The last property:

7. All of the pixels in an object are completely connected unless occluded by another object.

By “completely connected” I mean that we can trace a path from any object pixel to any other object pixel without going through non-object pixels.  The only exception to this that I’m aware of is abstract objects like words, which have space between the letters but due to Gestalt psychology are comprehended as one object.  The other, more obvious exception, is when one object is occluded visually by another.  This is such an important case that I have incorporated it into the rule itself.




