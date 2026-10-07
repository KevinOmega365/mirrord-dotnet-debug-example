# Debugging .NET Applications with mirrord

## Overview

This is a sample web application built with ASP.NET Core and Redis to demonstrate debugging Kubernetes applications using mirrord. The application is a guestbook that stores entries using Redis and displays them on a web interface.

## Prerequisites

* VS Code with C# extension
* .NET 9.0 SDK or higher
* Docker and Docker Compose
* kind (K8s in Docker)
* mirrord CLI installed
* Windows WSL2

## CLI Quick Start

### 1. Kind of a Cluster

1. Open Docker Desktop
1. Build a container ```docker build -t dotnet-guestbook:local .```
1. Start a cluster```kind create cluster```
1. Make the container available ```kind load docker-image dotnet-guestbook:local```
1. Create the pods ```kubectl create -f ./kube```

### 2. Hook the local version into the cluster

1. ```mirrord exec -f mirrord.json -- dotnet run --project src```
1. <http://localhost:8080>
1. ctrl-c to stop mirrord
1. Make a change to Program.cs (chage the h1 text on line 125)
1. ```mirrord exec -f mirrord.json -- dotnet run --project src```
1. <http://localhost:8080>
1. ctrl-c to stop mirrord

### 3. VS Code debugger

1. Install the mirrord extension
1. ctrl + shift + P > mirrord: Change Settings
1. Copy mirrord.json into the .mirrord folder
1. Click mirrord in the VS Code footer to start it
1. Run the debugger (F5)
1. <http://localhost:8080/>
1. put a break point on line 155
1. try adding a new guestbook entry
1. check the value of message in the debug console

## Cleaning up

1. ```kind delete cluster```
1. Docker > Images: delete dotnet-guestbook
1. Docker > Images: delete kindest/node

## Notes

### Architecture

The application consists of:

* ASP.NET Core web server
* Redis instance for storing guestbook entries

### License

This project is licensed under the MIT License - see the LICENSE file for details.
