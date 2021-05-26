---
layout: post
title:  "The curious case of a Recurrent CPU Spike"
date:   2021-02-01 00:18:23 +0700
categories: [elixir, cpu spikes, aws, remote connection, epmd]
---

Truth being said, I am not sure event to this day how to investigate properly CPU Spikes in Elixir.

Various tools, which have $top capabilities offered no support in identifying root cause for the bellow case.

### Inception

When CPU related alarms call suddenly for CPU utilization hitting 100% suddenly with no means of completing its activity, all one can do is start digging around to check what external or internal activity can cause it.

When no increase spike in external calls happen, when no abnormal obvious internal activity is visible and when no SQL query is being slow, one can loose hope in finding root cause to such behaviors.  

What worked in this case was time, tons of patience, rabbit eyes and even a bit of paranoia during each incident in trying to correlate the CPU spike to any activity.


 ![]({{ site.url }}/static/img/high_cpu_spike.png) 


### Investigation Approaches

As mentioned above, I lack clarity on how such matters can be approached. For any advice please write a comment or just link here a resource.

As nothing seemed to help, a meticulous approach was carried on in trying to spot the issue.

The following tools and resources were used in trying to spot the cause:

   ```
      :etop.start([{:node, :"service@ip"}])

      :erlang.trace(:all, true, [:all])

      capture the trace messages:

      {:messages, messages} = Process.info(self(), :messages)

   ```

One another investigation idea at some point was that for certain there is a process crazy repeating activities by calling functions in a loop with an increased rate, therefore, different set of investigations were carried out to follow this thread, unfortunately without luck.

### The problem

I don't even remember how one day I spot the issue, by luck or by trained extra attention to everything that moved around. 

The context is of deploying elixir services via docker files in AWS ECS, as independent nodes, with custom solution for remote connection to the nodes via AWS SSM(Working with SSM Agent - AWS Systems Manager) to connect to one of the desired nodes.

Example: 

   ```
      aws ecs list-tasks --custer name --service-name service
      aws ssm start-session --target ${CLUSTER}${FARGATE_TASK_ID}${FARGATE_CONTAINER_ID})
   ```

Next step from the node is to connect to the elixir console for the service via 

   ```
      iex --remsh service-name --sname debug --cookie COOKIE
   ```

One day a sudden revelation came to check during a spike the status of connected nodes, and there it was.

It was identified that if the ssm session expires, the debug node remains connected, and throws the CPU of the service to 100%.

In order to save the situation a manual/code disconnect_node action is being required, making the CPU drop back to normal activity.

   ```
      :erlang.nodes()
      [:"debug@ip"]

      :erlang.disconnect_node(:"debug@ip")
      true
   ```

### Conclusion

As all the above is only about the discovery, the satisfaction of understanding the root cause is not met. 

I have no clue how to move forward, this blog post is also a call for action in case someone sees it and has an idea.


### Resources

[Elixir Forum Topic](https://elixirforum.com/t/expired-ssm-agent-session-where-a-debug-node-is-connected-via-iex-remsh-does-not-disconnect-automatically-the-node-leaving-nodes-state-inconsistent-bonus-cpu-on-service-node-spikes-instantly-to-100/38990) 

[AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html)
