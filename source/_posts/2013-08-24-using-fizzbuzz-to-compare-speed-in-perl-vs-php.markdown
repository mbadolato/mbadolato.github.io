---
layout: post
title: "Using FizzBuzz to Compare Speed in Perl vs. PHP"
date: 2013-08-24 01:03
comments: true
categories: [perl, php, fizzbuzz, speed shootout, programming]
---

I know a lot of people dislike Perl, but I programmed a lot of large applications in it for many years. I agree that it can be difficult for programmers new to Perl to pick up on some of its nuances, but I disagree that it's a complete mess/line noise/write-only. Good clean code can be written in any language, just like horrible disgusting code can be written in any language.

A while back I saw a <a href="http://raid6.com.au/~onlyjob/posts/arena/">language speed shootout</a>, whereby the same version of a script was written in many different languages and compared against each other to see how quickly they ran. Surprisingly, Perl has been so well optimized, that its performance was right on par with the C and C++ versions of the test. PHP, Ruby, Python, Java, etc. were all comparatively much slower. You could see the iterations of each getting much longer as the memory use increased. Yet Perl (and the C's) stayed pretty consistent throughout.

Not being satisfied with someone else's benchmarks, I ran the programs on a local machine. While the times were different, the results were nearly identical in terms of performance trends.

<!--more-->

Last year when I was hiring a developer for my team, I used the [FizzBuzz](http://en.wikipedia.org/wiki/Fizz_buzz) application as a coding exercise for the candidates to [weed out the people that claimed they could program but couldn't](http://www.codinghorror.com/blog/2007/02/why-cant-programmers-program.html). Of course, this lead to a throw-down amongst the current crew as to how each of us would write it if we had too.

I eventually came up with my minimalist version, then realized that with just a few tweaks, I could make the code run under either Perl of PHP. Seeing is that FizzBuzz is a very simple program, and not a memory resource hog like the original benchmark shootout was, I was curious to see how something that simple would perform in a shootout.

I quickly modified the program and made it identical for Perl and PHP (other than the ```<php``` tag at the top of the PHP version and the shebang line at the top of the Perl version). The results of the benchmark were surprising! Even with this ultra-simple example of exactly identical code, Perl was significantly (5.5x) faster.

```
[bado.local mbadolato ~]$ perl -v

This is perl 5, version 12, subversion 4 (v5.12.4) built for darwin-thread-multi-2level
(with 2 registered patches, see perl -V for more detail)
```
```
[bado.local mbadolato ~]$ php -v
PHP 5.5.0 (cli) (built: Jun 21 2013 12:09:38)
Copyright (c) 1997-2013 The PHP Group
Zend Engine v2.5.0-dev, Copyright (c) 1998-2013 Zend Technologies
```

```
[bado.local mbadolato ~]$ time perl fizzbuzz.pl

*snip output*

real    0m0.006s
user    0m0.003s
sys     0m0.003s
```

```
[bado.local mbadolato ~]$ time php fizzbuzz.php

*snip output*

real    0m0.034s
user    0m0.020s
sys     0m0.010s
```

```perl fizzbuzz.pl
#!/usr/bin/perl

for ($x = 1; $x <= 100; $x++)
{
	$fizz = ($x % 3) ? '' : 'Fizz';
	$buzz = ($x % 5) ? '' : 'Buzz';

	printf("%s\n", ($fizz || $buzz) ? "$fizz$buzz" : $x);
}
```

```php fizzbuzz.php
<?php

for ($x = 1; $x <= 100; $x++)
{
	$fizz = ($x % 3) ? '' : 'Fizz';
	$buzz = ($x % 5) ? '' : 'Buzz';

	printf("%s\n", ($fizz || $buzz) ? "$fizz$buzz" : $x);
}
```

The output of both scripts is identical.

```
1
2
Fizz
4
Buzz
Fizz
7
8
Fizz
Buzz
11
Fizz
13
14
FizzBuzz
16
17
Fizz
19
Buzz
Fizz
22
23
Fizz
Buzz
26
Fizz
28
29
FizzBuzz
31
32
Fizz
34
Buzz
Fizz
37
38
Fizz
Buzz
41
Fizz
43
44
FizzBuzz
46
47
Fizz
49
Buzz
Fizz
52
53
Fizz
Buzz
56
Fizz
58
59
FizzBuzz
61
62
Fizz
64
Buzz
Fizz
67
68
Fizz
Buzz
71
Fizz
73
74
FizzBuzz
76
77
Fizz
79
Buzz
Fizz
82
83
Fizz
Buzz
86
Fizz
88
89
FizzBuzz
91
92
Fizz
94
Buzz
Fizz
97
98
Fizz
Buzz
```