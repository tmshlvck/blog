---
date: 2011-09-18T00:36:00.001+02:00
description: ''
published: true
slug: 2011-09-moose-postmodern-thing-in-perl-or
tags:
- Perl Moose OOP
- legacy-blogger
readtime: 5
title: Moose (a postmodern thing in Perl, or whatever) and circular reference+inheritence
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2011/09/moose-postmodern-thing-in-perl-or.html)*.

Now speaking of Moose, the postmodern object system in Perl 5, taken from Perl 6, chewed, digested and... Whatever:-) Let's assume I have a class Animal like that:<br />
<br />
<pre>package Animal;
use Moose;
use Animal::Dog;
use Animal::Cat;

has 'type' =&gt; (is=&gt;'ro', isa=&gt;'Str');

sub make_noise { print "Making noise."; }

sub specify {
  my $self = shift;

  return Animal::Dog-&gt;new({type=&gt;$self-&gt;type()}) if($self-&gt;type() eq 'dog');
  return Animal::Cat-&gt;new({type=&gt;$self-&gt;type()}) if($self-&gt;type() eq 'cat');
  return $self;
}
</pre><br />
And two subclasses:<br />
<br />
<pre>package Animal::Dog;
use Moose;
extends 'Animal';

override 'make_noise' =&gt; sub { print "Wuf!"; }



package Animal::Cat;
use Moose;
extends 'Animal';

override 'make_noise' =&gt; sub { print "Meow!"; }

</pre><br />
It simply does not work. You will get some <b>****ing</b> strange message that the first override in the Animal::Cat failed (TODO: Add here the real message...) and I can understand why: In order to compile the Animal class it has to use (=load and compile) the Animal::Dog and Animal::Cat.<br />
<br />
Any way around it? To use something like Factory design pattern known from Java. But, does it really mean I can not use any subclass in a superclass with Moose? Is it a feature or a bug?:-)