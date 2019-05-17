# pree
Watching Dats for incoming requests 👀👀👀

Making requests work in an environment like Dat which is composed solely of static files.
I go over this in detail in [my research](https://github.com/ethmimo/research/blob/master/dat/requests.md)

I built a watchtower on Dat essentially, it [prees](https://www.urbandictionary.com/define.php?term=pree) other DatArchives.

Here's how the magic happens
```js
const site = new DatArchive(window.location)
console.log(site.url)
site._loadPromise

let count = 0

async function initDat() {
  count++
  const archive = await DatArchive.create({ title: `${count}` })
  archive._loadPromise
  console.log('created archive')
  archive.watch('/request.txt', async function ({path}) {
    console.log('caught a change')
    const req = await archive.readFile(path)
    await resp(req)
    console.log('all done', count)
  })
}

async function resp(request) {
  await site.writeFile('/response.txt', request)
}
```
