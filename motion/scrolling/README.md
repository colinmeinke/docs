# Scrolling

There are two methods to alter scroll behaviour (scroll hijacking) on a web
page.

## Method 1

This method replaces user scroll entirely.

1. We disable user scroll by setting the page to 100% of the viewport, and
   hiding content that overflows. The user's scroll bar is hidden, and regular
   `scroll` events no longer fire. However, `wheel` and `touch` events
   still fire, but they do not have any effect.
2. We listen for these `wheel` and `touch` events and calculate the
   **direction** of the attempted scroll. Depending on our desired behaviour, we
   may also be required to calculate the **distance** of the attempted scroll.
3. We then use CSS `transform` and `transition` rules to offset the page
   against the viewport by the required distance, recreating scrolling.

### Problems with this method

- Not all users have an input that fires a `wheel` or `scroll` event. This
  method will not work for these users.
- We have to calculate scroll position ourselves based on `wheel` or `touch`
  events. This can be complex to do consistently across devices, browsers and
  inputs.

### When to use this method

- When we want to snap between each section of content. This does not require
  complex distance calculations, only a simple **up** or **down** calculation.

### Lessons to remember

- We should provide alternative navigation between each section of content.
  Otherwise a user with an input that does not fire `wheel` or `touch` events
  will not be able to navigate the page.

## Method 2

This method is in addition to user scroll.

1. At some point during the user's interaction with our web page, we decide we
   want to scroll the viewport to a certain section of content (the target).
2. We calculate the position of the target and then then rapidly increment the
   viewport's scroll position until the target is reached.

### Problems with this method

- The user may scroll at any time during this process. We can react in two ways.
    1. Abort our process and let the user take over (recommended).
    2. Ignore the user and keep updating the scroll position as planned. This
       leads to a janky tug of war with our user.
- Performance isn't perfect. It can be acceptable, depending on device and
  browser, assuming the duration is long and the ease gentle.

### When to use this method

- When we want to ease between in page links.
- When we want to snap to a section of content. For example, if the viewport has
  come to a rest between sections.

### Lessons to remember

- We should allow the user to override scroll at any time.

## [BONUS] Method 3

There is a CSS specification called
[snap points](https://drafts.csswg.org/css-scroll-snap) that is not far from
being a reality. It is currently in the process of being implemented by browser
vendors.

## Notes

Should we be altering a user's expected scroll behaviour? Is our use case so
valuable that it offsets the risk of alienating our users?

A user knows and expects their browser's scrolling behaviour to work in the
default manner. Changing this can potentially lead to a confusing and
frustrating user experience. Even changing the ease or duration of a scroll can
be jarring.
