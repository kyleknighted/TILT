# @at-root

Sometimes you need styles to be specific, sometimes less. Let's look at where [@at-root](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#at-root) can help us!

```html
<div class="wrapper">
  <ul class="list">
    <li class="list__item">Item One</li>
    <li class="list__item">Item Two</li>
    <li class="list__item">Item Three</li>
  </ul>
</div>
```

```scss
// Someone wrote too tightly specific default styles
.wrapper ul {
  margin-left: 25px;
}

// But, now we need to add additional styles to the list
.list {
  list-style: none;
  background-color: #f0f;
  margin: 0; // This won't work because the previous style is too specific

  // So, we use @at-root to render `.wrapper .list`
  @at-root .wrapper & {
    margin: 0;
  }

  // And now we continue on like normal...
  &__item {
    background-color: #aaa;
  }
}
```

This all renders as:

```scss
.wrapper ul {
  margin-left: 25px;
}

.list {
  list-style: none;
  background-color: #f0f;
  margin: 0;
}

.wrapper .list {
  margin: 0;
}

.list__item {
  background-color: #aaa;
}
```

Now everything works great!
