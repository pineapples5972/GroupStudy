---
tags:
  - Projects
  - "#CICD"
  - "#Woodpecker-CI"
  - DevOps
---

Project Link: https://git.exozy.me/nvpie/jenkins-projects
Jenkins Way: https://medium.com/@rajani103/jenkins-cicd-with-github-integration-c9790cd4d6fb

We are using Woodpecker-CI self hosted at: https://ci.exozy.me 

 Step 1: Clone the repo in the machine

`git clone https://git.exozy.me/nvpie/jenkins-projects

 Step 2 : Add the `.woodpecker.yaml` file into root (/) of repo

with the following contents:
```.woodpecker.yaml
steps:
  TEST:
    image: zsh
    commands:
      - echo "hi this is test"

  BUILD:
    image: zsh
    commands:
      - podman build . -t todoapp

  CLEAN:
    image: zsh
    commands:
      - podman rm -f todoapp || true

  RUN:
    image: zsh
    commands:
      - podman run -d -p 8989:8989 --name todoapp todoapp:latest

```

Note: Inspired by this [example file](https://git.exozy.me/a/Hello-world/src/branch/main/.woodpecker.yml)

==Since our instance runs on the `local` backend rather than the `docker` backend, passing a container image name in the `image` field will not work. Instead, we should pass one of the shells available in the system (ex. `bash`, `fish`), and that will run our commands.==

Step 3: enable the repo from woodpecker gui.

once we push this to the repo it will execute all the actions in following order:

	1. TEST
	2. BUILD
	3. CLEAN
	4. RUN

Now our instance of nodejs note app is running on http://localhost:8989


