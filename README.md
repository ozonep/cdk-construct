# @sls-next/cdk-construct

How to deploy Next.js to AWS via CDK v2 (currently in RC state):

stack.ts file:

```ts
import { Stack, StackProps } from "aws-cdk-lib";
import { Construct } from 'constructs';
import { NextJSLambdaEdge } from "@sls-next/cdk-construct";

export class MyStack extends Stack {
  constructor(scope: Construct, id: string, props: StackProps) {
    super(scope, id, props);
    new NextJSLambdaEdge(this, "NextJsApp", {
      serverlessBuildOutDir: "./build"
    });
  }
}

```

bin.ts file:

```ts
import { App } from "aws-cdk-lib";
import { Builder } from "@sls-next/lambda-at-edge";
import { MyStack } from "./stack";

// Run the serverless builder for Next.js app, this could be done elsewhere in your workflow
const builder = new Builder(".", "./build", { args: ["build"] });

builder
  .build()
  .then(() => {
    const app = new App();
    new MyStack(app, `MyStack`);
  })
  .catch((e) => {
    console.log(e);
    process.exit(1);
  });```

```
