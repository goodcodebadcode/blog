---
title: "Just another post. Getting started with Hugo"
description: Itatur? Quiatae cullecum rem ent aut odis in re eossequodi nonsequ.
date: 2020-09-15T11:30:03+00:00
draft: true
aliases: ["/hello-world"]
tags: ["javascript", "react", "webdev"]
categories: ["themes", "syntax"]
series: ["Getting started with Hugo"]
hideMeta: false
disableShare: false
cover:
    image: "/images/unsplash.jpg"
    alt: "A random image."
    caption: "Hello world"
    relative: false
comments: true
---


Xerum, quo qui aut unt expliquam qui dolut labo. Aque venitatiusda cum, 
voluptionse latur sitiae dolessi aut parist aut dollo enim qui voluptate ma 
dolestendit peritin re plis aut quas inctum laceat est volestemque commosa as 
cus endigna tectur, offic to cor sequas etum rerum idem sintibus eiur? Quianimin 
porecus evelectur, cum que nis nust voloribus ratem aut omnimi, sitatur? 
Quiatem. 

{{< highlight cpp >}}

#include <iostream>
using namespace std;

class Data
{
private:
    int n, m;
public:
    Data() { n = 0; m = 0; }
    Data(int n, int m) { this->n = n; this->m = m; }

    //stuff for ostream operator << ...
    //stuff for istream operator >> ...
};

{{< /highlight >}}

Nam, omnis sum am facea corem alique molestrunt et eos evelece arcillit ut aut 
eos eos nus, sin conecerem erum fuga. Ri oditatquam, ad quibus unda veliamenimin 
cusam et facea ipsamus es exerum sitate dolores editium rerore eost, temped 
molorro ratiae volorro te reribus dolorer sperchicium faceata tiustia prat.

{{< highlight js >}}

import React, { useState } from "react";
import { useAsyncEffect } from "use-async-effect2";
import cpFetch from "cp-fetch"; //cancellable c-promise fetch wrapper

export default function TestComponent(props) {
  const [text, setText] = useState("");

  useAsyncEffect(
    function* () {
      setText("fetching...");
      const response = yield cpFetch(props.url);
      const json = yield response.json();
      setText(`Success: ${JSON.stringify(json)}`);
    },
    [props.url]
  );

  return <div>{text}</div>;
}

{{< /highlight >}}

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor 
incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis 
nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. 
Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu 
fugiat nulla pariatur.
