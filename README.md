<p align="center">
  <img width="40%" src="https://uploads-ssl.webflow.com/5eeba6a68ba54530ffd09006/5ef50dd56e5da25b1e65f1ad_header.png"><br/>
  <a href="https://buildbuddy.io">BuildBuddy</a> is an open source Bazel build event viewer, result store, and remote cache.<br/><br/>
  <a href="https://github.com/buildbuddy-io/buildbuddy/blob/master/LICENSE"><img src="https://img.shields.io/badge/Licence-MIT-brightgreen.svg" /></a>
  <a href="https://github.com/buildbuddy-io/buildbuddy/actions"><img src="https://img.shields.io/github/workflow/status/buildbuddy-io/buildbuddy/CI" /></a>
  <a href="https://github.com/buildbuddy-io/buildbuddy/releases"><img src="https://img.shields.io/github/v/release/buildbuddy-io/buildbuddy?color=brightgreen" /></a>
  <a href="https://slack.buildbuddy.io"><img src="https://img.shields.io/badge/slack-join-brightgreen" /></a>
</p>

# Intro

BuildBuddy is an open source Bazel build event viewer, result store, and remote cache. It helps you collect, view, share and debug build events in a user-friendly web UI.

It's written in Golang and React and can be deployed as a Docker image. It's run both as a [cloud hosted service](https://buildbuddy.io) and can be deployed to your cloud provider or run on-prem. BuildBuddy's core is open sourced in this repo under the [MIT License](https://github.com/buildbuddy-io/buildbuddy/blob/master/LICENSE).

# Get started

## Starting the Master Server

1. The default configuration file for the master server is located at: `enterprise/config/buildbuddy.local.yaml`.
   
2. To start the master server, execute the following command:
   ```
   bazel run //enterprise/server:server
   ```

## Starting the Executor

1. The default configuration file for the executor is located at: `enterprise/config/executor.local.yaml`.

2. To start the executor, execute the following command:
   ```
   bazel run //enterprise/server/cmd/executor:executor
   ```

### Notes:

- **Configuration**: Ensure that both the master server and the executor are properly configured by reviewing and, if necessary, updating their respective configuration files before starting.
  
- **Dependencies**: Make sure all dependencies are installed and up-to-date, and that `Bazel` is properly set up on your system before running the commands.

- **Running the System**: The master server and executor can be run independently. However, for proper functionality, they should both be running concurrently in most cases. First, start the master server, and then start the executor.

---

Getting started with BuildBuddy is simple. Just add these **two lines** to your `.bazelrc` file.

**.bazelrc**

```
build --bes_results_url=https://app.buildbuddy.io/invocation/
build --bes_backend=grpcs://remote.buildbuddy.io
```

This will print a **BuildBuddy URL** containing your build results at the beginning and end of every Bazel invocation. You can command click / double click on these to open the results in a browser.

**Want more?** Get up and running quickly with our fully managed [BuildBuddy Cloud](https://buildbuddy.io) service. It's free for individuals, open source projects, and teams of up to 3.

If you'd like to host your own instance **on-prem** or in the cloud, check out our [documentation](https://github.com/buildbuddy-io/buildbuddy/blob/master/docs/introduction.md).

# Documentation

Our [documentation](https://github.com/buildbuddy-io/buildbuddy/blob/master/docs/introduction.md) gives you a full look at how to set up and configure BuildBuddy.

# Questions?

If you have any questions, join the [BuildBuddy Slack channel](https://slack.buildbuddy.io) or e-mail us at [hello@buildbuddy.io](mailto:hello@buildbuddy.io). We’d love to chat!

# Features

- **Build summary & logs** - a high level overview of the build including who initiated the build, how long it took, how many targets were affected, etc. The build log makes it easy to share stack traces and errors with teammates which makes collaborative debugging easier.
  ![Build summary & logs](https://uploads-ssl.webflow.com/5eeba6a68ba54530ffd09006/5ef50dcad5a75b86b544bb78_invocation.png)

- **Target overview** - quickly see which targets and tests passed / failed and dig into more details about them.
  ![Target overview](https://uploads-ssl.webflow.com/5eeba6a68ba54530ffd09006/5ef50dc920cf144d738c85dc_targets.png)

- **Detailed timing information** - BuildBuddy invocations include a "Timing" tab - which pulls the Bazel profile logs from your build cache and displays them in a human-readable format.
  ![Detailed timing information](https://uploads-ssl.webflow.com/5eeba6a68ba54530ffd09006/5ef50dcaa74972a17a9321f8_timing.png)

- **Invocation details** - see all of the explicit flags, implicit options, and environment variables that affect your build. This is particularly useful when a build is working on one machine but not another - you can compare these and see what's different.
  ![Invocation details](https://uploads-ssl.webflow.com/5eeba6a68ba54530ffd09006/5ef50dc9ab8ed94458c7b7ba_details.png)

- **Build artifacts** - get a quick view of all of the build artifacts that were generated by this invocation so you can easily access them. Clicking on build artifacts downloads the artifact when using either the built-in BuildBuddy cache, or a third-party cache running in GRPC mode that supports the bytestream API - like [bazel-remote](https://github.com/buchgr/bazel-remote).
  ![Artifacts](https://uploads-ssl.webflow.com/5eeba6a68ba54530ffd09006/5ef50dc937902d6619bc3c8e_4-artifacts.png)

- **Raw logs** - you can really dig into the details here. This is a complete view of all of the events that get sent up via Bazel's build event protocol. If you find yourself digging in here too much, let us know and we'll surface that info in a nicer UI.
  ![Raw logs](https://uploads-ssl.webflow.com/5eeba6a68ba54530ffd09006/5ef50dc9d9168f1d84e739c7_raw.png)

- **Remote cache support** - BuildBuddy comes with an optional built-in Bazel remote cache to BuildBuddy, implementing the GRPC remote caching APIs. This allows BuildBuddy to optionally collect build artifacts, timing profile information, test logs, and more. Alternatively, BuildBuddy supports third-party caches running in GRPC mode that support the bytestream API - like [bazel-remote](https://github.com/buchgr/bazel-remote).
- **Viewable test logs** - BuildBuddy surfaces test logs directly in the UI when you click on a test target (GRPC remote cache required).
  ![Viewable test logs](https://uploads-ssl.webflow.com/5eeba6a68ba54530ffd09006/5ef50dcc3397b445759a124b_test_log.png)

- **Dense UI mode** - if you want more information density, BuildBuddy has a "Dense mode" that packs more information into every square inch.
  ![Dense UI mode](https://uploads-ssl.webflow.com/5eeba6a68ba54530ffd09006/5ef50dca6ad0da7d3c313946_dense.png)

- **BES backend multiplexing** - if you're already pointing your `bes_backend` flag at another service. BuildBuddy has a `build_event_proxy` configuration option that allows you to specify other backends that your build events should be forwarded to. See the [configuration docs](https://github.com/buildbuddy-io/buildbuddy/blob/master/docs/config-misc.md) for more information.
  ![BES backend multiplexing](https://uploads-ssl.webflow.com/5eeba6a68ba54530ffd09006/5ef50dcca5a68708ebe347d5_multiplex.png)

- **Slack webhook support** - BuildBuddy allows you to message a Slack channel when builds finish. It's a nice way of getting a quick notification when a long running build completes, or a CI build fails. See the [configuration docs](https://github.com/buildbuddy-io/buildbuddy/blob/master/docs/config-integrations.md) for more information.
  ![Slack webhook support](https://uploads-ssl.webflow.com/5eeba6a68ba54530ffd09006/5ef50dc7caabdd3e23528f51_slack.png)
