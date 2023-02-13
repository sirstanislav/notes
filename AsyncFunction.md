```js
function sleep(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}
async function delay() {
  console.log("aspetto 5 secondi");
  await sleep(5000);
  console.log("sono passati 5 secondi");
}
delay();
```
