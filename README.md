# local-simplesite-httpservers

Demonstrate various simple http-servers for local web development which
includes:

- npm [http-server: a simple static HTTP server](https://www.npmjs.com/package/http-server)

- npm [serve](https://www.npmjs.com/package/serve)

- Python [http-server](https://docs.python.org/3/library/http.server.html)

In addition, we show deployment of the simple web site to "Render.com".

## Prerequisite

- [Node.js & NPM](https://digitalcompanion.gitbook.io/home/setup/node.js)

- [Python](https://digitalcompanion.gitbook.io/home/setup/dev-environment/python)


## Setup

- Clone this repo

- Note: the `build` folder contains a very simple web site that will be served
  by the simple http-server


### npm http-server

- Make sure `node.js` exists
```
npm --version
8.11.0
```
 
- Install the npm `http-server` module
```
npm install http-server
```

- Run the Http server
```
npm run http-server

# alternative
npx http-server ./build -p 8000
```

- Open browser and go to `http://localhost:8000`

- CTRL-C to terminate

### npm serve

- Make sure `node.js` exists
```
npm --version
8.11.0
```
 
- Install the npm `serve` module
```
npm install serve
```

- Run the Http server
```
npm run serve
```

- Open browser and go to `http://localhost:8000`

- CTRL-C to terminate

### python server

- Make sure Python exists
```
python3 --version
Python 3.8.10
```

- Make the script executable
```
chmod +x run-py-server.sh
```

- Run the Http server
```
./run-py-server.sh
```


## Deploy



## References

- [Quick and easy HTTP servers for local development](https://timnwells.medium.com/quick-and-easy-http-servers-for-local-development-7a7df5ac25ff)

- [Render - Static Sites](https://render.com/docs/static-sites)