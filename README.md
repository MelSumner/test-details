# test-details

This is to test AT with the `<details>` element and various invocations

HTML spec for the `<details>` element: https://html.spec.whatwg.org/#the-details-element

From the spec:
The details element represents a disclosure widget from which the user can obtain additional information or controls.

What about accessibility?

The `details` element:

* has the implicit role of group.
* is not eligible to receive any other role
* may receive any aria-* attribute that is in the "global" list (https://www.w3.org/TR/wai-aria-1.1/#global_states)
* may receive any aria-* attributes applicable to the group role

The `summary` element:

* has the implicit role of button
* this means that all child elements should be considered presentational by assistive technology (this is not necessarily true in practice, but may be considered either a bug or coincidental that it works, not purposefully allowed by spec)
* is not eligible to receive any other role
* may receive any global aria attribute (see above)
* may receive any aria-* attributes applicable to the button role
* can only be used as the first child of the `details` element

## What really happens

Consider this code:

```html
<details>
  <summary>User Menu</summary>
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="./about">About</a></li>
    <li><a href="./contact">Contact Us</a></li>
  </ul>
</details>
```

I tried this in these combinations:

* VoiceOver and Chrome (MacOS)
* VoiceOver and Safari (MacOS)
* Firefox and NVDA (Windows)

Assistive Tech (AT) will tell the user something like this:

* "User Menu, collapsed, summary button"
* "User menu, collapsed, disclosure triangle"
* "User Menu button collapsed"

Once you press the button (technically here the summary element, although the entire elements stays focused), it will announce something like:

* "User menu, expanded, summary button"
* "User Menu, expanded, disclosure triangle"
* "User Menu button expanded"

You can then press TAB (or VO + TAB in Safari) to navigate to the next interactive element, which in this case would be the "Home" link and would be read (in most cases) like, "list with three items. home, link."

Technically this works, even if it's a bit confusing to use with a screen-reader. I expected a disclosure widget and now I'm in a list with links?

If I use my keyboard shortcuts (VO + U, then left/right arrow key through the lists)

* I will see/hear the links inside of the `details` element in the "links" list
* I will see/hear the summary inside of the form elements list but only if I am in Chrome- Safari does not have this same list.

Nothing associates the two or combines them.

Unfortunately, a11y testing is not throwing an error right now for no-nested-interactive elements, so there's no indication that doing this is harmful or undesired. /shrug
