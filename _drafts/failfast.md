---
layout: post
title: "Need Robust Software? Make It Fragile"
date: 2015-08-04
tags: architect
description:
  How to create robust and failure-resistant software
  &mdash; by making it fragile and Fail Fast.
keywords:
  - fail fast vs fail safe
  - failure resilience
  - fail fast
  - fail safe
  - robust architecture
---

In any software project, the goal is to create something stable. We don't want it
to break in front of a user. We also don't want our website to show an
"internal application error" instead of a web page. We want our software
to work, not fail. That's a perfectly valid and logical desire,
but in order to achive that, we have to make our software as fragile
as possible. This may sound counterintuitive, but that's the way it is.
The more fragile your app is in development, the more robust it is
in production.

<!--more-->

By fragile, I'm referring to the [Fail Fast](http://martinfowler.com/ieeeSoftware/failFast.pdf)
philosophy, which is the opposite of
Fail Safe. I believe you know the difference, but let me remind you anyway,
by example. This is Fail Safe:

{% highlight java %}
public int size(File file) {
  if (!file.exists()) {
    return 0;
  }
  return file.length();
}
{% endhighlight %}

This method is supposed to calculate and return a file size. It first checks
whether the file exists. If it doesn't exist, the method returns zero. Indeed,
the file is absent, so there is no size. We could complain that the file is
absent, but what for? Why make noise? Let's keep it quiet and return zero.
We don't fail because we're trying to keep the app running. This is called Fail Safe.

To the contrary, this is how Fail Fast looks:

{% highlight java %}
public int size(File file) {
  if (!file.exists()) {
    throw new IllegalArgumentException(
      "There is no such file; I can't get its length."
    );
  }
  return file.length();
}
{% endhighlight %}

We can't find a file? We don't hide this fact. We make this situation
public and visible. We scream and cry. We throw an exception. We **want** the
app to crash, break, and fail, because someone gave us a file that doesn't
exist. We complain and protest. This is called Fail Fast.

Which philosophy, if we follow it everywhere, will make our software
robust and failure-resilient? Only the second one &mdash; the Fail Fast.

Why? Because the quicker and easier the failure is, the faster it will
be fixed. And the fix will be simpler and also more visible. Fail Fast
is a much better approach for maintainability. The code becomes cleaner.
It is much easier to track a failure. All methods are ready to break and throw
an exception on even the tiniest problem.

In this example, if the method returns zero, it's not obvious
whether the file exists and its size is actually zero or if its name is wrong
and it is just not found. The Fail Safe approach conceals problems and makes
code less maintainable, and that's why it's difficult to stabilize.

In the beginning, during production, we will have many crashes and errors. But
all of them will be visible and easy to understand. We will fix them and
cover them with unit tests. Each fix will make our software more stable
and better covered by tests.

Software designed with the Fail Safe approach in mind will look more stable
at the beginning, but it will degrade quickly and inevitably turn into 
an unmaintainable mess.

Software designed with the Fail Fast approach in mind will crash frequently
at the beginning but will improve its stability with every fix and eventually
become very stable and robust.

That's why fragility is the key success factor for robustness.