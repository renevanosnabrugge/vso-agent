# Microsoft Cross Platform Build Agent

A cross platform build agent for Microsoft Visual Studio Online (VSO) and Team Foundation Server (TFS).  Supported on Mac OSX and Linux.

*NOTE: This is for the unreleased build.vnext service which is in preview for a subset of accounts*

## Pre-Reqs

### Node and Npm:
**Mac OSX**: Download and install node from [nodejs.org](http://nodejs.org/)

**Linux**: Install [using package manager](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager)

From a terminal ensure at least node 0.10 and npm 1.4:
```bash
$ node -v && npm -v
v0.12.0
2.5.1
```

## Install the agent installer

Installs the agent installer once globally.  This allows you to stamp out instances of the agent.

```bash
$ sudo npm install vsoagent-installer -g
```

This does not update your agents.  It simply pulls down the latest version of the agent installer.
Stop old agents and configure new/updated agents in a new directory.
Updating an agent in place is coming soon.

### Create Agents

From a directory you created for the agent, run the installer.  Repeat from different folders for multiple agents.

```bash
$ vsoagent-installer
```

## Provide Permissions to Account

[>> VIDEO:  Configure Permissions <<](http://youtu.be/VgRpl67nOKU)

Determine which account the agent will run as.

   1. Enable alternate credentials for account agent will run builds as.
   2. Project Admin UI: (from project, gear upper right) 
      * Ensure queue created.  Name first default. (elect to create a pool if creating)
   3. Collection Admin UI: Security tab, 
      * Add user to Project Collection Build Service Accounts (allows agent to write back build data)
   4. Account Admin (Control Panel): Agent Pools tab, expand pool
      * Add user to Agent Pool Administrators (allows adding agent to pool)
      * Add user to Agent Pool Service Accounts (allows agent to listen to the build queue)

## Configure Agent

[>> VIDEO:  OSX Configure - Interactive or Service <<](http://youtu.be/ILJlYGYbXtA)

Run the vsoagent from the agent folder.
Configuration will ask for the username and password of the account the agent will run as.
note: if the agent isn't configured, on first run, it will configure.

```bash
$ node agent/vsoagent

Enter poolName(enter sets default) > 
Enter serverUrl > https://contoso.visualstudio.com
...
Config saved
Waiting ...
```

## Run Interactively

To run the agent, simply run vsoagent in the agent directory with node

```bash
$ node agent/vsoagent
```

The agent will stay running interactively as long as your log on session is active.  
If you want to keep running after you log off, consider running as a service (below), nohup (linux) or a docker container.

To test out, we recommend running interactively.  It's also useful for running UI/emulator tests that require gui interaction.  (LaunchAgent other option below)

## Run as a Service

> OSX only for right now. Linux soon

### Install Service

[OSX Types](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/DesigningDaemons.html#//apple_ref/doc/uid/10000172i-SW4-SW9)

Run in the agent directory

Run as a daemon (OSX | Linux)
```bash
$ ./agent/svc.sh install
```

Run as launch agent (only OSX)
```bash
$ ./agent/svc.sh install agent
```
*potentially run UI tests*
[Auto Logon and Lock](http://www.tuaw.com/2011/03/07/terminally-geeky-use-automatic-login-more-securely/)

### Check Status
```bash
$ ./svc.sh status
8367	-	vsoagent.myaccount.agent1
```

*note: 
    output is (pid)  (rc)  (name)
    if it is running pid will have a positive number
    rc is last exit code.  if negative, term signal number.  if postive, err return code from last run.
*

### Stop
```bash
$ ./svc.sh stop
```

### Start
```bash
$ ./svc.sh start
```

### Restart
If the service is loaded but you want to stop and start or the host has exited from some reason:
```bash
$ ./svc.sh restart
```

### Uninstall Service
Stop first and then:
```bash
$ ./svc.sh uninstall
```

### Contents

A .service file is created with the information about the service such as where the .plist file is etc...

### Update Existing Agents

Repeat the same steps to update the agent.  This will update the installer to the latest version.

```bash
$ sudo npm install vsoagent-installer -g
```

Go to the directory you created for the agent and run the same command you used to create the agent.

```bash
$ vsoagent-installer
```

## Contributing

[How to contribute](docs/contribute.md)
