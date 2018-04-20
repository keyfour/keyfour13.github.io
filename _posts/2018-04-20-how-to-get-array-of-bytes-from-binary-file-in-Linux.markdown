---
layout: post
title: "How To Get Array of Bytes From Binary File in Linux"
date:   2018-04-20 07:39:48 +0300
categories: softwareengineering
---

Sometimes it happens that you need to put some binaries in your ``C`` code.
For example it can be firmware, predefined values or something else that you need in this case.
There is useful tool which can help us. It name is ``xxd``.

Here is help output:

    $ xxd -h
    Usage:
            xxd [options] [infile [outfile]]
        or
            xxd -r [-s [-]offset] [-c cols] [-ps] [infile [outfile]]
    Options:
        -a          toggle autoskip: A single '*' replaces nul-lines. Default off.
        -b          binary digit dump (incompatible with -ps,-i,-r). Default hex.
        -c cols     format <cols> octets per line. Default 16 (-i: 12, -ps: 30).
        -E          show characters in EBCDIC. Default ASCII.
        -e          little-endian dump (incompatible with -ps,-i,-r).
        -g          number of octets per group in normal output. Default 2 (-e: 4).
        -h          print this summary.
        -i          output in C include file style.
        -l len      stop after <len> octets.
        -o off      add <off> to the displayed file position.
        -ps         output in postscript plain hexdump style.
        -r          reverse operation: convert (or patch) hexdump into binary.
        -r -s off   revert with <off> added to file positions found in hexdump.
        -s [+][-]seek  start at <seek> bytes abs. (or +: rel.) infile offset.
        -u          use upper case hex letters.
        -v          show version: "xxd V1.10 27oct98 by Juergen Weigert".



As can be seen above there is ``-i`` options that just what needed.

Let's generate some random values:


    $ dd bs=32 count=1 if=/dev/urandom of=/tmp/rnd

And after that get our byte array with ``xxd`` command:

    $ xxd -i /tmp/rnd

The output will be similar to:

{% highlight c %}
unsigned char _tmp_rnd[] = {
  0xac, 0xca, 0xe3, 0x83, 0x38, 0x7c, 0xb1, 0x13, 0x0c, 0xd1, 0xa5, 0x5b,
  0x8d, 0xf0, 0x1e, 0x89, 0x60, 0x57, 0x95, 0xf5, 0xb1, 0xee, 0xca, 0x28,
  0x9f, 0x9d, 0xc4, 0x45, 0xdc, 0x7a, 0xa2, 0x95
};
unsigned int _tmp_rnd_len = 32;
{% endhighlight %}

The result is presented whit two values ``unsigned char`` array which is our binary file 
and ``unsigned int`` length of this array. They are named as file path that we use on
previous step with some substitutions because of constraints of ``C`` language variables
naming.

It's very simple way to get the right result and can save a lot of time.
