---
layout: post
title: "Integration of Playwright End-to-End Tests with GitLab CI/CD and AWS Amplify"
date: 2023-12-01 20:34:50 +0000
slug: integration-of-playwright-end-to-end-tests-with-gitlab-cicd-and-aws-amplify
tags: [unit-testing, e2etesting, quality-assurance, gitlab-ci, playwright, awsamplify, end-to-end-test]
lang: en
---


![Cover Image]({{ '/assets/images/a8c35628-c69d-4431-8c74-a2c142190696.png' | relative_url }})

We deploy our Next.js project using AWS Amplify and have successfully managed to run unit tests by configuring the Amplify build through `amplify.yml`. However, for our project's end-to-end testing needs, we preferred the Playwright framework. Playwright suits our purposes better, but unfortunately, Amplify does not directly support Playwright’s end-to-end tests.

**Encountered Challenges and Alternative Solutions**

Initially, my attempts to run Playwright tests on Amplify were unsuccessful. Subsequently, I explored using Vercel, integrating it with GitLab CI/CD. This, however, did not yield the expected results, and we faced several pipeline challenges. Additionally, Vercel's pricing model was not compatible with our budget. My experiences and the challenges encountered during this phase were shared in this discussion: [GitHub Discussion](https://github.com/orgs/vercel/discussions/4780).

**Resolving with GitLab CI/CD**

Ultimately, we opted for GitLab CI/CD as an alternate approach to execute end-to-end tests. Our DevOps team configured a machine on AWS and integrated it with a GitLab runner. We successfully executed the tests by utilizing the .gitlab-ci.yml file that we had configured.

**Automated Deployment and Webhook Integration**

In our workflow, each time we push a commit, Amplify typically kicks off an automatic redeployment process. However, to integrate our end-to-end tests effectively, we modified this setup for both our staging and production environments. We turned off the automatic deployment feature to first run and validate our end-to-end tests. For managing deployments post-testing, we set up two webhooks in Amplify, one for each environment. Once the end-to-end tests pass, we manually trigger the deployment using curl commands that interact with these webhooks, allowing us to deploy to the respective environments only after ensuring everything is in order.

**GitLab CI/CD Configuration**

We configured the end-to-end tests to run only in the staging and production environments. For a dynamic setup, we assigned different base URLs based on branches. Additionally, we utilized Amplify webhook URLs, setting them in GitLab's environment variables.

```typescript
default:
  image: node:18

stages:
  - test
  - trigger_deployment

cache:
  key:
    files:
      - package-lock.json
      - package.json
  paths:
    - node_modules/

test_e2e:
  stage: test
  image: mcr.microsoft.com/playwright:v1.39.0-jammy
  script:
    - npm ci
    - npm run test:e2e
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
      variables:
        APP_BASE_URL: "https://example.com/"
    - if: '$CI_COMMIT_BRANCH == "staging"'
      variables:
        APP_BASE_URL: "https://staging.example.com/"


trigger_deployment:
  stage: trigger_deployment
  script:
    - |
      if [ "$CI_COMMIT_BRANCH" = "main" ]; then
        curl -X POST -d {} $AMPLIFY_PRODUCTION_WEBHOOK_URL -H "Content-Type:application/json"
      fi
    - |
      if [ "$CI_COMMIT_BRANCH" = "staging" ]; then
        curl -X POST -d {} $AMPLIFY_STAGING_WEBHOOK_URL -H "Content-Type:application/json"
      fi
  rules:
    - if: '$CI_COMMIT_BRANCH == "staging" || $CI_COMMIT_BRANCH == "main"'
  when: on_success
```
