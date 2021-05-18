---
title: "What's up with the cake?"
date: 2021-05-18T20:39:29+02:00
tags: [programming, naming, cake]
draft: false
---

Hang on, the cake is not a lie!

But first...

# Let's talk: **Naming Things**.

One of the many things a programmer should learn, is to properly name variables, functions, classes, etc.
With proper naming, we can eliminate a lot of problems that will haunt us when we get older. _(Yes, even a minute ago you were younger...)_

When learning to program, we are initially introduced to variables using simple mathematical problems, for example: `c = a + b` or `(x * y) / z`

This is not bad, personally however, neither during my high school times nor during my university studies where we made aware of how to find good names.
Resulting in many problems and confusion for the next few years.

Even now, making a quick search on google using `<programming_language> tutorial` as the keywords, I can find many tutorials that introduce you to programming using various simplifications, but never touch the topic of naming, which is... unfortunate.

## A simple example

Let us look at some older code I have written. I started to focus on naming things back then, but I was still inconsistent and barely understanding the topic. I will try to guess each variable's purpose just from its name, type and the struct it is defined in.

    struct IndiceData
    {
        // Probably holds the allocated number of bytes.
        uint32_t allocated;

        // WTF, maybe the number of all indices?
        uint32_t n;

        // Holds an allocated chunk of data with the total byte size equal to `allocated`
        void* buffer;

        // Probably the same pointer as above, just with a type.
        uint16_t* indices;
    };

After looking into the use case, I was partially wrong. The `allocated` variable holds the total number of **elements**, not bytes, possible to store in the given `buffer` memory block.
However, `n` is more complicated, generally it always ends with the same value as in `allocated`, it is however incremented once for each loaded indice.
If the `n` variable would differ from `allocated` after the load operation, it would assert and crash the game engine.

Unlucky how it is, the naming is beyond bad in this example. There should be no need to perform mental gymnastics or even check the implementation to just find out the purpose of these variables.
Let us improve this example **without** changing the implementation.

> _For now ignore the fact, that this structure in itself is badly designed...._

    struct IndiceData
    {
        uint32_t indice_count; // previously `allocated`
        uint32_t loaded_indice_count; // previously `n`
        uint16_t* indice_array; // previously `indices`

        void* buffer;
    };

We can understand this structure bit easier now. It can hold `indice_count` of indices in `indice_array`. It also holds information about `loaded_indice_count` and the `buffer` where everything is stored.
Because we did not change the implementation, we can now easily see that `loaded_indice_count` seems to be out of place here, and probably shouldn not be part of this struct. Especially that we already have a variable that holds the number of indices called `indice_count`.

Looking at the implementation, we also end with a much clearer assertions as an example:

    assert(indice_count == loaded_indice_count); // new

    // vs

    assert(allocated == n); // old

Now we get to the **how-to**.


## How to find good names?

**Practice.**

Nothing will help you more than plain practice naming things. This takes time, and can be extremely hard, sometimes resulting in hours spent just to find **ONE** good name.

Nonetheless that would be kind of idiotic to leave it here. There are a few things that might help you along:
1. Avoid quick _single_ character names. It is tempting to use `i` in a `for` loop, but it's deceptive in the long run.
    * Bad example: `for (int i = 0; i < doors.size(); ++i)`
    * Good example: `for (int door_idx = 0; door_idx < active_doors.size(); ++door_idx)`
2. Avoid generic names like `Manager`, `System`, `Container`. Almost everything can be named that way, and it does not provide any meaning in most cases.
    * But also, don not hesitate to use them when it is reasonable.
    * For example, an `EntityManager` is acceptable, however `DoorSystem` is simply wrong. Is it a system of a single door or multiple doors working together?
3. Find a single responsibility for your variable, type, function, etc.
    * Bad example: `DoorManager`, what does it do?
    * Good example: `DoorPuzzle`, we have a puzzle of doors that need to be used in some way.
4. However do not go crazy with the name length. Try to keep the name context aware.
    * If we are in a function called, `enumerate_character_traits` we can use a local variable name like `count`. We do not want to write every time `character_trait_number`.
    * In a broader context however `count` can be too ambiguous and `character_trait_number` will provide enough information to the developer.
5. [Thesaurus](https://www.thesaurus.com/) is your friend.

Obviously, the list could be way longer and much more detailed, however that is the issue with this topic. It is too broad to be properly taught, which is probably the reason it is ignored in so many places.

## Where do we go from now?

For you, probably into one of your projects, looking through all the lines of code, trying to find cases where you could name things better.

Of course, in the beginning may feel awkward when your pretty 16-character long function turns into a 69-character long monster, but you will appreciate the verbosity of it later.

With each day not looking at a functions implementation, you will forget how it works and proper naming will always help you understand it quicker.

**Also, you do not want to be a monster to other programmers, do you?!**

# "What's up with the cake?"

Why did I name this blog "**Digital Cake**"?

I do not really know, probably just for fun :P

Maybe a little spoiler what topics I will cover, but who knows in the end? Even I have no idea what I will write next time.

And with that I will leave a final advice:

> **Don't forget to have fun while naming things!**

Cheers!

_<Imagine a cake image here, as finding a totally free one on the internet seems to be impossible>_
