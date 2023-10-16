---
tags:
  - linear-data-structures-algorithms
  - cloning
  - pointers
  - tips-and-tricks
---
# Cloning vs. Pointers

a process' memory a lot like a long piece of tape. we can punch holes (or allocate memory) at various points all along the tape:

```
String @ 0x0001
int @ 0x0028
Object @ 0x0032
double @ 0x0060
```

each chunk of data has a position in memory, also known as a **pointer**, demonstrated with these `0xXXXX` values (these example ones aren't to scale, just fyi).

these pointers are super handy! they're what allow us to reuse memory whenever we need it, without allocating all-new memory over and over again. learning when and how to use them is very important when writing efficient software!

let's take that `Object` from the above example. if you want to reuse this `Object` elsewhere in your code, there are two ways you can do this!

# pointers

```java
Object original_obj = new Object(2);
Object reference_obj = original_obj;
```

here we have two instances of `Object`. `original_obj` and `reference_obj`. for the sake of demonstration, we assume `Object`'s constructor takes an `int` and stores it in a variable called `number`.

well, that's kind of a lie. there's only one instance here! `reference_obj` is, as the name implies, a *reference* to `original_obj`; a pointer! rather than creating another instance entirely, `reference_obj` only contains the memory address of `original_obj` under the hood, and can tell java that the memory address in question is for an `Object`.

let's see it in action!

```java
original_obj.number; // 2
reference_obj.number; // 2

reference_obj.number = 4;

original_obj.number; // 4
reference_obj.number; // 4
```

note how both `original_obj` and `reference_obj`'s `number`s changed, even though we only changed the `number` of one of these `Object`s. this demonstrates how references share the exact same pool of data in memory!

# cloning

```java
Object original_obj = new Object(2);
Object clone_obj = original_obj.clone();
```

now we have the same `original_obj` as before, but a new `clone_obj`, which `clone`s from the original. for demonstration, it's assumed that this `Object` has the `clone()` method implemented.

unlike the previous example, `clone_obj` is new data! we've allocated new space in memory for this instance, and they are now untethered from each other.

let's see how that works!

```java
original_obj.number; // 2
clone_obj.number; // 2

clone_obj.number = 4;

original_obj.number; // 2
clone_obj.number; // 4
```

`original_obj` remains unchanged, but `clone_obj.number` has been changed independently from its original! likewise, we can change `original_obj.number` without affecting the data of its clone.