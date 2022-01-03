## Elixir/Phoenix Deployment Error - "Release Command Failed, Deployment Aborted"

Run `flyctl logs` or `fly logs`.

Troubleshooting tips for some common errors:

  ### Release module not available
  ```
  (UndefinedFunctionError) function xxx.Release.migrate/0 is undefined (module xxx.Release is not available)
  ```
  
  - Correct app name is used in the release command in fly.toml (if you're following the [Fly Phoenix Guide for version 1.6.3 or earlier](https://fly.io/docs/getting-started/legacy_elixir/) make sure you've changed `HelloElixir.Release.migrate` in the release command to `YourApp.Release.migrate`
  - Ensure you have followed all steps in [Phoenix Deploying with Releases Guide](https://hexdocs.pm/phoenix/releases.html)
  - Your config/runtime.exs is configured as per the [Phoenix Guide for Deploying on Fly](https://hexdocs.pm/phoenix/fly.html#runtime-configuration)
  - 
  
  ### Database Connection error
  ```
  [error] Postgrex.Protocol (#PID<0.162.0>) failed to connect: ** (DBConnection.ConnectionError) tcp connect
  ```
  
  - Check your db status with `fly status -a <app-db>` (e.g. `fly status -a hello-elixir-db`)
  - `[:inet6]` is configure in socket_options in `config/runtime.exs`
  ```
  config :web_app, WebApp.Repo,
    socket_options: [:inet6]
  ```
  this code is commented out by default in your config/runtime.exs
  -  
