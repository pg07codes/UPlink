
# livink
> Get all links from a webpage filtered by status codes, status classes and many more

![logo](https://user-images.githubusercontent.com/34238240/82594261-6e50bd80-9bc1-11ea-9176-9b9b99be48ed.png)

  
## Install
```
$ npm install livink
```

## Usage

```js
const livink = require('livink');

// use without any configuration like this

livink("https://any-valid-url-here.com").then(res=>{
console.log(res);
});
/* sample output of all links (img,href,mailto,etc)
[
  { link: 'https://xyz.com/domains/example', returnedStatus: 200 },
  { link: 'https://xyz.com/example', returnedStatus: 404 },
  { link: 'https://xyz.com/some-page#ok', returnedStatus: 200 },
  { link: 'https://hello.com/new-page/id', returnedStatus: 500 },
  { link: 'https://xyz.com/', returnedStatus: 400 },
  { link: 'https://abc.com/do/it/page', returnedStatus: 405 },
  { link: 'https://abc.com/domains/example', returnedStatus: 200 },
  { link: 'https://xyz.com/domains/id?q=true', returnedStatus: 301 },
  { link: 'https://pqr.com/example', returnedStatus: 203 }
]
*/

```
### To filter out results based on status codes, you can pass a configuration object which is structured as
```
{
status: can be a number(status code) or array of status codes,
statusRange: must be an two item array[min-status-code,max-status-code]
statusClass: can be a string(status class) or array of status class
}
```
- note 1: string/string array value of `statusClass` must be from these strings-  `'info'`, `'success'`, `'redirect'`, `'clientErr'`, `'serverErr'` only. Visit [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status) to see why they are named like this.
- note 2: only one of the three keys `status`,`statusRange`,`statusClass` must be passed in configuration object as of now. Make an issue if you need to use them simultaneously.

```js

// use with config object like this

livink("https://any-valid-url-here.com",{
	status:404    // filters links with 404 status
}).then(res=>{
console.log(res);
})
/* sample output
[
  { link: 'https://xyz.com/example', returnedStatus: 404 }
]
*/

// or like this

livink("https://any-valid-url-here.com",{
	status:[200,404,500]   // filters links with status codes in array
}).then(res=>{
console.log(res);
})
/* sample output
[
  { link: 'https://xyz.com/domains/example', returnedStatus: 200 },
  { link: 'https://xyz.com/example', returnedStatus: 404 },
  { link: 'https://xyz.com/some-page#ok', returnedStatus: 200 },
  { link: 'https://hello.com/new-page/id', returnedStatus: 500 },
  { link: 'https://xyz.com/', returnedStatus: 400 },
  { link: 'https://abc.com/domains/example', returnedStatus: 200 },
]
*/
```
```js
// or use with statusRange 

livink("https://any-valid-url-here.com",{
	statusRange:[203,410]   // filters links between 203 and 410(including both)
}).then(res=>{
console.log(res);
})
/* sample output
[
  { link: 'https://xyz.com/example', returnedStatus: 404 },
  { link: 'https://hello.com/new-page/id', returnedStatus: 500 },
  { link: 'https://abc.com/do/it/page', returnedStatus: 405 },
  { link: 'https://xyz.com/domains/id?q=true', returnedStatus: 301 },
  { link: 'https://pqr.com/example', returnedStatus: 203 }
]
*/
```

```js

// or use with statusClass

livink("https://any-valid-url-here.com",{
	statusClass:'clientErr'  // filters links with status codes which fall in status class of clientError ie. between 400-499. 
}).then(res=>{ 
console.log(res);
})
/* sample output
[
  { link: 'https://xyz.com/example', returnedStatus: 404 },
  { link: 'https://xyz.com/', returnedStatus: 400 },
  { link: 'https://abc.com/do/it/page', returnedStatus: 405 }
]
*/

```

**Finding this package useful, star this repo**
**Found a bug or want a feature, make an issue for it**




