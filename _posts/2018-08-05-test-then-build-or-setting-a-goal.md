---
layout: post
title: "Test then build or, setting a goal"
date: 2018-08-05
---

## How do you know something works?
If a tree falls in the forest does anyone hear it?  Likewise if you create a function do you know it works?  I'm not even talking about edge/corner cases in this hypothetical instance.  I'm talking about plain, vanilla instance where the data you're taking in is pristine.  No weird characters, no bad data, evertyhing goes as expected.

To know that everything works as intended you have to test it.  You have to walk it through the ring of fire that is an integrations or behavior test.  Unit testing is great for when you're developing but when we're talking about behavior (our function working well with the turbulent world around us) then *all unit tests are lies*.

More testing == more declarative code so don't stop your tests.  Just be realistic about what each type of test will do.  The _du jour_ methodology I've seen is unit test locally (as in make sure each little piece of a what you're building works), then mock a behavioral test, then add a full integrations test for a staging/mirror environment.  Who knows maybe that'll change as new tools/frameworks come out, do whatever works at the end of the day.

The point is: *prove it works*.

## Check for the shape you want, not the right shape
In this little picture below we've got our magic function that outputs tetris style shapes.  In this magical land we want the function to output a block of two widths.  This is our _design_ stage, we have talked about what we want and written it down somewhere.

MAGIC SHAPE FUNCTION OUTPUT GOES HERE

So we go ahead an make the function righ?  Eeasy enough, stick a couple blocks together and write a test checking that we've got our 2 block sized block.  *Wrong*!  This is the danger of writing your function, then testing based on what you _think_ should happen.

If we're only checking that we've got two blocks together in a horizontal fashing we'll potentially let shapes like below through.

SHAPE PICTURE EXAMPLES GO HERE



EXAMPLE TEST WITH LIMITS GOES HERE