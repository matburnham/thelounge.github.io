---
layout: documentation
title: Themes
category: Plugins
order: 5.1
---

# Finding Themes

Themes published on npm should have the keyword `lounge-theme`, so you can find a list by [searching on npmjs.org](https://www.npmjs.com/search?q=keywords%3Alounge-theme).

# Installing Themes

To install theme `lounge-theme-custom`

* `npm install lounge-theme-custom`
* Add `lounge-theme-custom` to the `plugins` array in `config.js`
* Restart The Lounge
* Select theme in options

# Creating Themes

Create an npm package using the base example.json below, and publish to npm.

```
{
  "name": "lounge-theme-custom", // Replace 'custom' with your theme's name
  "version": "1.0.0",
  "description": "A theme for The Lounge",
  "main": "package.json",
  "lounge": {
    "css": "theme.css", // Set to the theme css file
    "type": "theme"
  },
  "license": "MIT",
  "homepage": "https://github.com/username/lounge-theme-custom", // Set to your homepage
  "repository": {
    "type": "git",
    "url": "git+https://github.com/username/lounge-theme-custom.git" // Set your git repository
  },
  "keywords": [
    "lounge",
    "lounge-theme"
  ],
  "bugs": {
    "url": "https://github.com/username/lounge-theme-custom/issues" // Set to a page for people to file issues
  }
}
```