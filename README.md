# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

# Change in index.js (Paste this piece of code in index.js inside src folder)
```
const root = ReactDOM.hydrateRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

# Install few dependencies mentioned below

express, @babel/register, @babel/preset-env, @babel/preset-react, ignore-styles

# Create a new folder called server and add two files in it as server.js and index.js

# Paste the piece of code in server.js created inside server folder

import express from "express";
import fs from "fs";
import path from "path";
import React from "react";
import ReactDOMServer from "react-dom/server";
import { StaticRouter } from "react-router-dom/server";
import App from "../src/App";

const app = express();

app.use("^/$", (req, res) => {
  fs.readFile(path.resolve("./build/index.html"), "utf-8", (err, data) => {
    if (err) {
      console.err(err);
      return res.status(500).send("Some error happened");
    }

    const html = ReactDOMServer.renderToString(
      <StaticRouter location={req.url}>
        <App />
      </StaticRouter>
    );

    return res.send(
      data.replace('<div id="root"></div>', `<div id="root">${html}</div>`)
    );
  });
});

app.use(express.static(path.resolve(__dirname, "..", "build")));

app.listen(4000, () => {
  console.log("App is launched");
});

# Paste this piece of code in index.js create inside server folder

require("ignore-styles");

require("@babel/register")({
  ignore: [/(node_modules)/],
  presets: ["@babel/preset-env", "@babel/preset-react"],
});

require("./server");

# Add a dependency to run server in package.json

"ssr": "node server/index.js"

# Make build

npm run build

# Run with node to render on server side

npm run ssr
