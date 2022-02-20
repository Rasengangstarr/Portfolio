---
title: "React Parliament Viewer Part 1"
date: 2022-02-19T14:34:18Z
draft: false
summary: "Learning React.JS by building a view of the UK Parliament API"
tags: 
- 'react'
- 'project'
- 'webdev'
mermaid: true
---
[Part 2](../parliament-viewer-2)

[Github Page](https://github.com/Rasengangstarr/ParliamentViewer)

![Phase 1 Screenshot](/images/parliamentphase1.png)

Before I got distracted by [making this Hugo site](../hugo-static-web-site), I had planned to spend the weekend learning a new Javascript framework.

My initial choice was [Solid.js](https://www.solidjs.com/), but all of the learning resources for it seem to expect you to already know [React.JS](https://reactjs.org/), so I went for that instead.

I found out yesterday that the UK Parliament has an unexpectedly extensive [API](https://members-api.parliament.uk/index.html), which you don't even need authentication to use, so I decided my project would be some view on data about Members of Parliament.

This document is written from my perspective as a Vue user, so I'll likely only point something out where its different.

# Installation and Setup

The first step was getting the tools for React development installed. This requires you to install npm, then from there install the create-react-app package.

I had all sorts of trouble with the version in the Ubuntu repositories, so downloaded their latest stable tar file and followed [these instructions](https://stackoverflow.com/questions/63312642/how-to-install-node-tar-xz-file-in-linux) to get it installed.

Once that was in place, running 

```
create-react-app ParliamentViewer
```

created a new templated app.

I followed the first part of [this tutorial](https://www.youtube.com/watch?v=Dorf8i6lCuk), which recommended deleting some of analytics and test stuff that comes by default, which I did (I'm not planning to unit test any of this for the time being), and I followed the tutorial to the point where I had a sub component and a main App component.

# Styling

I'm appauling at CSS, but i created some using the [Nord](https://www.nordtheme.com/) color scheme i've been ricing everything i use with recently:

```css
body {
  background-color: #2E3440;
  color: #e5e9f0;
  font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
}

.card {
  background-color: #D8DEE9;
  color: #434c5e;
  margin-left:3em;
  margin-bottom:1em;
  border-radius: 0.7em;
  padding: 1em;
  padding-bottom: 3em;
  padding-top: 0.1em;
  width: 20em;
  box-shadow: 5px 5px 2px #1D2330;
}

.card-actions
{
  float:right;
}

.button {
  font-size: 1em;
  padding: .5em;
  background-color: #4c566a;
  color: #D8DEE9;
  font-weight:500;
  border-radius: 0.5em;
  padding-left: 1em;
  padding-right: 1em;
}
```

# Structure

In the first instance, my application is structured as follows:

{{<mermaid>}}
graph TD;
  App-->MemberList;
  MemberList-->MemberCard1;
  MemberList-->MemberCard2;
  MemberList-->MemberCard3;
{{</mermaid>}}

- Each Member Card is responsible for rendering the name of the Member of Parliament.
- The Member List is responsible for fetching the Members in batches, then rendering the Card Elements.
- The App is the root level with a single Member List

# Member Card

```javascript
function MemberCard(props) {
  return (
    <div className="card">
      <h2>{props.name}</h2>
      <div className="card-actions">
        <button className="button">Details</button>
      </div>
    </div>
  );
}
export default MemberCard;
```

Here we can see how a basic React component is laid out. It struck me as significantly less verbose than it would be in Vue.

The props object is passed in by the parent, and we can get the individual properties we passed in off it.

# Member List

```javascript
  return (
    <>
      <h2>Members of Parliament</h2>
        {Members.map((item, id) => {
          return <MemberCard name={item.value.nameDisplayAs} />;
        })}
    </>
  );
```

Here we can see that property being passed in to 'name' on the MemberCard. I'm mapping over the Members array to show each item in it.

Again, much more terse than the v-for syntax in Vue, plus its native Javascript, which is cool and seems like it could be really powerful (could you reduce over an array like this? or filter?)

The rest of this file is where most of the new/complicated stuff is.

```javascript
import React, { useState, useEffect } from "react";
```

We import useState and useEffect at the start of the file.

useState seems to have the same function as the data section in a Vue component would, though I may be wrong.

It get's used like this:

```javascript
const [Members, fetchMembers] = useState([]);
```

Which as far as I can tell says: use fetchMembers to write the state of the Members object. Could Members now be used elsewhere in the application? I'm not sure.

useEffect, as far as I can tell from the documentation, is called every time the page is rendered - when I add more functionality, I need to check its not calling this every time something happens. 

```
  useEffect(() => {
    getAllMembers();
  }, []);
```

It may be more appropriate to use some kind of on mount method.

At any rate, this get's called when the page loads and calls my logic:

```javascript

function getAllMembers() {
    let numMembers = 650;
    let batchSize = 20;
    let numBatches = numMembers / batchSize;
    let currentBatch = 0;
    let allMembers = [];
    let promises = [];
    while (currentBatch < numBatches) {
      promises.push(getMemberBatch(currentBatch))
      currentBatch += 1;
    }
    Promise.all(promises).then(function(values)
    {
      values.forEach(v => {
        allMembers = allMembers.concat(v.items);
      });
      fetchMembers(allMembers);
    });
    
  }

  ```

  Most of this is insignificant - I have to call the members api in batches to get all the MPs back. I'm using Promise.all to get the results of all those batch fetches and putting them into one array, which is used in the HTML portion of the component.

  Notice fetchMembers(allMembers); at the end - this is what activates that useState thing from before.

  ```javascript

    var getMemberBatch = function (batchNumber) {
    return new Promise(function (resolve, reject) {
      fetch(
        "https://members-api.parliament.uk/api/Members/Search?skip" +
          batchNumber * 20 +
          "&take=20"
      )
        .then(response => response.json())
        .then(res => {
          resolve(res);
        })
        .catch(err => {
          console.log(err);
          reject(err);
        });
    });
  };

```

This is the part that does the actual work, calling the API and returning a Promise for the batch in question.

And for completeness, here is the whole component:

```javascript
import React, { useState, useEffect } from "react";
import MemberCard from "./MemberCard";

export default function MemberList() {
  const [Members, fetchMembers] = useState([]);

  var getMemberBatch = function (batchNumber) {
    return new Promise(function (resolve, reject) {
      fetch(
        "https://members-api.parliament.uk/api/Members/Search?skip" +
          batchNumber * 20 +
          "&take=20"
      )
        .then(response => response.json())
        .then(res => {
          resolve(res);
        })
        .catch(err => {
          console.log(err);
          reject(err);
        });
    });
  };

  function getAllMembers() {
    let numMembers = 650;
    let batchSize = 20;
    let numBatches = numMembers / batchSize;
    let currentBatch = 0;
    let allMembers = [];
    let promises = [];
    while (currentBatch < numBatches) {
      promises.push(getMemberBatch(currentBatch))
      currentBatch += 1;
    }
    Promise.all(promises).then(function(values)
    {
      values.forEach(v => {
        allMembers = allMembers.concat(v.items);
      });
      fetchMembers(allMembers);
    });
    
  }

  useEffect(() => {
    getAllMembers();
  }, []);

  return (
    <>
      <h2>Members of Parliament</h2>
        {Members.map((item, id) => {
          return <MemberCard name={item.value.nameDisplayAs} />;
        })}
    </>
  );
}

```

# App

Finally, the root level component is extremely dumb, simply instantiating the MemberList:

```javascript
import MemberList from "./components/MemberList";

function App() {
  return (
    <div>
      <MemberList /> 
    </div>
  );
}

export default App;

```
# Next steps

Next I'll create a modal to be opened when that 'details' button is clicked, which will show more information about each MP.