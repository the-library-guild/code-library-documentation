# code-library-documentation
A place for documentation that does not belong to only one repository.

**Learning Platform Project:** [link](https://app.code.berlin/projects/ckysjm8f4145610wl8e2e34fef)

# Table of Contents

- [Prolog](README.md)
- [Background Research](Research.md)
- [Roadmap](Roadmap.md)
- [Technology Overview](TechnologyOverview.md)
- [Workflow Charts](Flowcharts.md)

# Setup Guide(Prod)
## **Environment Variables**

**code-library-client**
  variables that need to be accessed in the browser are to be located in /next.config.js, 
  the rest in /.env.local. 
  required variables can be found at /process.d.ts

**code-library-server**
  all variables are found in /.env and distributed for consumption by /src/env.ts.
  required variables can be found at /process.d.ts

**code-library-perms**
  does not consume any environment variables
  
# Setup Guide (Dev)
## **Environment Variables**

**code-library-client**
  variables that need to be accessed in the browser are to be located in /next.config.js, 
  the rest in /.env.local. 
  required variables can be found at /process.d.ts

**code-library-server**
  all variables are found in /.env and distributed for consumption by /src/env.ts.
  required variables can be found at /process.d.ts

**code-library-perms**
  does not consume any environment variables

## Development Setup (tested on macOS Monterey)

**Prerequisites**

- Download and open the Docker Desktop App ([https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)), which will automatically run the docker daemon in the background

**Setting up the Workspace**

In order to run the app via docker-compose up, your workspace setup must follow this exact folder structure, including folder names:

```bash
code-library
├── code-library-client
│   └── ...
└── code-library-server
    └── ...
```

We will set this up in the following steps, using the command line:

1. `mkdir code-library && cd code-library`
2. `git clone https://github.com/the-library-guild/code-library-client`
3. `git clone https://github.com/the-library-guild/code-library-server`

**Running the Docker Containers**

Having completed the setup, we can now simply move into the client folder and tell docker to run the project, this will install all dependencies and make the library app available at [http://localhost:3000/](http://localhost:3000/)

1. `cd code-library-client`
2. `docker-compose up` (the docker daemon must be active in order for this command to work)

consult code-library-client/package.json and code-library-server/package.json for additional commands

**Troubleshooting**

- Google Login will fail in browsers other than Google Chrome during development
- run docker-compose down first
- env variables

**code-library-perms**

1. `git clone https://github.com/LinusBolls/code-library-perms && cd code-library-perms`
2. `npm i`

also gets automatically installed in both main repositories as a dependency in /node_modules
