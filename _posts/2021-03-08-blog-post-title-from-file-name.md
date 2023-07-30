The Binding Problem

![Book logo](/../docs/assets/images/image1.png)

People, when they look at the world around them, see in terms of objects- a tree, a bench, a table, a cloud.  This is a great way to look at the world and most people take their ability to do it for granted.  But computers can’t do this yet.  Computers still see the world in terms of pixels.  Sometimes, if you train a computer hard enough, you can get the computer to see a certain class of object, say a fire hydrant.  And if you do this enough times for enough objects, you can eventually get the computer to see more objects.  Some computers can also use segmentation algorithms that break the entire visual field into objects, but none** of these algorithms learn to see the world of objects using unsupervised video image training data (the same way a human would.)

** There are actually a handful of algorithms that do this, the most promising of which are based on VAEs, like Genesis, SPACE, etc. but they all have shortcomings which I will talk about in a later post

How do we go from not understanding the visual feed to seeing distinct objects that are independent from each other?  I previously mentioned that this is so natural for humans that many take it for granted.  In fact, humans learn to break the visual world into objects before they learn to talk or form memories.  Learning to break the world into objects is core to the human experience, and also difficult to study because it’s hard to ask a baby about its learning experience before it can talk.  It is such a universal experience that we can question whether the ability to break the visual world into objects is a learned ability, or whether it’s something intrinsic, something we’re just born with.  I believe that it is a learned ability, but that the learning process is intrinsic.

I believe it is a learned process because of three evidences:
Infants slowly develop their perceptive abilities over the first two years of their life.
For example, when born, humans have no object perceptive ability, but after two months have learned to track moving objects with their eyes.  
Human infants gradually learn to see past larger areas of occlusion.
Colorblind people are better at perceiving certain types of camouflage than color sighted people, suggesting a learned process that is affected by color perception.  Note that camouflage is a rare human fail case for object perception: an object is there, but it is not perceived.
We have evidence from people who have been blind for most of their lives, but, due to medical advances, have had an operation later in life that enables them to see.  These people can see, but they cannot perceive, and they can also catalog their experiences in learning to perceive for the benefit of the student of perception.  The best detailed case I have found of this is of Virgil, from the Oliver Sacks book An Anthropologist on Mars.  Virgil progresses from seeing a whirl of color and light to faces, letters, shapes, edges, movement and whole objects.

The learning process itself is intrinsic because it happens to most humans.  It is like language development, or learning to walk.

Even though this is a learned process, there are still priors, at least for humans.  For example, humans have a predisposition for detecting the human face; Virgil could almost immediately discover the face of his doctor.  But I believe that general object perception is possible in a computer without priors.


Having evidence that this is a learned process in humans gives us confidence that we can have the computer learn the same process.  However, even though we know the object binding process is learned, we don’t know how it is done (either the binding process itself, or the learning of the process).  One natural follow-up is to turn inwardly, look at an object really hard, and say to ourselves, “How do I know that what I’m looking at is indeed an object?  What’s an object, anyway?  And what is the advantage of seeing the world in terms of objects?”

What is an object?
A discrete chunk of stuff that acts on its own.  For example, a ball.  I could conceivably throw the ball, and it would move on its own.  
Not all objects necessarily move. Something like a fire hydrant you would never see move, but still counts as an object.  However, due to the magic of parallax, we can still visually distinguish movement of the fire hydrant, not because of the hydrant moving, but because of the way the observing eye moves.  Visually, they are the same.
In the visual world, background elements like clouds, mountains, sky can also be objects.  This is a convenient definition because it lets us assign any pixel in our visual world to an object, even if it’s part of the background.
If we are using movement as our basis for distinguishing objects, we can’t determine the objectness of background elements that don’t move.  However, I still believe that motion is important for distinguishing objects, as it can be a “dead giveaway” when one object moves independently of the rest of the visual world.
As a side note, some datasets such as coloured Multi-dSprites (Burgess et al., 2019) and CLEVR only use static images, whereas others such as the GQN dataset (Eslami et al., 2018), and ShapeStacks (Groth et al., 2018) use video. Our algorithm should be good at both.

Why is it an advantage to see the world in terms of objects?  It is an advantage to lump stuff together based on like properties.  It’s easier on the brain to think of a thousand objects, each with their own consistent properties, than one large visual field-sized object or one hundred million pixel-sized objects.

What are some properties that apply to all objects?  If we didn’t know that objects exist in the first place, how could we find them with no prior knowledge?  

It is surprisingly difficult to find properties that apply to all objects, so let’s relax our standards and try to find properties that apply to *almost all* objects.  We should be able to live our lives for several months without coming across an object that violates these properties.  I should also warn you that this is not a topic that I have found studied anywhere, so a lot of this is just guesswork by me + thinking about objects a lot.  But I still believe it is useful.

Let’s consider two objects, a rose on a rosebush and a distant mountain.



