---
customTheme : "style"
---


### Github Actions in Action

<img src="./img/actions_vs_pipelines.jpg" style="max-height:400px;"/>
<br>
<img src="./img/team_lars.PNG" style="max-width:200px;border-radius: 20px"/>
<img src="./img/team_taw.PNG" style="max-width:200px;border-radius: 20px"/>
<img src="./img/team_martin.PNG" style="max-width:200px;border-radius: 20px"/>
<img src="./img/team_wal.PNG" style="max-width:200px;border-radius: 20px"/>



---

# Why GHA
- Ørsted is moving to <span style="color:orange">**Github**</span> soon anyway
- Ci\/Cd scripts needs <span style="color:#239aa9 ">**improvement**</span>.
- Moving to GitHub => <span style="color:red">**simplify**</span> our Ci\/CD
- GHA has very large <span style="color:#ccfc6f">**community**</span>
- Introducing <span style="color:orange">***templates***</span>
- Dependabot and other tools

---

### How to work with Github Actions?
<img src="./img/github_docs.PNG" style="max-width:1000px;"/>

---

### It looks familiar to Azure Pipelines

<img src="./img/actions_tab.PNG" style="max-width:1000px;"/>


---

### How to start

<img src="./img/github_folder.PNG" style="width:450px;"/>


---

### Basic CI pipeline
```yaml
name: CI Pipeline

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build-ci-python-3_9:
    uses: settler-hackathon/github-templates/.github/workflows/python-ci.yml@main
    with:
      python-version: "3.9"

  build-ci-python-3_10:
    uses: settler-hackathon/github-templates/.github/workflows/python-ci.yml@main
    with:
      python-version: "3.10"

  build-ci-python-3_11:
    uses: settler-hackathon/github-templates/.github/workflows/python-ci.yml@main
    with:
      python-version: "3.11"

```

---

### Basic CD pipeline
```yaml
name: CD Pipeline

on:
  workflow_run:
    workflows: ["CI Pipeline"]
    types:
      - completed
env:
  PYTHON_DEPLOYMENT_VERSION: 3.11
  ENV: prod


jobs:
  test-python-3_9:
    permissions:
      contents: read
      packages: write
    uses: settler-hackathon/github-templates/.github/workflows/python-cd.yml@main
    with:
      python-version: "3.9"
      ENV: $ENV
      REPO-NAME: "github-actions"


  test-python-3_10:
    permissions:
      contents: read
      packages: write
    uses: settler-hackathon/github-templates/.github/workflows/python-cd.yml@main
    with:
      python-version: "3.10"
      ENV: $ENV
      REPO-NAME: "github-actions"

  test-python-3_11:
    permissions:
      contents: read
      packages: write
    uses: settler-hackathon/github-templates/.github/workflows/python-cd.yml@main
    with:
      python-version: "3.11"
      ENV: $ENV
      REPO-NAME: "github-actions"


  publish-image-python-3_11:
    needs: ["test-python-3_9","test-python-3_10","test-python-3_11"]
    permissions:
      contents: read
      packages: write
    uses: settler-hackathon/github-templates/.github/workflows/python-push-image.yml@main
    with:
      ENV: $ENV
      REPO-NAME: "github-actions"
      PYTHON_DEPLOYMENT_VERSION: "3.11"
```

---

### Simple Demo

Pray to the demo gods!

---

