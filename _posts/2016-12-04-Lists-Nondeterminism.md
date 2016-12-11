---
layout: post
title: Lists as Non-Determinism
---

Reading through "Learn you a Haskell" I was a little shocked with the one-liner for calculating the power set of a list. This is what ensued. 


<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

## FilterM

So check out this one liner for powerset:

```haskell
powerset :: [a] -> [[a]]  
powerset xs = filterM (\x -> [True, False]) xs  
```

How does this work at all? It lead to the source code for filterM:

```haskell
filterM          :: (Applicative m) => (a -> m Bool) -> [a] -> m [a]
filterM p        = foldr (\x -> q x (p x)) (pure [])
					where
						q x = liftA2 (\flg -> if flg then (x:) else id)
```

Of course, we can and should compare this to the definition for the regular filter:

```haskell
filter :: (a -> m Bool) -> [a] -> m [a]
filter p = foldr (\x -> q x (p x)) []
				where
					q x = \flg -> if flg then (x:) else id
```
So really all we've done is lift `q`. We really see how this works when we write out the fold:

```haskell
filter p (x:xs) = q' (p x) (filter p xs)
	where q' bool ls = if bool then x:ls else ls

filterM p (x:xs) = liftA2 q' (p x) (filter p xs)
	where q' bool ls = if bool then x:ls else ls
```

Now write the `filterM` in applicative style:

```haskell
filterM p (x:xs) = q' <$> p x <*> filter p xs
	where q' bool ls = if bool then x:ls else ls
```
Now consider how `<*>` works for lists. In particular, `(+) <*> [1,2] <*> [3,4] = [4,5,5,6] = [a+b | a<-[1,2], b<-[3,4]]`. That is, we apply every + in every combination. 

So now finally we have

```haskell
powerSet (x:xs) = filterM (\_ -> [True, False]) (x:xs) 
		= [q' bool ys | bool<-[True, False], ys<-powerSet xs]
		= powerSet xs ++ [x:ys | ys<-powerSet xs]
```

which is much easier to understand. 