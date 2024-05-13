Also worth reading @s-zeng's repos:
https://github.com/s-zeng/haskell-monads-showcase

@s-zeng comments on the [original project](https://github.com/stepchowfun/effects.git):

This is a fairly good overview of the effect composition options you have in haskell. 

Notably it's missing a [readerT approach](https://www.fpcomplete.com/blog/readert-design-pattern/), 
but readerT makes a lot less sense for this problem. It would also be nice to see a few more effect library comparisons, as there's quite a zoo of those. He got the two main camps though (free and freer monads), so that's nice.

The Eff (extensible effects) library he uses used the same paper that I read for my program, but packages it all a bit more fancily in a library with lots of composition utilities (e.g. `Control.Eff.Extend.send` and such). In spirit, my implementation feels closer to their free monad implementation (but that too uses a library `Control.Monad.Free`).

The main difference between what's in `DiskFS.hs` and these solutions is that they separate the free(r) monad from the instructions/dsl datatype. Hence, using operators like `send` or `liftF` to put the legal operations into a free(r) monad.
Whereas in `DiskFS.hs`, you'll notice that I have the freer monad style `Pure :: a -> Simulator a` and `Bind :: Simulator a -> (a -> Simulator b) -> Simulator b` "instructions" as part of the same datatype, and rely on those to implement the monad interface. So in some ways, this is somewhere in between "bespoke monad" and the free effect implementation, but backed by a freer monad.

The true advantage of the more heavy-weight free(r) library packages is in composition of effects, and in dependency injection for testing. Using those libraries, it's significantly easier to swap out the implementation for any individual effect, and mix-and-match implementations of each different individual effect without changing the actual business logic. This is good for testing and such. I have an example using polysemy for this for a networks school assignment, where I wrote the logic out first in a high level dsl, and then was able to implement the closer-to-hardware stuff that the dsl does independently of the high-level business logic. I'll dig that out for you in a second
