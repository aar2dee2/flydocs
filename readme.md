# Elixir/Phoenix Deployment Error - "Release Command Failed, Deployment Aborted"

If you've made it here you're almost done with deploying your app on Fly!

Here are a few tips to get you started
- To get more information on the error, run `flyctl logs` or `fly logs`.
- Also check that your `config/runtime.exs` is configured as per the [Phoenix Guide for Deploying on Fly](https://hexdocs.pm/phoenix/fly.html#runtime-configuration).

## Troubleshooting common error messages:

  ### Release module not available
  ```output
  (UndefinedFunctionError) function xxx.Release.migrate/0 is undefined (module xxx.Release is not available)
  ```
  
  Just check that the correct app name is used in the release command in `fly.toml`. If you're following the [Fly Phoenix Guide for version 1.6.3 or earlier](https://fly.io/docs/getting-started/legacy_elixir/) make sure you've changed `HelloElixir.Release.migrate` in the release command to `YourApp.Release.migrate`.
  
  
  ### Database Connection error
  ```output
  [error] Postgrex.Protocol (#PID<0.162.0>) failed to connect: ** (DBConnection.ConnectionError) tcp connect
  ```
  
  You can check your db status with `fly status -a <app-db>` (e.g. `fly status -a hello-elixir-db`). If the database is up and running, check that `[:inet6]` is configured in socket_options in `config/runtime.exs`.
  ```elixir
  config :web_app, WebApp.Repo,
    socket_options: [:inet6]
  ```
   If you don't see your database running, check that the database is setup and attached to the app, and the `DATABASE_URL` variable has been set (this should show up with `flyctl secrets list`). You can refer to the [Postgres on Fly guide](https://fly.io/docs/reference/postgres/) for setting up and attaching your database.
 
 
## Helpful threads from the Fly Community
  - [Unable to deploy Elixir app, no feedback in output or logs](https://community.fly.io/t/unable-to-deploy-elixir-app-no-feedback-in-output-or-logs/1664/26)
  - [Error release command failed, deployment aborted](https://community.fly.io/t/error-release-command-failed-deployment-aborted/2918)
  - [Errors deploying new pg cluster - :nxdomain](https://community.fly.io/t/errors-deploying-new-pg-cluster-nxdomain/3432)


If the above do not help resolve the error, please share your app instance's `fly.toml` and logs (Run `fly logs -a <app_name>` ) and we'll investigate further. You can post on [Fly Community](https://community.fly.io/) or write to support@fly.io
