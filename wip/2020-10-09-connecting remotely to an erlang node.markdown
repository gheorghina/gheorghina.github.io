---
layout: post
title:  "Elixir Application Abnormal Behaviors Cheat Sheet"
date:   2020-10-09 00:18:23 +0700
categories: [elixir, erlang, observability]
---


### What is this about?


 Next, you need to connect to the remote node running on docker from iex on your dev node using Node.connect(:"remote@hostname"). In order to do this, you have to make sure both the epmd and the node ports on the remote machine are reachable from your local node.

Finally, once your nodes are connected, from the local iex, run :debugger.start() which opens the debugger with the GUI. Now in the local iex, run :int.ni(<Module you want to debug>) and it will make the module visible to the debugger and you can go ahead and add breakpoints and start debugging.

You can find a tutorial with steps and screenshots here. https://crypt.codemancers.com/posts/2017-11-22-elixir-remote-debugging/ 