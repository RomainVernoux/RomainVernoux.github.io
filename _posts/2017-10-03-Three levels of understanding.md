---
layout: post
title: "The 3 levels of understanding"
date: 2017-10-03
categories: philisophy english
---
In the last couple of years, I have been a reader, a student, an apprentice, a researcher, a speaker, a trainer, and above all a passionate debater.
These activities have something in common: they are all in some way about teaching and learning.
Like most people, I did not need a lot of time in school to believe that there are good teachers and bad teachers.
But it took me years to understand that it is actually all about good and bad ways to *understand* things.
This article is a lot about learning, and a bit about teaching if you see it as the action allowing others to learn efficiently.

## Let's start with a game

Consider a simple game with a pawn moving on a board.
In this example, I am trying to teach you the rules of the game, and your goal is to *understand* on which squares the pawn can move next.

![Game 1]({{ site.baseurl }}/public/resources/3-levels-understanding/game1.png)

### Level 0

Suppose that the explanation goes along those lines (if you are a French speaker, suppose this is written in another language you are not fluent in):

> Si le pion est en D3, il peut aller en C2, B1, C4, B5, E2 et E4.
> Par réflexion, il peut aussi aller en A2, A4, D1 et D5.

> Si le pion est en D1, il peut aller en C2, B3, A4 et E2.
> Par réflexion, il peut aussi aller en D3, C4 and B5.

Chances are that you got a few words right, but you definitely did not *understand* the rules, since you cannot tell on which squares the pawn can move next.

### Level 1

Now that we are more familiar with your language skills, let me try again in good ol' English:

> If the pawn is in D3, it can move to C2, B1, C4, B5, E2 and E4.
> By reflection it can also move to A2, A4, D1 and D5.

> If the pawn is in D1, it can move to C2, B3, A4 and E2.
> By reflection it can also move to D3, C4 and B5.

If your English is decent, this should start to make more sense.
However, although you might have understood that D3, C2, or B1 are coordinates, I did not give you the coordinate reference frame (Are A, B and C columns or rows? Does it start from the top or the bottom?) and you still do not fully *understand* the rules.

### Level 2

It is now time for me to fix that:

![Game 2]({{ site.baseurl }}/public/resources/3-levels-understanding/game2.png)

> If the pawn is in D3, it can move to C2, B1, C4, B5, E2 and E4.
> By reflection it can also move to A2, A4, D1 and D5.

![Game 3]({{ site.baseurl }}/public/resources/3-levels-understanding/game3.png)

> If the pawn is in D1, it can move to C2, B3, A4 and E2.
> By reflection it can also move to D3, C4 and B5.

A picture is worth a thousand words.
Nevertheless, you have not reached full *understanding* yet.
Of course, if the pawn were in D1, you could easily tell me where it can move next.
But what if it is in E3 like in our original question? You might have an idea, but you cannot know for sure.

### Level 3

For that, we need to go another route :

> The pawn moves diagonally, with no restriction in distance.
> When it reaches the edge of the board, it can optionally use one (and only one) 90° reflection and "bounce" on edge before moving on diagonally again.

![Game 4]({{ site.baseurl }}/public/resources/3-levels-understanding/game4.png)

![Game 5]({{ site.baseurl }}/public/resources/3-levels-understanding/game5.png)

With that information, you *understood* the rules, and our original question seems now simple to answer.
When the pawn starts in E3, the squares it can reach are the following:

![Game 6]({{ site.baseurl }}/public/resources/3-levels-understanding/game6.png)

## The 3 levels

I believe that the 3 steps you climbed to reach full understanding are in no way specific to this particular example.
I call them *the 3 levels of understanding*.

The first level is *syntactic*: it requires to understand the *words*.
When you 1-understand something, you are able to *repeat* it.
When you don't, you exclaim yourself "What?".

The second level is *semantic*: it corresponds to understanding the *meaning* of the sentence.
When you 2-understand something, you can *apply* your new learning in the real world.
When you don't, you usually say "What do you mean?" or "It doesn't make sense!".

The third level is *inductive*: it means to understand the *abstract teaching* behind the phrase.
When you 3-understand something, you get a new skill that you can now *explain* yourself.
When you don't, you protest "Got it, but why?" or expect more "What is the context?".

In most situations we all expect to reach the first two levels, and we actually learned how to detect when we get there.
But the same cannot be said about the third level.
Once there, we feel like the new pieces of information fall into place, fit perfectly in our mental representation of the world, and can be explained in terms of smaller, simpler concepts.
The third level give us the confidence that we understood everything that is required to start building on top of this new knowledge.

## Up the understanding ladder

Climbing the three steps of the understanding ladder is not always an easy journey.
The following are a few examples and anecdotes about the three levels of understanding in the real world.

### As a developer

What does it mean for us developers to *understand* code? At the first level, it means being able to read the code written by someone else.
This is basically looking and saying "Yup, this is valid Java".
At the second level, it corresponds to looking up an answer on StackOverflow, understanding why it works and successfully applying it in one's particular use case.
But at the third level, it means understanding a language to the extent of being able to write meaningful code by oneself.

