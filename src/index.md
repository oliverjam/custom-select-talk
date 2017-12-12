# Custom Selects Aren't Worth It

**(or, The trials and tribulations of trying to do it properly)**

[@\_oliverjam](https://twitter.com/_oliverjam)
[oliverjam.es](http://oliverjam.es)

---

## Accessibility is important!

But this talk isn't about why

---

## The process

This is an overview of the process of building accessible components (not a guide)

---

## A rant

So we got given some designs...

---

## Three attempts

1. Managing focus directly
2. Using `aria-activedescendant`
3. Using live regions

---

## Spoilers

We weren't happy with any of the solutions

---

## Research

* Surprisingly few resources on basic selects
* All focused on building more complex components like autocompletes

---

## Native behaviour

The bare minimum should be to reproduce native browser behaviour

* A button that triggers a popup menu
* Usable without a mouse
* Accessible to assistive technologies

<select style="width: 200px; margin-top: 1em;">
  <option value="apples">Apples</options>
  <option value="bananas">Bananas</options>
  <option value="oranges">Oranges</options>
</select>

---

## <abbr>ARIA</abbr> roles

`aria-role` lets divs roleplay as something more interesting

---

## Listbox

> The listbox role is used to identify an element that creates
> a list from which a user may select one or more items
>
> <cite>—[MDN](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_listbox_role)</cite>

Related to "combobox" (usually used for autocompletes)

---

## Minimal markup

```javascript
<button
  aria-label="Destination select menu"
  aria-expanded="false"
  aria-haspopup="true">
  {selectedOption}
</button>
<ul
  role="listbox"
  tabindex="-1">
  <li
    id="option-0"
    role="option"
    value="tattooine">
    Tattooine
  </li>
</ul>
```

---

## First attempt

I started with the obvious: move focus to each child as it becomes 'active'

---

### Voiceover demo

<video src="assets/vid/focus-select.mp4" controls></video>

---

### Problems

* Managing focus is annoying, especially in React.
* Async state changes meant focus got out of sync
* We didn't want to rely on refs

---

## Second attempt

`aria-activedescendant` to the rescue!

> The aria-activedescendant property provides an alternative method of managing focus for interactive elements
>
> <cite>—[W3C](https://www.w3.org/WAI/GL/wiki/Using_aria-activedescendant_to_allow_changes_in_focus_within_widgets_to_be_communicated_to_Assistive_Technology)</cite>

---

### Minimal markup

```javascript
...
<ul
  role="listbox"
  tabindex="-1"
  aria-activedescendant="option-{activeIndex}">
  ...
</ul>
```

---

### Magic <abbr>ARIA</abbr> property

Just update `aria-activedescendant` on the parent with the ID of the currently active child—simple right?

---

### Voiceover demo 1: Safari

<video src="assets/vid/activedescendant-success.mp4" controls></video>

---

### Voiceover demo 2: Chrome

<video src="assets/vid/activedescendant-failure.mp4" controls></video>

---

### Problems

* The implementation is still kind of buggy
* Chromium has an [open bug](https://bugs.chromium.org/p/chromium/issues/detail?id=666049) for this
* Safari has a similar one for `role="combobox"`

---

## Third and final attempt

* How do third party libraries solve this?
* [Downshift](https://github.com/paypal/downshift) is amazing
* Live regions!

---

### Live regions

Screenreaders read out whatever text you dump in them. Allows us create a custom 'aural UI'

* `aria-live="assertive"`
* `role="status"`

---

### Minimal markup

```javascript
...
<ul
  role="listbox"
  tabindex="-1">
  ...
</ul>
<div
  class="visually-hidden"
  aria-live="assertive"
  role="status">
  {a11y message}
</div>
```

---

### Voiceover demo

<video src="assets/vid/live-select.mp4" controls></video>

---

### Problems

* Changes the default screenreader experience
* You're now responsible for everything
* Translations!

---

## What did we gain?

We spent a week researching and building—ended up with ~240 lines of code to maintain and a functionally worse component.

Not a great trade-off.

---

## The real solution

Just use a native `<select>`.

You can style the button to look right, then let users have a platform-specific experience for the actual menu.

---

## Takeaway

I don't want to discourage anyone from building cool stuff.

80% accessible is better than 0%.

But maybe don't spend a week needlessly replicating browser functionality 😉

---

## Bonus bug!

<video src="assets/vid/sticky-bug.mp4" controls></video>

---

## Resources

* [My demo code](https://github.com/oliverjam/react-a11y-select)
* [Mozilla's 'Using the Listbox role'](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_listbox_role)
* [Mozilla's 'Using the aria-activedescendant attribute'](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_aria-activedescendant_attribute)
* [Paypal's Downshift library](https://github.com/paypal/downshift)
* [Tutorials for building real components](https://inclusive-components.design/)

---

# Thanks 🔥

[@\_oliverjam](https://twitter.com/_oliverjam)
[oliverjam.es](http://oliverjam.es)
