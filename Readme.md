# Torrent all-in-one bot

Lorem ipsum i am too lazy figure what it does yourself

You might be lazy too so here ya go:

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/patheticGeek/torrent-aio-bot)

## TODO after deploy

### Minimum required to get torrent download working:

Set a variable with key "SITE" and value is the link of your site. eg. "https://\<project name>.herokuapp.com". This is important to keep bot alive or server will stop after 30 min of inactivity.

### To start a torrent bot:

Set a variable with key "TELEGRAM_TOKEN" and token of your bot as value. [How to get token](https://core.telegram.org/bots/#creating-a-new-bot)

### To get search working:

Go to the build packs section in settings and click add buildpack and enter "https://github.com/jontewks/puppeteer-heroku-buildpack.git" as buildpack url then click save changes.

Set the enviorment variables. Go to heroku dashboard open the app then go to Settings > Config vars > Reveal Config vars.

> Heroku dosent detect third party buildpack required for using puppeteer so it is recommended to git clone and then deploy to heroku after adding buildpack manually.
>
> If you do not deploy with git clone and then search wont work downloading will still work.

### To get gdrive upload:

1. Go to https://developers.google.com/drive/api/v3/quickstart/nodejs and click on Enable the Drive API
   copy client id and set an enviorment variable in heroku with name CLIENT_ID then copy client secret and set another env named CLIENT_SECRET.
2. Start download of a torrent when its complete wait a minute and in logs (open heroku project goto More > View Logs) and youll see a line with Get auth code from here. Copy the url paste it and login with your google account. At end you will get a code copy it and set another env named AUTH_CODE.
3. Start another torrent download after it finished wait for a minute youll get a value of token in logs copy that including curly brackets and set value of another env named TOKEN to it.

Finally, download a torrent and wait for some time for it to upload the torrent to gdrive and you can find the zip of torrent in home of drive

> Use this torrent for testing or when downloading to setup drive it is well seeded and downloads in ~10s
>
> magnet:?xt=urn:btih:dd8255ecdc7ca55fb0bbf81323d87062db1f6d1c&dn=Big+Buck+Bunny

## API Endpoints

prefix: /api/v1

### For downloading:

| Endpoint          |    Params    |                                                                Return |
| :---------------- | :----------: | --------------------------------------------------------------------: |
| /torrent/download | link: string | { error: bool, link: string, infohash: string errorMessage?: string } |
| /torrent/list     |     none     |                    {error: bool, torrents: [ torrent, torrent, ... ]} |
| /torrent/remove   | link: string |                  { error: bool, errorMessage?: string, link: string } |
| /torrent/status   | link: string |                 {error: bool, status: torrent, errorMessage?: string} |

link is magnet uri of the torrent

```
torrent:  {
  magnetURI: string,
  speed: string,
  downloaded: string,
  total: string,
  progress: number,
  timeRemaining: number,
  redableTimeRemaining: string,
  downloadLink: string,
  status: string,
  done: bool
}
```

### For searching:

| Endpoint        |            Params            |                                                          Return |
| :-------------- | :--------------------------: | --------------------------------------------------------------: |
| /search/{site}  | query: string, site?: string | {error: bool, results: [ result, ... ], totalResults: number, } |
| /details/{site} |        query: string         |                                         {error: bool, torrent } |

query is what you want to search for or the link of the torrent page
site is the link to homepage of proxy to use must have a trailing '/'

```
result: {
  name: string,
  link: string,
  seeds: number,
  details: string
}

torrent: {
  title: string,
  info: string,
  downloadLink: string,
  details: [ { infoTitle: string, infoText: string } ]
}
```

sites available piratebay, 1337x, limetorrent
