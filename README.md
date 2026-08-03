## Multiple Docker Image

#### vs e project open koro.
#### terminal --- docker images remove koro.
```bash
docker system prune -a
```
---
#### docker images build koro
```bash
docker build -t my-node-app .
```
#### docker images check
```bash
docker images
```
---

#### docker desktop e docker image create hoye jabe. then image run koro. 
![](https://imgur.com/VACCjig.png)
#### containers e server run hoye jabe. port e click korle browser e open hoye jabe.
![](https://imgur.com/UQ4QcKZ.png)
#### vs e project e index.js file update korbo.
```bash
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello, World! This is a simple Express server.');
});

app.listen(4000, () => {
  console.log('Server is running on port 4000');
});
```
--
