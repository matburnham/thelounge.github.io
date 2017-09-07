---
layout: documentation
title: Heroku
category: Deployment
order: 4.3
---

# Heroku

This document will explain how to install The Lounge on Heroku. If you want to learn about Heroku you should read their [documentation](https://devcenter.heroku.com/articles/getting-started-with-nodejs#introduction).

<div class="alert alert-warning" role="alert">
  <p>
    Please be aware that Heroku automatically kills unpaid apps after 1 hour of inactivity, and then spins them back up the next time a request comes in.
    This does not apply to paid accounts.
    If you scale up to two servers and pay for the second one, you get two always-on servers.
    <a href="https://devcenter.heroku.com/articles/dynos\#dyno-sleeping">Read more</a>
  </p>

  <p>
    When Heroku kills The Lounge, you need to connect to servers and channels again from scratch.
    In practice, you get no always-on functionality with an unpaid Heroku account.
  </p>
</div>

As a hack, you can use a service like <a href="https://uptimerobot.com/">Uptime Robot</a> to
fetch a page every half hour or so. This will act as a keep-alive.

### Step 1:

Begin by logging in with the [Heroku toolbelt](https://toolbelt.heroku.com/):

```
heroku login
```

### Step 2:

Clone the repository and install The Lounge from source:

```
git clone https://github.com/thelounge/lounge
cd lounge
npm install
NODE_ENV=production npm run build
```

### Step 3:
Comment out lines 10 to 14 in `.gitignore` to ensure that all built files are
committed to your repository.

### Step 4:
Heroku works by forwarding a port specified in the `PORT` environment variable to
port `80`. Modify the `port` configuration value to use Heroku's environment variable:

```
port: process.env.PORT,
```

### Step 5:

In the `lounge/` directory, run:

```
heroku create
```

### Step 6: (optional for user access control)

Set configuration variables:

```
heroku config:set LOUNGE_HOME=/app
```

To create users, run the following in the `lounge/` directory:

```
LOUNGE_HOME=. ./index.js add <username>
```

This will still default to putting your config in ~/.lounge, so you'll need to copy the users directory across to your lounge/ directory. Beware that this will put your hashed password in a config file - think whether you want to commit that to a public respository.

### Step 7:

Time to publish to Heroku!

If you've made any changes to the repository (like adding users or the Profile), don't forget to save the changes with `git`:

```
git add .
git commit -m "Added Heroku files"
```

And with that done, lets go ahead and push to Heroku:

```
git push heroku
```
