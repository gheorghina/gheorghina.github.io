---
layout: post
title:  "Diagnosing Elixir Applications"
date:   2020-10-09 00:18:23 +0700
categories: [elixir, erlang, observability]
---


### What is this about?


This page shall be a small aggregation of useful tools and elixir or erlang modules which can help during painful investigations.

Most of the painful deep dive situations can be avoided with a good system of metrics and observability of the system. They can help identifying quicker the problems, abnormalities in behavior and also can give the direction for the detailed investigations.

What also speeds up the process of finding the root cause of issues is a good knowledge of the domain as well as the insights of the application mechanics. 

As no elixir application resembles another, some additional checks can be generalized or can be removed completely: checking messages status, checking the monitor processes, check where in the cluster a certain process resides, how is the data flow being implemented, what can be the bottlenecks...

The following list shall be a small cheat sheet, which can be used in times of pressure and uncertain grounds.


### Most common used tools

The following list of tools are meant to capture and provide a list of most common figures.

  - [:observer_cli](https://hexdocs.pm/observer_cli/) 
    The observer cli can be enabled by adding it as a dependency and as well to the mix file under the extra_application which will ensure it will be loaded but it will not be started automatically together with the system. Allowing you to start it on demand.  
    
      ```
      included_applications: [:observer_cli]
      ``` 

      ```
      :observer_cli.start()
      ```

  - [OTP runtime_tools]([https://erlang.org/doc/man/runtime_tools_app.html)
    OTP ships with a list of useful debugging tools for debugging and observing the production system. They don't have to be added to the deps list, but in order to be enabled, have to be specified as an extra_application. Mix guarantees all non-optional applications are started before your application starts.

      ```
      extra_applications: [:runtime_tools]
      ```

  - [OTP Tracing]() 
    A good suite of tracing BIFs, built-in functions, are being shipped with Erlang. Most of the job is identifying the culprit PID in order to be able to look deeper into its data.
    
    - [sys](https://erlang.org/doc/man/sys.html)
      Can be used to retrieve the state of a process but most of the time it is being used for sending system messages used by programs, and messages used for debugging purposes.
      The caveat with this in production is that it does not redirect IO to remote shells, and does not have rate limit capabilities for trace messages.

      ```
      :sys.get_state(:net_kernel)
      :sys.get_state(pid)
      :sys.get_state("process_name")
      ``` 

    - [dbg](https://erlang.org/doc/man/dbg.html)
      Used to trace everything in terms of functions, processes, ports and messages, putting a lot of load on the node.
      If the issues can be reproduced in testing environments, it is recommended to use it there instead of production, so that you will not risk putting a production node down. 

      ```
      :dbg.start() 
      :dbg.tracer() 
      :dbg.tp(Module, Function, Arity, [])   % enable global tracing
      :dbg.tpl(Module, Function, Arity, [])   % enable local tracing
      :dbg.p(all, c)   % trace calls (c) of that MFA for all processes 
      :dbg.stop_clear() 
      ```

    - [Erlang BIFs](https://erlang.org/doc/man/erlang.html)
      Erlang BIFs are available as part of the erlang module. Their lower level of abstraction makes them difficult to use.

      ```
      pid = :erlang.whereis("process_name")
      
      Process.info(pid)
      
      {:message_queue_len, count} = :erlang.process_info(pid, :message_queue_len)

      Process.info(:erlang.list_to_pid('<0.6879.0>'))

      :erlang:memory % list with information about memory dynamically allocated by the Erlang emulator

      :erlang.process_info(pid, :message_queue_len)

      :erlang.system_info(:process_count) % information about the current system by process_count

      :erlang.system_info(:port_count)

      :erlang.statistics(:total_run_queue_lengths_all) % only tasks that are expected to be CPU bound are part of the result.

      :erlang.statistics(:reductions) 

      :erlang.demonitor(sync_ref, [:flush]) 

      :erlang.monitor(:process, pid)

      :erlang.trace(pid, true, [all, {tracer, Tracer}]) %trace everything

      :erlang.trace(pid, false, [all, {tracer, Tracer}]) %stop tracing

      :erlang.trace_pattern({mymod, myfun, 3}, Match, [local])

      :erlang.trace(all, true, [call, {tracer, Tracer}])
   
      :erlang.trace(all, false, [call, {tracer, Tracer}])
      ```


    - [crash_dump](https://erlang.org/doc/apps/erts/crash_dump.html)

      Erlang ships with a crash dump viewer as part of the :observer application which allows for seeing information about running processes, ports, ETS tables, atoms, modules, memory usage, including the details about the crash.

      ```
      :crashdump_viewer.start()
      ```

    - Other common used modules, when the runtime applications is not enabled

      ```
      :inet.getstat  % for showing the port statistics

      :ets.all() |> Enum.map(fn v -> :ets.info(v, :memory) end) |> Enum.sort(&(&1 >= &2)) % show all ets resources displayed top down based on memory consumption
      ```

  - Other Tracing Libraries

      - [Tracer](https://hexdocs.pm/tracer/readme.html)

        ```
          def deps do
            [{:tracer, "~> 0.1.1"}]
          end
        ```

      - [Rexbug](https://github.com/nietaki/rexbug)

        ```
          def deps do
            [{:rexbug, "~> 0.1.1"}]
          end
        ``` 

      - [recon](https://ferd.github.io/recon/) 

        ```
        :recon.proc_count(:memory, 50) |> Enum.map(fn {pid, mem, _} -> Process.info(pid, [:memory, :registered_name, :dictionary]) end)
        ```



### Investigating Memory Leaks

  Investigating memory leaks is more straight forward, as by taking multiple snapshots in time of top processes, can be easier to spot.
  One of the example was caused by a previous version of myxql adapter which used to keep list of DBConnections in the cursors map, never releasing them, causing the related ets tables to keep growing, bringing services down due to out of memory exceptions. 
  The issue could be reproduced [here](https://github.com/gheorghina/dbconn_cursors_leak).
  Details for the fix and the described problem can be seen [here](https://github.com/elixir-ecto/ecto/issues/3311). 



### Investigating sudden CPU spikes

  Investigating CPU spikes can be more painful than memory related issues, where the tools are more helpful. When having CPU issues, it can be mainly caused by an excessive activity of s specific part of the system. For this I will prepare a separate detailed blog article.



### Alternative Resources

[Erlang in Anger](https://www.erlang-in-anger.com/)  

[Debugging Elixir Applications](https://elixir-lang.org/getting-started/debugging.html) 

[A Guide to Tracing in Elixir](https://www.erlang-solutions.com/blog/a-guide-to-tracing-in-elixir.html)

[IEx Helpers](https://hexdocs.pm/iex/IEx.Helpers.html)
 
[BIFs and NIFs in the BEAM](http://beam-wisdoms.clau.se/en/latest/eli5-bif-nif.html)

[Plataformatec tracing and observing your remote node](http://blog.plataformatec.com.br/2016/05/tracing-and-observing-your-remote-node/)
