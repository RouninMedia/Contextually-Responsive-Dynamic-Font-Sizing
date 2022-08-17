# Element Relative Font Sizing
*Font-sizing which decreases proportionate to the length of the text, so that titles, subtitles etc. never wrap*

If we need to set a responsive `font-size` relative to the *viewport*, this is relatively straightforward:

 - `font-size: 4%` will set the `font-size` relative to the *viewport width* 
 - `font-size: 4vw` will set the `font-size` relative to the *viewport width* 
 - `font-size: 4vh` will set the `font-size` relative to the *viewport height*

But what if you have a small piece of text *within an element* which can be anything from **8 to 40 characters**, but which you want (ideally) to have a `font-size: 18px` (or as close to that as possible)?

And what if the element itself, depending on responsive display, is sometimes `300px`, sometimes `50%` and sometimes `90vw` wide?

## HTML:
```html
<aside>
 <h2>Sidebar Heading</h2>
</aside>
```

## CSS:
```css
aside h2 {
  line-height: 27px;
  font-size: 18px;
}

aside h2.longHeading {
  font-size: var(--sidebarHeadingFontSize);
}
```

## JS:
```js
const adjustSideBarHeadingFontSizes = () => {

  let sidebar = document.querySelector('aside');
  let sidebarWidth = parseInt(window.getComputedStyle(sidebar).width.replace('px', ''));
  let sidebarHeadings = [...sidebar.querySelectorAll('h2')];

  for (let sideBarHeading of sideBarHeadings) {

    if (sideBarHeading.length > (sidebarWidth / 10.5)) {
        
      let excessLength = (sideBarHeading.length - (sidebarWidth / 10.5));
      let sideBarHeadingFontSize = (18 - (excessLength / 1.75))
      sideBarHeading.classList.add('--longHeading');
      sideBarHeading.style.setProperty('--sideBarHeadingFontSize', sideBarHeadingFontSize + 'px');
    }
  }
}

window.addEventListener('resize', adjustSideBarHeadingFontSizes);
window.addEventListener('load', adjustSideBarHeadingFontSizes);
```
