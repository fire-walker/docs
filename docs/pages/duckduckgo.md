## Theme from URL

First format the bookmarklet url into the format the script accepts. To do that replace the `&` with `;` and get rid of the `k` that every parameter starts with.

For example;

!!! info ""
    https://duckduckgo.com/?kae=d&ks=m&kak=-1&kax=-1&kaq=-1&kap=-1&kao=-1&kau=-1&k5=1&k7=1a1b26&kj=16161e&kx=1abc9c&k21=16161E&k18=-1&ka=e&kaa=BB9AF7&k9=C0CAF5&k8=6183BB&kt=e

Would be;

!!! info ""
    ae=d; s=m; ak=-1; ax=-1; aq=-1; ap=-1; ao=-1; au=-1; 5=1; 7=1a1b26; j=16161e; x=1abc9c; 21=16161E; 18=-1; a=e; aa=BB9AF7; 9=C0CAF5; 8=6183BB; t=e

Then just run this script replacing `ddg_cookie_input` with the formatted url. You must run it using the browser console from `https://duckduckgo.com`.

``` js
// Converts DDG cookie string into formatted JSON
const makeCookieData = (ddg_cookie_input) => {
	let ddg_json = {};
  const items = ddg_cookie_input.split(/[ ,]+/);
  items.forEach((item)=>{
    let parts = item.split('=');
    ddg_json[parts[0]] = parts[1];
  });
  return ddg_json;
}

// Iterates over JSON, and adds to browser cookie store
const setCookies = (ddg_json) => {
  Object.keys(ddg_json).forEach(function(key) {
    document.cookie=`${key}=${ddg_json[key]}`;
  });
}

// Paste your cookie data here
const ddg_cookie_input = `5=1; ay=b; bc=1; ae=d; 
ax=v261-7; 18=1; aa=0a7355; x=a8d3ff; 8=d3d5e5; 
9=00af87; j=080813; 7=0b1021; 21=080813; a=Hack; t=v`;

// Call set cookies, passing in formatted cookie data
setCookies(makeCookieData(ddg_cookie_input));

// All done, reload page for changes to take effect :)
location.reload();
```