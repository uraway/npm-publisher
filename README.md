# npm-publisher Orb

An orb for NPM package authors.

## Personal Access Token

To publish your package, you need to create Authentication Token. More detail [here](https://docs.npmjs.com/creating-and-viewing-authentication-tokens).

After creation of Authentication Token, set as environment variable "NPM_TOKEN".

## Usage

Full usage example: [Orb registry page](https://circleci.com/orbs/registry/orb/uraway/npm-publisher)

```yaml
orbs:
  npm-publisher: uraway/npm-publisher@x.y.z

version: 2.1
workflows:
  build_publish:
    jobs:
      - npm-publisher/publish-from-package-version:
          publish-token-variable: NPM_TOKEN
          push-git-tag: true
          fingerprints: <fingerprints>
          pre-publish-steps:
            - restore_cache:
                keys:
                  - v1-node-cache-{{ .Branch }}-{{ checksum "package-lock.json" }}
                  - v1-node-cache-{{ .Branch }}
                  - v1-node-cache-
            - run: npm install
            - run: npm build
          post-publish-steps:
            - save_cache:
                key: v1-node-cache-{{ .Branch }}-{{ checksum "package-lock.json" }}
                paths:
                  - node_modules
          filters:
            branches:
              only: master
```
