I built this app for my website so that visitors can get to know me, and it can easily be tailored to yours

just import it and pass your own data through `props`:

```javascript
<script>
	import TwoTruths from 'two-truths'
	const promise = async () => await fetch('/some/data').then(res => res.json()) 
</script>


<TwoTruths getTruths={promise}/>
```

###`props`:

`getTruths` - an asynchronous function that returns an array of objects in the following data structure:
```json
{
	"id": "string",
	"sentence": "string",
	"is_true": "string" // this value should be 1 if the sentence is true and 0 if it's a lie
	{/* is_true is setup this way for easy integration with MySql databases like postgresql
		a BIT value in a postgres table will be returned by the Node.js pg module as "1" or "0" */} 
}
```
this function runs when the app is mounted and the returned values are used as the game data
there are no constraints on the length of the array, so be sure that you are returning 3 values (two truths and one lie), so that the rules programmed into the UI are properly implemented

the function is simply run again to reset the game when the replay button is clicked
[here](https://github.com/spope851/meDotCom/blob/main/server/server.js) is an example of a Node.js endpoint that returns three random values from a postgres table of truths and lies
