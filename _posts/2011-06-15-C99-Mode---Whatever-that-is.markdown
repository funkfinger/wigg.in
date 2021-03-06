---
layout: post
title: C99 Mode - Whatever that is...
date: "2011-06-15"
tags:
  - attiny
  - avr
  - avr-gcc
  - c
  - c99
  - code
  - electronics
  - gcc
  - makefile
---

So, I've never dabbled in C until now while using it with AVR microcontrollers - and while trying to complie a simple for loop:

<pre lang='c'>
  for (uint8_t bit = 0x80; bit; bit >>= 1) {
    ...
  }
</pre>

I got this error:

<pre>error: 'for' loop initial declaration used outside C99 mode</pre>

and to fix it I would move my decloration of the varabile outside of the loop. This seemed stupid to me so a quick Google search turned up <a href="http://cplusplus.syntaxerrors.info/index.php?title='for'_loop_initial_declaration_used_outside_C99_mode">this</a>

<blockquote>Back in the old days, when dinosaurs roamed the earth and programmers used punch cards, you were not allowed to declare variables anywhere except at the very beginning of a block.</blockquote>

Solution was to add <code>-std=c99</code> to the Makefile. My new AVR Makefile now looks like this..

<script src="https://gist.github.com/1028804.js?file=gistfile1.mak"></script>
