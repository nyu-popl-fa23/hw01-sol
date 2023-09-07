## Solution to Problem 1

(a)

* The use of `pi` at line 4 is bound at which line?
  Line 3, because the declaration of `pi` is in scope on
  that line and shadows the declaration on line 1.

* The use of `pi` at line 7 is bound at which line?
  Line 1, because the declaration of `pi` on
  that line is the only one that is in scope on line 7.

(b)

* The use of `x` at line 3 is bound at which line?
  Line 2, because the formal parameter `x` of `f`
  shadows the outer declaration of `x` on line 1.
  
* The use of `x` at line 6 is bound at which line?
  Line 5, because the occurrence of `x` in the match
  alternative on that line binds `x` to the matched value in
  the right-hand side of the match alternative. 
  The earlier bindings of `x` on lines 2 and 3 are shadowed
  by this new binding.
  
* The use of `x` at line 10 is bound at which line?
  Line 5, for the same reason as the previous example. Note that the
  declaration of `x` on line 8 is no longer in scope on line 10.
  
* The use of `x` at line 11 is bound at which line?
  Line 1, because the declaration of `x` on that
  line is the only declaration of `x` that is in scope on line 11.


## Solution to Problem 2

(a) Execution trace:

```
   pow(2,2)
-> if 2 > 0 then 2 * pow(2, 2 - 1) else 1
-> if true then 2 * pow(2, 2 - 1) else 1
-> 2 * pow(2, 2 - 1)
-> 2 * pow(2, 1)
-> 2 * (if 1 > 0 then 2 * pow(2, 1 - 1) else 1)
-> 2 * (if true then 2 * pow(2, 1 - 1) else 1)
-> 2 * (2 * pow(2, 1 - 1))
-> 2 * (2 * pow(2, 0))
-> 2 * (2 * (if 0 > 0 then 2 * pow(2, 0 - 1) else 1))
-> 2 * (2 * (if false then 2 * pow(2, 0 - 1) else 1))
-> 2 * (2 * 1)
-> 2 * 2
-> 4
```

(b) Tail-recursive implementation

```scala
def powTail(x: Int, n: Int): Int =
  require(n >= 0)
  def powLoop(i: Int, acc: Int, x: Int): Int =
      if i <= 0 then acc
      else powLoop(i - 1, acc * x, x)
  end powLoop
  powLoop(n, 1, x)
```

Execution trace:

```
   powTail(2, 2)
-> powLoop(2, 1, 2)
-> if 2 <= 0 then 1 else powLoop(2 - 1, 1 * 2, 2)
-> if false then 1 else powLoop(2 - 1, 1 * 2, 2)
-> powLoop(2 - 1, 1 * 2, 2)
-> powLoop(1, 1 * 2, 2)
-> powLoop(1, 2, 2)
-> if 1 <= 0 then 2 else powLoop(1 - 1, 2 * 2, 2)
-> if false then 2 else powLoop(1 - 1, 2 * 2, 2)
-> powLoop(1 - 1, 2 * 2, 2)
-> powLoop(0, 2 * 2, 2)
-> powLoop(1, 4, 2)
-> if 0 <= 0 then 4 else powLoop(0 - 1, 4 * 2, 2)
-> if true then 4 else powLoop(0 - 1, 4 * 2, 2)
-> 4
```