### A story of file permissions

I have plenty of examples and stories of people reaching the third level of understanding.
This one happened a few months ago, on a busy day in a medium-sized developer team.
I overheard two of my colleagues talking to each other.
Apparently, the first one could not run a bash script the second one had written earlier:

> — It must be a file permission issue. Can you check the output of `ls -l`?<br>
> — It says `-rw-r--r--`<br>
> — Then run `chmod +x script.sh`<br>
> — `ch`-what?<br>
> — `chmod +x script.sh`!<br>
> — Why?<br>
> — It will make your script executable.<br>
> — Oh yes, it works!

Notice the slow progression of *colleague 1* towards the understanding of the solution? He starts at level 0 (no understanding) and *colleague 2* has to repeat in order for him to reach level 1.
Then he asks for more explanation before applying the solution successfully and therefore reaching level 2.
But left as is, there would have been no level 3 in sight.
That's where I decided to jump in:

>— Do you know how to read the `-rw-r--r--` output of `ls -l`?

What followed was a short introduction to file permissions and the `chmod` command, after which *colleague 1* said:

>— Oh, I see, and if I want to only give execution permission to the owner of the file, I should run `chmod u+x script.sh`, right?

He had reached level 3.


### Somewhere, in a quantum physics lab

When I started talking about the 3 levels around me, the first victims were my roommates of the time.
One of them, a physics PhD student, told me that he recently had a similar experience at work.
He was trying to take over an experiment designed and maintained by someone who since left the team, and could not make it work as expected.
After a few trials, he contacted the creator of the experiment:

>— The figures seem incorrect, do you know why?<br>
>— [With a strong Spanish accent] You need to change the size of the potential well.<br>
>— The size of what?<br>
>— *Du puit de potentiel*, the potential well.

Overcoming the Spanish accent was reaching level 1 and making the experience work again gave him the level 2 badge.
But he quickly bumped into others issues he couldn't solve by himself and had to ask again for help.
At some point, he decided to talk to his professor:

>— Seriously, I don't get this experiment. I can't make it work!<br>
>— The problem is, you're not an operator, you're a researcher.

What he meant, with his own words, was that my friend should not satisfy himself of level 2 understandings of the situations at hand.


### Elon Musk's master plan for Tesla

Everytime I read something from Elon Musk or hear him talking, I am impressed by its communication style.
Even though he doesn't strike as very comfortable and fluent when he speaks, his statements are always meaningful, straight to the point and easy to understand.
He has something that make people want to follow him.
A very good example of it is this story from 2016.
Investors and analysts were trying to figure out where Tesla was going after the *Tesla Roadster* and *Tesla Model S*.

This is an explanation you could find (simplified for this article) in an investors report:
> We want to create and sustain a competitive advantage on the electric vehicle market while confirming our position as the transformational leader in sustainable life-style.

You could also opt for the spokesperson's interview version:
> We want to build an electric supercar, an electric sport car and an electric sedan, all powered by cheap, renewable energy that we will produce.

But compare to Elon Musk's own version of the roadmap:
> 1\. Create a low volume car, which would necessarily be expensive.<br>
> 2\. Use that money to develop a medium volume car at a lower price.<br>
> 3\. Use that money to create an affordable, high volume car.<br>
> 4\. Provide solar power.

(It turned out that Tesla later announced the affordable *Tesla Model 3* and the acquisition of the company *Solar City*)

What makes Elon Musk's version so good? It aims directly at giving you a level 3 understanding of the situation.
Look closely. The first version contains only words you know but does not seem to say anything useful : you reached level 1.
The second version states facts but does not link them together. You could tell what kind of cars Tesla is building, but you could not explain why : you reached level 2. 
With Elon Musk's version, you could probably explain Tesla's strategy to someone else : you reached level 3.

