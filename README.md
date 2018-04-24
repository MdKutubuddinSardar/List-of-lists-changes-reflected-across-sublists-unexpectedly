# List-of-lists-changes-reflected-across-sublists-unexpectedly

When you write `[x]*3` you get, essentially, the list `[x, x, x]`. That is, a list with 3 references to the same `x`. When you then modify this single `x` it is visible via all three references to it.

To fix it, you need to make sure that you create a new list at each position. One way to do it is

    [[1]*4 for n in range(3)]

which will reevaluate `[1]*4` each time instead of evaluating it once and making 3 references to 1 list.

---

You might wonder why `*` can't make independent objects the way the list comprehension does. That's because the multiplication operator `*` operates on objects, without seeing expressions. When you use `*` to multiply `[[1] * 4]` by 3, `*` only sees the 1-element list `[[1] * 4]` evaluates to, not the `[[1] * 4` expression text. `*` has no idea how to make copies of that element, no idea how to reevaluate `[[1] * 4]`, and no idea you even want copies, and in general, there might not even be a way to copy the element.

The only option `*` has is to make new references to the existing sublist instead of trying to make new sublists. Anything else would be inconsistent or require major redesigning of fundamental language design decisions.

In contrast, a list comprehension reevaluates the element expression on every iteration. `[[1] * 4 for n in range(3)]` reevaluates `[1] * 4` every time for the same reason `[x**2 for x in range(3)]` reevaluates `x**2` every time. Every evaluation of `[1] * 4` generates a new list, so the list comprehension does what you wanted.

Incidentally, `[1] * 4` also doesn't copy the elements of `[1]`, but that doesn't matter, since integers are immutable. You can't do something like `1.value = 2` and turn a 1 into a 2.
