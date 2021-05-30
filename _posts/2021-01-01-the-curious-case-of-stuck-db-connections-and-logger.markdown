---
layout: post
title:  "The curious case of stuck DBConnections in a elixir caused by RDS restart"
date:   2021-01-01 00:18:23 +0700
categories: [elixir, aws, mariadb, logger, dbconnections]
---

### The Context

The context was very well described on [DBConnection Raised issue](https://github.com/elixir-ecto/db_connection/issues/99) opened since 2017 on the elixir/db_connection github repo.

The observation in this blog post to the previous raised issues is that the problem can be reproduced with any elixir service being connected to any RDS(MySql, Postgresql) or Aurora, by just restarting the database.

As long as the connection between the service and the database is healthy, there is no problem, but as soon as the database gets restarted, the db_connection processes cannot seem to find the new database endpoint. 


### Checking the state

The problem gets visible by either checking the RDS connections count which will e less than the expected number, as for the failed node, the number of active connections is 0.

Another way to check it is by performing a small SQL query which will turn into: 

    $ Ecto.Adapters.SQL.query(Repo, "SELECT 1")

    {:error, %DBConnection.ConnectionError{ message: "connection not available and request was dropped from queue after 1471ms. This means requests are coming in and your connection pool cannot serve them fast enough. You can address this by:\n\n 1. Tracking down slow queries ....",
    reason: :queue_timeout,
    severity: :error }}

As guessed in the raised issues, this can be the situation when DBConenction tries to reconnect by using the previous DNS endpoint, as it may be cached somewhere at the OS level.
 

### Potential Fix

[Potential Fix](https://github.com/elixir-ecto/db_connection/pull/240)


### Temporary Solution

The obvious temporary solution, until the potential fix gets in place, is to implement either an outside monitor to restart the service either an inside monitor to kill the repo.

 
    defmodule Monitor do
        use GenServer
        alias Monitor

        defstruct [:name, :check_state_ms, :repo]

        def start_link(name: name, check_state_ms: check_state_ms, repo: repo)
            state = %Monitor{
                name: name,
                check_state_ms: check_state_ms,
                repo: repo
            }

            GenServer.start_link(__MODULE__, state, name: name)
        end

        @impl true
        def init(state) do
            Kerner.send(self(), :init)
            {:ok, state}
        end

        @impl true
        def handle_info(
            :init,
            %Monitor{check_state_ms: check_state_ms} = state
        ) do

            schedule_monitoring(check_state_ms)

            {:noreply, state}
        end 

        @impl true
        def handle_info(:ensure_db_connections, %Monitor{repo: repo} = state) do
            case Ecto.Adapter.SQL.query(repo, "SELECT 1") do
                {:ok, _} -> :ok
                {:error, reason} -> 
                    repo_pid = :erlang.whereis(repo)
                    Process.exit(repo_pid, :kill)
                    {:error, reason}
            end

            {:noreply, state}
        end

        defp schedule_monitoring(check_state_ms), 
            do: Process.send_after(self(), :ensure_db_connections, check_state_ms)

    end


### Resources

[DBConnection Raised issue](https://github.com/elixir-ecto/db_connection/issues/99)

[DNS Caching is causing failures on long running apps](https://github.com/xerions/mariaex/issues/180)

[Potential Fix](https://github.com/elixir-ecto/db_connection/pull/240)