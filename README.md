# Element Relative Font Sizing
*Font-sizing which decreases proportionate to the length of the text, so that titles, subtitles etc. never wrap*

If we need to set a responsive `font-size` relative to the *viewport*, this is relatively straightforward:

 - `font-size: 4%` will set the `font-size` relative to the *viewport width* 
 - `font-size: 4vw` will set the `font-size` relative to the *viewport width* 
 - `font-size: 4vh` will set the `font-size` relative to the *viewport height*

But what if you have a small piece of text *within an element* which can be anything from **8 to 40 characters** (ie. it doesn't have a predictable width), and:

 1. you want (ideally) to maintain as close to `font-size: 18px` as possible, regardless of the length of the text; *and*
 2. you need the element *itself* to be responsive: depending on the viewport, the element `width` will sometimes be `300px`, sometimes `50%` and sometimes `90vw`
 
 _______

## HTML:
```html
<aside>
 <h2>Sidebar Heading</h2>
 <h2>Longer Sidebar Heading</h2>
 <h2>Much Longer Sidebar Heading</h2>
 <h2>Dramatically Longer Sidebar Heading</h2>
</aside>
```

## CSS:
```css
aside {
  position: absolute;
  top: 0;
  right: 0;
  width: 200px;
  height: 300px;
  padding: 12px;
  color: rgb(255, 255, 255);
  background-color: rgb(0, 0, 127);
}

aside h2 {
  margin: 0;
  padding: 0;
  line-height: 27px;
  font-size: 18px;
}

aside h2.longHeading {
  font-size: var(--sidebarHeadingFontSize);
}
```

## JS:
```js
const adjustSidebarHeadingFontSizes = () => {

  let sidebar = document.querySelector('aside');
  let sidebarWidth = parseInt(window.getComputedStyle(sidebar).width.replace('px', ''));
  let sidebarHeadings = sidebar.querySelectorAll('h2');
  let sidebarFontSize = parseInt(window.getComputedStyle(sidebarHeadings[0]).fontSize.replace('px', ''));

  for (let sidebarHeading of sidebarHeadings) {
  
    let sidebarHeadingLength = sidebarHeading.textContent.length;

    if (sidebarHeadingLength > (sidebarWidth / 10)) {
     
      let excessLength = (sidebarHeadingLength - (sidebarWidth / 10));
      let sidebarHeadingFontSize = (sidebarFontSize - (excessLength / 2.3));      
      sidebarHeading.classList.add('longHeading');
      sidebarHeading.style.setProperty('--sidebarHeadingFontSize', sidebarHeadingFontSize + 'px');
    }
  }
}

window.addEventListener('resize', adjustSidebarHeadingFontSizes);
window.addEventListener('load', adjustSidebarHeadingFontSizes);
```

________

## Further Reading:

Dynamically adjusting the font-size of headings of unpredictable length represents a very specific edge-case in **Responsive Design** for which there is no specific tool (apart from scripts like the one above).

Many other *Responsive Technologies* are already available - or, in the case of *CSS Container Queries*, soon to be available:

### Responsive Units (`%`, `em`, `vw`, `vh`, `vmin`, `vmax`)

### Responsive Functions (`min()`, `max()`, `clamp()`)

### Intrinsic Sizing (`min-content`, `max-content`, `fit-content`)

### Responsive Displays (`display: table`, `multicol`, `display: flex`, `display: grid`)

### CSS @media queries

### CSS Container Queries (... coming soon)
 - https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Container_Queries
 - https://www.oddbird.net/2021/04/05/containerqueries/


