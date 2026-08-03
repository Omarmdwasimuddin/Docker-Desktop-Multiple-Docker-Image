## Multiple Docker Image তৈরি ও ব্যবহার

একই project থেকে একাধিক (versioned) Docker image কীভাবে তৈরি করে আলাদাভাবে run করা যায়, সেটা এই ডকুমেন্টে দেখানো হয়েছে।

---

## ধাপ ১: পুরনো Docker Image Remove করা

প্রথমে VS Code এ project open করে, terminal থেকে আগের সব unused Docker image/container/cache clean করে নিতে হবে:

```bash
docker system prune -a
```

> এই command সব stopped container, unused image, network এবং build cache মুছে দেয় — একদম fresh state থেকে শুরু করার জন্য এটা useful।

---

## ধাপ ২: প্রথম Image Build করা

Terminal এ নিচের command দিয়ে image build করতে হবে:

```bash
docker build -t my-node-app .
```

Build হওয়ার পর image list চেক করার জন্য:

```bash
docker images
```

---

## ধাপ ৩: Image Run করা

Docker Desktop এ যাওয়ার সাথে সাথে build হওয়া image টা দেখা যাবে। এরপর সেটা **Run** করতে হবে।

![Docker Desktop - image created](https://imgur.com/VACCjig.png)

Container run হওয়ার পর **Containers** section এ server running state এ show করবে। assigned port এ click করলে browser এ app টা open হয়ে যাবে।

![Container running, port click to open in browser](https://imgur.com/UQ4QcKZ.png)

---

## ধাপ ৪: Code Update করা

VS Code এ গিয়ে `index.js` file update করতে হবে:

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello, World! This is a simple Express server.');
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

এখানে response message পরিবর্তন করা হয়েছে — `"Hello, World!"` থেকে `"Hello, World! This is a simple Express server."`।

---

## ধাপ ৫: নতুন Version এর Image Build করা

Updated code দিয়ে নতুন একটা tagged version এর image build করতে হবে:

```bash
docker build -t my-node-app:v2 .
```

**Tag ব্যাখ্যা:**

| অংশ | অর্থ |
|---|---|
| `my-node-app` | image এর নাম (repository) |
| `v2` | version/tag — এটা না দিলে default হিসেবে `latest` tag বসে |

---

## ধাপ ৬: দুইটা Image একসাথে Run করা

Docker Desktop এ এখন `my-node-app` (পুরনো/`latest`) এবং `my-node-app:v2` (নতুন) — দুইটা আলাদা image show করবে। দুইটাকেই আলাদা আলাদা container হিসেবে একসাথে run করা যাবে, কারণ tag ভিন্ন হওয়ায় Docker এদের আলাদা image হিসেবে treat করে।

> **Note:** দুইটা container একসাথে run করাতে চাইলে আলাদা আলাদা **host port** সেট করতে হবে (যেমন একটা `3000`, আরেকটা `3001`), নাহলে port conflict হবে।

---

## সারসংক্ষেপ (Flow)

```
docker system prune -a (পুরনো সব clean করা)
   → docker build -t my-node-app . (v1 image build)
   → run করা → verify করা
   → index.js update করা
   → docker build -t my-node-app:v2 . (v2 image build)
   → দুইটা image (v1 + v2) আলাদা container হিসেবে একসাথে run করা
```
