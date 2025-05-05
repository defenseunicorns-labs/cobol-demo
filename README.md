# COBOL Demo

A Proof of Concept (PoC) for building and running Pro*COBOL programs in containers for deployment on Kubernetes.

## Overview

This project demonstrates how to containerize and run a Pro*COBOL application with Oracle database connectivity in a modern Kubernetes environment. It leverages several open-source tools to create a fully containerized workflow.

## Prerequisites

- [melange](https://github.com/chainguard-dev/melange) - For building packages
- [apko](https://github.com/chainguard-dev/apko) - For building container images
- [k3d](https://k3d.io/) - For running a local Kubernetes cluster
- [kubectl](https://kubernetes.io/docs/tasks/tools/) - For interacting with Kubernetes
- [maru](https://github.com/defenseunicorns/maru-runner) - For running tasks
- Docker runtime
- Oracle Instant Client libraries (see below)

### Oracle Instant Client Files

Due to Oracle licensing restrictions, you need to download these files yourself:

1. Visit the [Oracle Instant Client download page](https://www.oracle.com/database/technologies/instant-client/downloads.html)
2. Navigate to the Linux x64 downloads section
3. Download the following (you'll need an Oracle account):
   - `instantclient-basic-linux.x64-19.25.0.0.0dbru.zip`
   - `instantclient-precomp-linux.x64-19.25.0.0.0dbru.zip`
4. Place these files in the `src/libs/` directory

## Project Structure

- `src/cobolite.pco` - The Pro*COBOL example application source code
- `src/container.yaml` - Configuration for building the container image
- `src/package.yaml` - Configuration for building the Wolfi package
- `src/pipelines/procob.yaml` - Pipeline for compiling Pro*COBOL code
- `manifests/` - Kubernetes deployment manifests
  - `database.yaml` - Oracle database deployment
  - `worker.yaml` - COBOL application deployment

## How It Works

1. The Pro*COBOL source code is pre-compiled and compiled using the Oracle Instant Client and GNU COBOL
2. Melange builds a package with the compiled COBOL program and its dependencies
3. Apko creates a minimal container image with the package
4. K3d deploys a local Kubernetes cluster with:
   - An Oracle database container
   - A COBOL worker container that connects to the database

## Getting Started

The full development workflow can be run with:

```bash
maru run dev
```

This command will:
1. Check for required tools
2. Generate necessary keys for Melange
3. Build the COBOL package
4. Build a container image
5. Deploy to a local k3d Kubernetes cluster
6. Run the COBOL application

## COBOL Application Details

The sample application (`cobolite.pco`) demonstrates:

- Connecting to an Oracle database using Pro*COBOL
- Retrieving environment variables for configuration
- Executing SQL queries
- Reading from and writing to files
- Processing command-line arguments

It can run in two modes:
- `import` - Reads data from a file and inserts it into the database
- `export` - Exports data from the database to a file (default mode in the demo)

## Working with the Demo

To modify the application:
1. Edit `src/cobolite.pco`
2. Run `maru run dev` to rebuild and deploy

To change database connection details, modify the environment variables in `manifests/worker.yaml`.

## License

This project is licensed under the Apache License 2.0 - see the LICENSE file for details.