The funny story about this roadmap is that it was not published in 2016 but 10 years before in 2006 as *Tesla's Master Plan*, at a time when Tesla had not sold a single car yet!
It seems that the reasoning was so simple and the vision so clear that everything went according to the plan.
More details about this story can be found here : [Tesla's Master Plan, Part Deux](https://www.tesla.com/blog/master-plan-part-deux).
For more of Elon Musk's level 3 magic, [this talk about SpaceX going to mars](https://www.youtube.com/watch?v=SPblvFBnEDk) is a good place to start.


### Politics on income inequalities

The 3 levels of understanding are not exclusive to IT and engineering.
This example is about economics, and in particular policies that could be introduced to reduce income inequalities.

I heard this first statement in a TV interview:

> We identified several policy reforms that could yield a double dividend in terms of boosting GDP per capita and reducing income inequality [...]

Even without going into the details of the solutions, it contains technical terms that most people watching the news will not get ("double dividend", "GDP per capita", etc.), hence not reaching the first level of understanding.

Look now at the proposal of one of the candidates to the 2017 French presidential election:

> To reduce inequalities, I propose to introduce a basic income for everyone and to raise taxes to support our social model.

That is much clearer, and you could now answer questions such as "What does he propose?" or "How will we finance our social model?" but could you explain why basic income is a solution to income inequalities and why increasing taxes (which ones?) solves the issues of our social system?
If you cannot answer these, you are stuck at level 2.

Compare with the takeaway of Thomas Piketty's book *Capital in the Twenty-First Century*:

> As long as capital makes more money than work, inequalities will grow. Therefore we need to tax capital.

This book is a gold mine of simple explanations and convincing statements on economics, carefully crafted for a fast level 3 understanding.
It also contains concrete examples of how capital could be taxed and why other solutions are pointless in the long run.

It always strikes me that politics talk in ways very similar to the second statement above, thus reducing economics to opinions.
My guess is that most of them have not reached a level 3 understanding of the topic they are asked to talk about.

### Yes, even Special Relativity

A field in which the 3 levels of understanding fit particularly well is science and in particular physics, as I discovered while reading about the discovery of the theory of relativity.

As you may already know, the special relativity theory gives a relation between the notions of lengths and time for two observers moving relatively to each other.

It goes without saying that there are plenty of ways to explain it with words you never heard before or in ways that don't make sense to you at first, so I will spare you the examples of level 0 and level 1.

The Wikipedia article on Special Relativity (and in particular [this section](https://en.wikipedia.org/wiki/Special_relativity#Reference_frames.2C_coordinates.2C_and_the_Lorentz_transformation) is a good example of a level 2 explanation.
It contains a few formulas such as this one:

![Special Relativity formula]({{ site.baseurl }}/public/resources/3-levels-understanding/special-relativity.png)

Formulas are the scientific archetype of level 2 explanations: they are easy to apply (if I gave you values for *t*, *x*, *v* and *c*, could you give me *t'*?), but they are definitely not tantamount to mastering a topic.
Einstein actually wrote [a book](https://www.amazon.com/Relativity-Special-General-Albert-Einstein/dp/1420946331/) with his own (level 3) explanation of relativity, which is definitely worth a read if you are into science and have a high-school background in physics.

But here comes the funny part of the story: Einstein did not discover the above formulas, [Lorentz did](https://en.wikipedia.org/wiki/Lorentz_transformation).
But Einstein was the first to *explain* why they make sense in the real world, which is the definition of the level 3 understanding.
And guess who history says discovered Special Relativity ?


## Missing a step

We saw before that there was a natural path from level 0 to level 3, assuming you are given a chance to reach that point.
But what happens when we miss one of this step?

### Missing level 1

Missing the first step means not knowing (or disagreeing on) the definition of some words.
In the learning process, it leads to large misinterpretations.
In debates, it is the main cause of pointless discussions.
While teaching to someone else, it often makes you lose your audience (this is particularly easy to observe in technical presentations).
In general, simple and clear definitions should always precede important statements.

### Missing level 2

Missing the second step corresponds to not having examples nor particular cases to get started.
Depending on your background, this can make things seem too theoritical, but the absence of level 2 elements in an explanation is not critical.
Real issues arise when level 2 explanations are given without any level 3 insights.

### Missing level 3

All the examples above illustrate what happens when one does not reach the third level of understanding: with no context and no idea why things work the way they do, it is impossible to act outside the precise boundaries of the original explanation.
Nothing useful was really learnt.

Something else I discovered is that reaching a level 3 understanding on a topic makes it really easier to remember.
This is why at this point you still remember Elon Musk's master plan but completely forgot the Special Relativity formula.


## 3 levels of teaching

Because teaching is the action leading someone to *understand* something, it can greatly benefit from the lessons learned from the 3 levels of understanding.
As a teacher (or a speaker in general) always make sure that your audience understands the words you use, or even better, start by defining them (level 1).
Then, of course, present the solution to the matter at hand and the way to use it (level 2).
But don't forget to give enough insights for people to understand why it works (level 3)! On this perilous journey, use the tests associated to each level : *can you repeat? apply? explain?*

There is a famous anecdote about *Richard Feynman*, recipient of the Nobel Prize in Physics, once asked a very precise question on quantum mechanics: "Can you explain to me why spin one-half particles obey Fermi-Dirac statistics?".
Supposedly, his first answer was "I'll prepare a freshman lecture on it", but he later came back defeated:

> I couldn't do it. I couldn't reduce it to the freshman level. That means we don't really understand it.

If you can't *explain* something, you don't 3-understand it, and the best you can hope for your students is a disappointing 2-understanding.


## Do you understand?

Of course, it is okay to not 3-understand everything (as long as you are honest about it).
But as much as possible, remember these 3 (!) guidelines:

- don't settle for less than the third level
- always give explanations going through levels 1, 2, 3 in this order
- don't ask "do you understand?". Ask to repeat, apply or explain.
