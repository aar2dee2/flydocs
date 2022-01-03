## Elixir/Phoenix Deployment Error - "Release Command Failed, Deployment Aborted"

Run `flyctl logs` or `fly logs` to get more information on the error.

As a first step, you can check that config/runtime.exs is configured as per the [Phoenix Guide for Deploying on Fly](https://hexdocs.pm/phoenix/fly.html#runtime-configuration)

Troubleshooting tips for some common errors:

  ### Release module not available
  ```
  (UndefinedFunctionError) function xxx.Release.migrate/0 is undefined (module xxx.Release is not available)
  ```
  
  - Correct app name is used in the release command in fly.toml (if you're following the [Fly Phoenix Guide for version 1.6.3 or earlier](https://fly.io/docs/getting-started/legacy_elixir/) make sure you've changed `HelloElixir.Release.migrate` in the release command to `YourApp.Release.migrate`)
  - 
  
  ### Database Connection error
  ```
  [error] Postgrex.Protocol (#PID<0.162.0>) failed to connect: ** (DBConnection.ConnectionError) tcp connect
  ```
  
  - Check your db status with `fly status -a <app-db>` (e.g. `fly status -a hello-elixir-db`)
  - Ensure `[:inet6]` is configure in socket_options in `config/runtime.exs`
      ```
      config :web_app, WebApp.Repo,
        socket_options: [:inet6]
      ```
      this code is commented out by default in your config/runtime.exs
  -  

  ### Other errors
  ```
  
  ```
  
  Here are some helpful threads from the Fly Community
  - [Unable to deploy Elixir app, no feedback in output or logs](https://community.fly.io/t/unable-to-deploy-elixir-app-no-feedback-in-output-or-logs/1664/26)
  - [Error release command failed, deployment aborted](https://community.fly.io/t/error-release-command-failed-deployment-aborted/2918)
  - [Errors deploying new pg cluster - :nxdomain](https://community.fly.io/t/errors-deploying-new-pg-cluster-nxdomain/3432)
