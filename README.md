# Codex 문서

Codex 문서에 오신 것을 환영합니다.

Codex는 매우 강력한 검열 저항성과 내구성 보장을 제공하는 분산형 데이터 저장 플랫폼입니다.


## Run and build

 For documentation we use [VitePress](https://vitepress.dev/), which is [Vue](https://vuejs.org/)-powered static site generator built on top of [Vite](https://vitejs.dev/).

 To run site locally, we should clone repository, install dependencies and run the following command
 ```shell
 npm run docs:dev
 ```

 <details>
 <summary>Detailed guide</summary>

 1. [Install](https://nodejs.org/en/download/package-manager) node 20 or [above](https://nodejs.org/en/about/previous-releases)

    Using [nvm](https://github.com/nvm-sh/nvm)
    ```shell
    # nvm
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash

    # Node 22
    nvm install 22
    nvm use 22

    # Check
    node --version
    v22.6.0
    ```

 2. Clone repository
    ```shell
    git clone https://github.com/codex-storage/codex-docs
    cd codex-docs
    ```

 3. Install dependencies
    ```shell
    npm install
    ```

 4. Start a local dev server with instant hot updates
    ```shell
    # Local
    npm run docs:dev

    # Expose
    npm run docs:dev -- --host
    ```

 5. [Build the site](https://vitepress.dev/guide/deploy)
    ```shell
    npm run docs:build

    # .vitepress/dist
    ```
 </details>


## Contribute

 Please check [VitePress documentation](https://vitepress.dev/) for more details about customizations.

 Process
 - Create a fork
 - Create a custom branch in your fork
 - Add your contribution
 - Make a PR to the upstream repository

 Project structure
 - `learn` - Codex에 대해 배울 수 있는 모든 정보
 - `networks` - Codex 네트워크 관련 정보
 - `developers` - Codex 개발 프로세스 및 가이드
