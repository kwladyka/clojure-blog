---
title: "When use exclamation (!) mark in names of functions?"
date: 2015-06-19
aliases:
  - /posts-output/2015-06-19-when-use-exclamation-mark.html
---

Naming is a hard thing. What `!` really mean in functions names? When to use it?

<!--more-->

On site [clojure-style-guide](https://github.com/bbatsov/clojure-style-guide#changing-state-fns-with-exclamation-mark) we can read:

> The names of functions / macros that are not safe in STM transactions should end with an exclamation mark (e.g. reset!).

But what does it really mean? Anything what has a non-idempotent side effect.

> Idempotent operation is the one that has no additional effect if it is called more than once with the same input parameters.

Make it simple:

```clojure
(create-user! ...)
```

```clojure
(create-user-only-if-not-exist ...)
```

Why do we need that? Functions in STM transactions can be run many times, because of some conflicts. So using function `create-user!` you will add the same user many times, but function `create-user-only-if-not-exist` will add him only once, even if it will be run many times.

## Conclusion

`(create-user! ...)` has additional effects if you run it more than once with the same input. For example send e-mail each time or create +1 user.

`(create-user ...)` wouldn't have additional effects even if you run this many times with the same input.