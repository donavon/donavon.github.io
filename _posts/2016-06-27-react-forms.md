---
published: true
title: React Form Components
tags: JS React
---

We’ve all looked at sample code on how to implement forms in React.
But in this article, I'm going to walk you through the three types of form components and explain the difference.
I wouldn't be surprised if many of you didn't even know there were different kinds. Just just found out recently.

I’m going to talk in particular about a Sign-In form. In the simplest case,
we have three elements: a username or email address input, a password input, and a submit button.

## Types of React Form components

There are three types of form components in React:

1. Controlled/Stateful
2. Uncontrolled/Stateful
3. Uncontrolled/Stateless


### Controlled/Stateful Forms

[Controlled components](https://facebook.github.io/react/docs/forms.html#controlled-components) that use
traditional React classes are Controlled/Stateful components and are when you see in most examples on the Interwebs.
A typical example might look like this:

{% gist d7abef94077fc0135da55a7d704d7e5e ControlledStatefulReactComponent.js %}
See this live on [CodePen](http://codepen.io/donavon/pen/vKxGLM?editors=0010)

As you can see, there are `onChange` handlers on each of the form elements.
We save the values for each element in `state`, which re-renders the form with the `value=` for each element.

That's why they are called "controlled", as we manage (or control) the state of the form elements. Any form element with a `value` is considered to be controlled. And because they are ES6 classes, they are "stateful".

But Controlled/Stateful have one _major_ flaw: they fail when using Safari or Chrome Mobile and "autofill".
Autofill is you probably know, is the feature where the browser automatically fill in username/password
information for you. The problem with the browser mentioned previously is that when they fill in the value,
they fail to fire an `onChange` event
(see the [issue filed](https://github.com/facebook/react/issues/2125) on the React repo).

And as you can see in our last example, this defeats the whole idea of the
application holding the state. You end up losing the user's input and that's a horrible experience.

The solution to this is to use an uncontrolled form.

### Uncontrolled/Stateful Forms

Uncontrolled/Stateful forms rely on the DOM to keep the state of each element. They don't listen for `onChange`
on each element either (which is why they are unaffected by Safari and Chrome's ~bug~, um, feature).
Instead you listen only for onSubmit,
at which point you interrogate the DOM directly for the value of each element. But in order to do so,
you must keep a `ref`. Check out this code below and see the difference.

{% gist d7abef94077fc0135da55a7d704d7e5e UncontrolledStatefulReactComponent.js %}
See this live on [CodePen](http://codepen.io/donavon/pen/pbeEJM?editors=0010)

Check it out! The code is much more straightforward and there is a lot less re-rendering going on
as `state` never changes. In fact, take a look. We don't use `state`. This begs the question:
If we don't use `state`, then why not use a stateless component?

### Uncontrolled/Stateless Forms

Stateless Components have been all the rage since React v0.14 and are what the cool kids are using these days.
They are simply functions and thus don't
have methods, which means no constructor and no render method either. And because they don't have methods,
they also don't have lifecycles. They are simple. They abide by the contract that given the same
input `props`, they will render the same output. This makes them highly cachable in future versions of React.

But there's one problem, stateless components don't support `ref`s. How then are we to harvest the
form data? We do it the old fashioned way, we use the `event` object to find the form elements by name.
The same way you did it back in the early conestoga days of the web.

{% gist d7abef94077fc0135da55a7d704d7e5e UncontrolledStatelessReactComponent.js %}
See this live on [CodePen](http://codepen.io/donavon/pen/yJMaOL?editors=0010)

## Conclusion

So what type of React form should you use? My recommendation would be to use one of the Uncontrolled
types, if only because of the browser issues. Personally, I gravitate towards stateless components
because of the potential caching performance benefits. Once you start using stateless components,
you won't want to go back, and your users will be the benefit from a smaller, speedier application.
And you will the their hero.
