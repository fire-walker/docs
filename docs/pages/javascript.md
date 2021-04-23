## Methods

All of these achieve the same thing.

``` js
const o = {
  fn: function () {
      // do something
  }
}

const o = {
  fn: () => {
      // do something
  }
}

const o = {
  fn () {
      // do something
  }
}

// run - do something
o.fn
```