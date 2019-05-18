# pree
Watching Dats for incoming requests 👀👀👀

Making requests work in an environment like Dat which is composed solely of static files.
I go over this in detail in [my research](https://github.com/ethmimo/research/blob/master/dat/requests.md)

I built a watchtower on Dat essentially, it [prees](https://www.urbandictionary.com/define.php?term=pree) other DatArchives.

Here's how the magic happens
```js

  console.log(site.url)
  site._loadPromise
  let count = 0
  async function createAndLinkDat() {
    const archive = await DatArchive.fork('dat://3f6fa0a38d5cb7fc41f54d1859cbfd4c5a7316ce3884e675271f2632b807bb2c/', {
      title: 'Enter A Name For Your Profile',
      type: ['user-profile'],
      description: "dat://1f14f5b419ca4404e378e4ecf884a19a278a8d523e4c09aa0f00a337b391c15f/"
    })
    archive._loadPromise
    console.log('forked archive', archive.url)
    archive.watch('/request.json', async function ({path}) {
      console.log('caught a change')
      const reqobj = await archive.readFile(path)
      const req = JSON.parse(reqobj).status
      const url = archive.url.slice(6, archive.url.length - 1)
      await respond(url, req)
    })
  }
  async function respond(url, request) {
    const dir = await site.readdir('/')
    if (dir.includes(url)) {
      await site.writeFile(`/${url}/status.txt`, request)
    } else {
      await site.mkdir(`/${url}`)
      await site.writeFile(`/${url}/status.txt`, request)
    }
  }
```