When you’re looking at an object, you generally recognize it because it is a different color, shape, or texture than its surroundings.  But this doesn’t apply to *all* objects.  Color and texture distinguish a rose, but not a distant mountain.  Shape distinguishes the mountain, but we need a definition of shape that doesn’t preclude that we know which pixels comprise the mountain.

However, one property we can get is this: 
Objects have their own color, and while those colors can change over time, they generally don’t change very quickly.

Objects that change their color include leaves on a tree, and indeed, a rosebush.  However, these colors generally don’t change while we’re looking at them; the exception being when an object is undergoing some kind of chemical reaction, or is a chameleon.  I believe that texture and shape follow a similar vein as color:

Objects have their own shape and texture, and while those can change over time, they generally don’t change very quickly.

Once you comprehend the color, shape, and texture of an object, you’ve got the object down.  This doesn’t really help us learn to see objects for the first time, though: or does it??  Many traditional segmentation algorithms rely on separating color and shape (not so much texture).

We’ve already been over this property, but I think it’s worth repeating:

Objects are useful for comprehending the world around us.  The entire visual world can be comprised of objects.

This is more about what objects are and what they’re used for, but I think it’s worth mentioning since it is a property of all objects: every object we see helps us comprehend the visual world, and if you take one away, then we understand a little less.  This may sound a little tautological.

Here’s another property that has to do with movement:

Objects move slowly (an object in one frame contains most of the object in the next frame)

How many FPS a human can see is a subject of debate, especially among gamers, but let’s say the human sees at 144 Hz.  Most objects don’t move this quickly.  Exceptions are things like lightning, and possibly a fastball? (On the other hand, there are people who get paid to track and hit fastballs with a wooden bat, so maybe it’s not outside the realm of human visualization after all)  Generally, the eye moves much faster than most objects, which is good for our object perception because the eye’s movement can be predicted perfectly.

Objects move together, and independently of the outside environment.

This is the one I’m least sure of.  Well, I am sure that the pixels in an object move together, and that how an object moves is a distinctive characteristic that humans take advantage of, since the structure in the brain for distinguishing the movement of an object is the same as for distinguishing personality. But I’m not sure that objects move independently of the visual environment.  For the rose, and the fire hydrant, a human moving around the object can determine the object through parallax.  For a 2D video game like Super Mario Bros, parallax is useless, but most of the objects in the game still move on their own. For both the parallax case and the video game, the human can stimulate independent motion of the object.  But what about the mountain in the distance?



If the human moves their eye a very small amount parallel to the horizon, and we take the difference between two frames, we get this:

This is the absolute difference between two frames in RGB color space.  Because most of the pixels are the same, the difference is (0,0,0), or black.  But the area around the mountain is the light blue color minus the dark blue color, which ends up being (41, 71, 44).  This frames the mountain perfectly.
For a toy example like this, frame subtraction works great, but what about a more realistic mountain background?

It could still be possible with more sophisticated eye movement, which is why I’m not sure.

The last two properties are the crux of our algorithm, and the first one I’m almost positive applies to all objects. (The second one I can think of some counterexamples, but they are all abstract objects like “words”)
It is easier to predict a masked part of the object given the rest of the object than given the rest of the outside world.

For example, predicting a small region (shown in red and blue) inside a single rose flower given the rest of the rose is easy, but predicting the same region given the rest of the rosebush is hard.  However, it is possible to make a good guess from the “outside world” due to the fact that context clues can help. In this case, the outside world gives us the stem leading up to the rose, and the color of other roses on the bush, and whether those roses are in bloom.  From here, we could guess that the masked region would be “pink,” but we don’t really know exactly which shade of pink.



In this set of two images, the region covered by the blue square is much easier to imagine in the first image than the second.



The last property:

All of the pixels in an object are completely connected unless occluded by another object.

By “completely connected” I mean that we can trace a path from any object pixel to any other object pixel without going through non-object pixels.  The only exception to this that I’m aware of is abstract objects like words, which have space between the letters but due to Gestalt psychology are comprehended as one object.




## Blog Post Title From First Header

Due to a plugin called `jekyll-titles-from-headings` which is supported by GitHub Pages by default. The above header (in the markdown file) will be automatically used as the pages title.

If the file does not start with a header, then the post title will be derived from the filename.

This is a sample blog post. You can talk about all sorts of fun things here.

---

### This is a header

#### Some T-SQL Code

```tsql
SELECT This, [Is], A, Code, Block -- Using SSMS style syntax highlighting
    , REVERSE('abc')
FROM dbo.SomeTable s
    CROSS JOIN dbo.OtherTable o;
```

#### Some PowerShell Code

```powershell
Write-Host "This is a powershell Code block";

# There are many other languages you can use, but the style has to be loaded first

ForEach ($thing in $things) {
    Write-Output "It highlights it using the GitHub style"
}
```
