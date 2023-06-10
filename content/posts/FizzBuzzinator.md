+++
title = "I Have Spent Far Too Long Thinking About FizzBuzz"
date = 2022-06-19

[taxonomies]
categories = ["programming"]
tags = ["open source", "rust"]
+++

Welcome brave reader. It's dangerous to go along without context. Take [this pointer](https://github.com/aadriance/fizzbuzzinator) to the source of truth on GitHub.

<!-- more -->

For those seeking after the meat of the matter, please detour around my recounting of clarity, for I am aware you have important things needing attention, and I wouldn't desire you waste a moment longer than needed not consuming the important details of my exploratory work on FizzBuzz.

## A Rare Moment of Clarity
We have all found ourselves sipping a cup of coco whilst forcing our computer to compute FizzBuzz one billion times to satisfy our deeply unjustified urge to answer the question of what FizzBuzz implementation has the best performance. In that peaceful moment as your fingers halt their march to the deranged orders of a brain consumed by a basic programming problem you might wonder how it all came to this.

For one fateful evening I happened across ["FizzBuzz in Haskell by Embedding a Domain-Specific Language"](https://themonadreader.files.wordpress.com/2014/04/fizzbuzz.pdf). In that moment my brain became infected. In that moment I knew that I could make FizzBuzz unnecessarily contrived. It is a cruel thing to enlighten a programmer to new problems that can be complicated further. I was propelled forth to write implementations of FizzBuzz, and what good would many implementations of the same algorithm be if one did not race them to determine which was superior?

Was this a waste of time? Undoubtedly. It did propel me to write this first blog post, and perhaps that makes all the difference. Although I seriously doubt it.

## Enter The FizzBuzzinator

Inspired by my personal father figure hero [Dr. Heinz Doofenshmirtz](https://en.wikipedia.org/wiki/Dr._Heinz_Doofenshmirtz), the FizzBuzzinator is ten implementations of FizzBuzz that all race to compute ten million instances of FizzBuzz. Do that all one hundred times, dump it to a CSV file, and you've got science. At least according to [Adam Savage](https://www.reddit.com/r/mythbusters/comments/3wgqgv/the_origin_of_the_remember_kids_the_only/) who got his famous phrase from ballistic expert Alex Jason.

> Remember kids, the only difference between screwing around and science is writing it down!

Testing Specifics:
- Each FizzBuzz implementation measured the time to compute FizzBuzz on all numbers 0-10,000,000.
- Each FizzBuzz implementation must do this 100 times.
- The Average, median, minimum, and maximum times for the implementation are computer from the 100 records.
- All times are recorded in seconds.

Now onto the FizzBuzz!

### A Brute Force FizzBuzz

FizzBuzz follows these rules when given a number:
- If the number is divisible by 3, Fizz
- If the number is divisible by 5, Buzz
- If the number is divisible by both, FizzBuzz
- Else return the number

Great, let's turn that into a program!

```rust
fn brute_fizzbuzz(num: u32) -> String {
    if num % 15 == 0 {
        String::from("FizzBuzz")
    } else if num % 3 == 0 {
        String::from("Fizz")
    } else if num % 5 == 0 {
        String::from("Buzz")
    } else {
        num.to_string()
    }
}
```

The implementation is a "brute force" implementation because it is a linear search of the solution space. There are minor optimizations at play, the check for 3 & 5 has been condensed into a check for divisibility by 15. Interestingly order of operations matters for two reasons:
- If 3 or 5 was checked first, you wouldn't be able to return your answer without checking divisibility by 15.
- We can take advantage of the nature of the properties of divisibility. Assuming an unbiased distribution of numbers, more inputs are divisible by 3 than 5. Checking 3 first allows performing less checks on average. More on this later.

How did it perform?

| Avg         | Median     | Min     | Max       |
| :---------- | :--------- | :------ | :-------- |
| 0.408333959 | 0.41058575 | 0.39908 | 0.4158568 |

This is our base line, it's all up (or down?) from here!

### A Worse Brute Force

If "Fizz" is a higher frequency response than "Buzz", then switching the order of operations should impact performance. Presenting a strategically worse FizzBuzz:

```rust
fn worsebrute_fizzbuzz(num: u32) -> String {
    if num % 15 == 0 {
        String::from("FizzBuzz")
    } else if num % 5 == 0 {
        String::from("Buzz")
    } else if num % 3 == 0 {
        String::from("Fizz")
    } else {
        num.to_string()
    }
}
```

What do the numbers show?

| FizzBuzz | Avg         | Median     | Min       | Max       |
| :------- | :---------- | :--------- | :-------- | :-------- |
| Brute    | 0.408333959 | 0.41058575 | 0.39908   | 0.4158568 |
| Worse    | 0.40853067  | 0.4104857  | 0.3998531 | 0.4113386 |

That's worse for sure. Not horrible, but worse.

Considering the nature of FizzBuzz, 1/3 answers will be Fizz, 1/5 will be Buzz, 1/15 will be FizzBuzz. Ish, you have overlap between the Fizz and Buzz ratio with the FizzBuzz ratio. To be precise:
- 1/15(6.6666-%) = FizzBuzz
- 4/15(26.6666-%) = Fizz 
- 2/15(13.3333-%) = Buzz
- 8/15(53.3333-%) = Number

This can be used to estimate performance of the "brute force" methods.

Normal brute force:
- 1/15 will require one evaluation
- 4/15 will require two evaluations
- 10/15 will require three evaluations
- Average of 2.6 evaluations

Wait why did Buzz and Number get combined? Falling through an if doesn't require an extra computation. I does require an extra jump, but I am counting evaluations here and not branches.

For worse brute:
- 1/15 will require one evaluation
- 2/15 will require two evaluations
- 12/15 will require three evaluations
- Average of 2.73333- evaluations

While similar, the worse brute force implementation on average requires more computations.

### An Accumulative Approach

As a software engineer one may be acquainted with [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself). The brute force solution is unfortunately damp. Checking for divisibility by 15 is a repetition of an answer that could be ascertained from the combination of results from checking 5 and 3. Here is a DRY version of FizzBuzz that accumulates the results of divisibility checks:

```rust
fn accumulate_fizzbuzz(num: u32) -> String {
    let mut result = String::from("");
    if num % 3 == 0 {
        result += "Fizz";
    }

    if num % 5 == 0 {
        result += "Buzz";
    }

    if result.is_empty() {
        result += &num.to_string();
    }

    result
}
```

In order to avoid the repetition the string can be built as the combination of results from the individual check. How does it preform?

| FizzBuzz   | Avg         | Median     | Min       | Max       |
| :--------- | :---------- | :--------- | :-------- | :-------- |
| Brute      | 0.408333959 | 0.41058575 | 0.39908   | 0.4158568 |
| Accumulate | 0.619932373 | 0.61990625 | 0.6188068 | 0.6226539 |

Oh, a horrible mistake has been made. Why is the clean, DRY, engineered version of FizzBuzz slower?

Strings.

The original implementation consisted of modulus operators, and branching. In the accumulative implementation a string is being dynamically generated. The act of concatenating strings is no simple matter. Memory buffers have to be extended, contents copied, it's a whole affair!

FizzBuzz is not doomed to be waterlogged, there are other data types that can be accumulated besides strings.

### Accumulating Bits

Building a string might be natural, as the output is a string, but it's a costly operation. In place of a string FizzBuzz can be thought of as a two bit set of flags. If the first bit is set, Fizz is present, if the second bit is set, Buzz is present, if both bits are set, then both are present and it represents FizzBuzz.

- 0b00 = Number
- 0b01 = Fizz
- 0b10 = Buzz
- 0b11 = FizzBuzz

Those bits can represent an index into an array of answers, and side step the string concatenation problem:

```rust
fn bitaccumulate_fizzbuzz(num: u32) -> String {
    let results: [&str; 3] = ["Fizz", "Buzz", "FizzBuzz"];
    let mut idx = 0;

    if num % 3 == 0 {
        idx += 1;
    }

    if num % 5 == 0 {
        idx += 2;
    }

    if idx == 0 {
        num.to_string()
    } else {
        String::from(results[idx - 1])
    }
}
```

That should be a DRY approach rivaling the powers of the Brute implementation.

| FizzBuzz       | Avg         | Median     | Min       | Max       |
| :------------- | :---------- | :--------- | :-------- | :-------- |
| Brute          | 0.408333959 | 0.41058575 | 0.39908   | 0.4158568 |
| Bit Accumulate | 0.431947604 | 0.4321037  | 0.4271722 | 0.4353882 |

It's in the same ballpark now, but slower. What gives?

Recalling the discussion on the worse brute force implementation, the original brute force method takes 2.6 calculations on average to compute the answer. This bit accumulating approach:
- Always requires 2 modulus operations.
- Always requires 3 checks.
- Requires 0-2 addition operations to accumulate bits.

This solution is more complicated computationally. It doesn't get the advantage of early returns the brute force implementation offers, and requires more operations to arrive at the answer.

### A Compositional Tree Search

Going down the DRY path didn't return great results. What about reshaping the view of the problem. What if instead of a linear search of the problem space, a tree structure was used? How does FizzBuzz turn into a tree structure? With a fair amount of squinting and head turning.

Assuming a number is divisible by 3, how many possible answers to FizzBuzz are there? Two. It has to either be Fizz or FizzBuzz. Like wise if it wasn't divisible by 3, two answers are possible, the number or Buzz. After the first check the implementation can branch into a subspace of possible answers that remain.

If you're scratching your head, no worries the code is simpler:

```rust
fn compositional_fizzbuzz(num: u32) -> String {
    fn opti_buzz(num: u32) -> String {
        if num % 5 == 0 {
            String::from("FizzBuzz")
        } else {
            String::from("Fizz")
        }
    }

    fn pessi_buzz(num: u32) -> String {
        if num % 5 == 0 {
            String::from("Buzz")
        } else {
            num.to_string()
        }
    }

    if num % 3 == 0 {
        opti_buzz(num)
    } else {
        pessi_buzz(num)
    }
}
```

There are two sub-functions, "Optimistic Buzz" and "Pessimistic Buzz". Each represents the possible solution space after determining if the number was divisible by 3. This implementation accomplishes removing the check for divisibility by 15, but does end up with additional repetition of syntax. How do the numbers look?

| FizzBuzz      | Avg         | Median     | Min      | Max       |
| :------------ | :---------- | :--------- | :------- | :-------- |
| Brute         | 0.408333959 | 0.41058575 | 0.39908  | 0.4158568 |
| Compositional | 0.407376408 | 0.40918285 | 0.397688 | 0.4127639 |

Finally, the brute of a dragon is slain. This implementation will always return an answer in 2 checks, and unlike the accumulative approach no extra computations were added in.

### Matchbox, an Exercise in Playing With Syntax

The compositional tree search was a big win for FizzBuzz implementations everywhere. That implementation is not the sole way to implement such an idea. Consider the "matchbox" implementation:

```rust
fn matchbox_fizzbuzz(num: u32) -> String {
    match num % 3 {
        0 => match num % 5 {
            0 => String::from("FizzBuzz"),
            _ => String::from("Fizz"),
        },
        _ => match num % 5 {
            0 => String::from("Buzz"),
            _ => num.to_string(),
        },
    }
}
```

It uses different syntax to express the same idea. One would expect similar performance to the compositional solution.

| FizzBuzz      | Avg         | Median     | Min       | Max       |
| :------------ | :---------- | :--------- | :-------- | :-------- |
| Brute         | 0.408333959 | 0.41058575 | 0.39908   | 0.4158568 |
| Compositional | 0.407376408 | 0.40918285 | 0.397688  | 0.4127639 |
| Matchbox      | 0.409592448 | 0.4123591  | 0.3986988 | 0.4176746 |

One should also test their assumptions before deploying to production. Frankly, I am not an expert on the innards of Rust. One would have to dig into the assembly to determine what decisions the compiler made that lead to this disparity of results. While I could do that, I have paid sufficient dividends to my sunk cost of thinking about FizzBuzz and will leave this as an exercise for the reader. I decided it was useful to leave this here as a reminder that [all syntax is equal, some are just more equal than others](https://en.wikipedia.org/wiki/Animal_Farm).

### Branchless FizzBuzz

One could declare victory now that the original brute algorithm has been usurped, but I will continue on. I once watched a talk from someone, who seemed to know what they were doing, talk about how branches are bad and branchless programs are good. I don't remember the points of the talk, but if I squint hard at my bit accumulating solution that looks like the bones of a branchless solution. Let's chuck a few more arrays in there for indexing answers from and that turns into:

```rust
fn branchless_fizzbuzz(num: u32) -> String {
    let fizzdex = [1, 0, 0];
    let buzzdex = [2, 0, 0, 0, 0];
    let results = [&num.to_string(), "Fizz", "Buzz", "FizzBuzz"];
    let mut idx = 0;
    idx += fizzdex[(num % 3) as usize];
    idx += buzzdex[(num % 5) as usize];
    results[idx].to_string()
}
```

Implementation notes:
- Num % 3 has 3 possible answers, and num % 5 has 5 possible answers, that's where the lengths of the fizzdex and buzzdex arrays come from.
- This implementation re-uses the same bit flag idea of representing FizzBuzz as the accumulative implementation.
- The results array encodes the the number as a string unlike the accumulative implementations that determined that with a branch.


How bad were the branches?

| FizzBuzz   | Avg         | Median     | Min      | Max       |
| :--------- | :---------- | :--------- | :------- | :-------- |
| Brute      | 0.408333959 | 0.41058575 | 0.39908  | 0.4158568 |
| Branchless | 0.83132986  | 0.83125575 | 0.829375 | 0.8368611 |

Ouch. That's consistent timing. Consistently bad. Our usual culprit, strings is at it again. The number is converted to a string on every computation of FizzBuzz instead of when required. No worries, let's smash some ideas from the compositional FizzBuzz solution in here and things will be all good.

### A Smart(?) Branchless FizzBuzz

The compisitional FizzBuzz contained different possible solution spaces to individual functions. Likewise the cost of converting the number to a string can be contained into a function. How is that done without branching? More arrays.

```rust
fn smartbranchless_fizzbuzz(num: u32) -> String {
    fn fizzbuzz_answer(idx: usize, _num: u32) -> String {
        let results: [&str; 4] = ["", "Fizz", "Buzz", "FizzBuzz"];
        results[idx].to_string()
    }

    fn num_answer(_idx: usize, num: u32) -> String {
        num.to_string()
    }

    let fizzdex = [1, 0, 0];
    let buzzdex = [2, 0, 0, 0, 0];
    let results = [
        num_answer,
        fizzbuzz_answer,
        fizzbuzz_answer,
        fizzbuzz_answer,
    ];

    let mut idx = 0;
    idx += fizzdex[(num % 3) as usize];
    idx += buzzdex[(num % 5) as usize];
    results[idx](idx, num)
}

```
 
The answers are now contained in functions, which are chosen based on indexing into an array. The decision of which function to calls lines up with the original result array. If index is 0, then the number must be converted to a string. In all other situations the array of possible Fizz/Buzz answers is used.

What's the performance?

| FizzBuzz         | Avg         | Median     | Min       | Max       |
| :--------------- | :---------- | :--------- | :-------- | :-------- |
| Brute            | 0.408333959 | 0.41058575 | 0.39908   | 0.4158568 |
| Branchless       | 0.83132986  | 0.83125575 | 0.829375  | 0.8368611 |
| Smart Branchless | 0.463881771 | 0.4621437  | 0.4559619 | 0.4776058 |

It would appear cleverness, while worthy of accolades during code review, is a waste of CPU cycles. Alright, maybe we should give up and branch.

### Branchful FizzBuzz

Instead of containing the number conversion to a function that is selected via array indexing let's branch:

```rust
fn branchful_fizzbuzz(num: u32) -> String {
    let results: [&str; 4] = ["", "Fizz", "Buzz", "FizzBuzz"];
    let fizzdex = [1, 0, 0];
    let buzzdex = [2, 0, 0, 0, 0];
    let mut idx = 0;
    idx += fizzdex[(num % 3) as usize];
    idx += buzzdex[(num % 5) as usize];
    if idx == 0 {
        num.to_string()
    } else {
        results[idx].to_string()
    }
}
```



There, I've de-clevered. How does this compare?

| FizzBuzz         | Avg         | Median     | Min       | Max       |
| :--------------- | :---------- | :--------- | :-------- | :-------- |
| Brute            | 0.408333959 | 0.41058575 | 0.39908   | 0.4158568 |
| Branchless       | 0.83132986  | 0.83125575 | 0.829375  | 0.8368611 |
| Smart Branchless | 0.463881771 | 0.4621437  | 0.4559619 | 0.4776058 |
| Branchful        | 0.444799967 | 0.4447638  | 0.4416377 | 0.4489821 |

Branches perform better than clever indexing it would seem. In fact, all these are slower than the number from the bit accumulation implementation.

| FizzBuzz       | Avg         | Median     | Min       | Max       |
| :------------- | :---------- | :--------- | :-------- | :-------- |
| Brute          | 0.408333959 | 0.41058575 | 0.39908   | 0.4158568 |
| Bit Accumulate | 0.431947604 | 0.4321037  | 0.4271722 | 0.4353882 |

The reality is that modern computers are well optimized for common programming constructs including branching. There are [branch predictors](https://en.wikipedia.org/wiki/Branch_predictor) and [speculative execution](https://en.wikipedia.org/wiki/Speculative_execution). Modern computers are actually a little [too eager to help us](https://meltdownattack.com/). There was a time and place for cleverness such as [Duff's device](https://en.wikipedia.org/wiki/Duff%27s_device). Modern machines and compilers encourage sane programs with common constructs. Which is good news to my ears.

### Doof FizzBuzz

Now I would be remissed in a collection of FizzBuzz implementations to not include an implementation in honor of the time that I, erm, Dr. Doofenshmirtz forgot that the modulus operator existed. Presenting, the modulus-less FizzBuzz:

```rust
fn doof_fizzbuzz(num: u32) -> String {
    fn doof_mod(num: u32, check: u32) -> bool {
        let div = num / check;
        (div * check) == num
    }

    if doof_mod(num, 15) {
        String::from("FizzBuzz")
    } else if doof_mod(num, 3) {
        String::from("Fizz")
    } else if doof_mod(num, 5) {
        String::from("Buzz")
    } else {
        num.to_string()
    }
}
```

You laugh, but:

| FizzBuzz | Avg         | Median     | Min       | Max       |
| :------- | :---------- | :--------- | :-------- | :-------- |
| Brute    | 0.408333959 | 0.41058575 | 0.39908   | 0.4158568 |
| Doof     | 0.409001909 | 0.4107218  | 0.4009717 | 0.413815  |

No record breaking results, but it preformed better than most. Turns out there are worse mistakes you can make than reinventing a basic operator.


## Conclusion

Congratulations on successfully navigating to the end of these unmerited ramblings on an introductory programming problem. I think these findings reaffirm sage words from Donald Knuth:

> We should forget about small efficiencies, say about 97% of the time: premature optimization is the root of all evil. Yet we should not pass up our opportunities in that critical 3%.

