<img src='https://github.com/byangular/images/raw/master/s3-github-action/architecture.png' border='0' alt='architecture' />

Implementing automatic distribution of [AWS](https://aws.amazon.com/ko/) product [S3](https://aws.amazon.com/ko/s3/) through [GitHub Actions](https://github.com/features/actions) operation.

> Create smart aws diagrams [Cloudcraft](https://cloudcraft.co/)

<br />

## What is AWS ?

Whether you're looking for compute power, database storage, content delivery, or other features with services operated by Amazon, 

AWS has services to help you build sophisticated applications with increased flexibility, scalability, and reliability.

## What is S3 ?

Amazon Simple Storage Service (Amazon S3) is an object storage service that offers industry-leading scalability, data availability, security, and performance.

This means customers of all sizes and industries can use it to store and protect any amount of data for a range of use cases, such as websites, mobile applications, backup and restore, archive, enterprise applications, IoT devices, and big data analytics.

▾ Amazon S3 works

<img src='https://github.com/byangular/images/raw/master/s3-github-action/s3-works.png' border='0' alt='s3-works' />

## What is GitHub Actions ?

GitHub Actions makes it easy to automate all your software workflows, now with world-class CI/CD. Build, test, and deploy your code right from GitHub. 

Make code reviews, branch management, and issue triaging work the way you want.

## Continuous Deployment with Github Actions

### Add a Workflows File to Your Source Repository

Github Actions to build your workflows yml files.

Add a `*.yml` file to your source code repository to tell Github Actions.

[GitHub Actions](https://github.com/features/actions)

▾ build.yml

```bash
name: Angular S3 Build
on:
  push:
    branches:
      - master

jobs:
  build:
    runs-on: ubuntu-18.04
    steps:
      - name: Checkout source code
        uses: actions/checkout@master

      - name: Cache node modules
        uses: actions/cache@v1
        with:
          path: node_modules
          key: ${{ runner.OS }}-build-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.OS }}-build-
            ${{ runner.OS }}-

      - name: Install
        run: npm install

      - name: Lint
        run: npm run lint

      - name: Test
        run: npm run test

      - name: Build
        run: npm run build

      - name: Deploy
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        run: |
          aws s3 cp \
            --recursive \
            --region ap-northeast-2 \
            dist s3://github-action-angular-build-tutorial
```
