import { Aside } from "~/components/configurable/Aside";

<Title>Deploying to AWS via Flightcontrol</Title>

Here, we're going to use [Flightcontrol](https://www.flightcontrol.dev/?ref=solid) to deploy our Solid project.

If you haven't heard of Flightcontrol, it's a platform that fully automates deployments to Amazon Web Services (AWS). For more information on Flightcontrol and what they offer, visit [flightcontrol.dev](https://www.flightcontrol.dev/?ref=solid).

## Connecting Flightcontrol with Your Online Git Repository

With Flightcontrol, you can make use of GitHub's continuous development actions. When connecting Flightcontrol to your GitHub repo, there's no further configuration that needs to be done.

Flightcontrol automatically detects pushes to the branch specified and builds the project based on the commands specified in the `package.json` and the settings in the Flightcontrol build settings.

Here's a quick step by step guide to get your Solid online repo code up and running on Flightcontrol.

**Step 1:** Log into or sign up on Flightcontrol.

**Step 2:** Connect your Github account to Flightcontrol.

<img src="/images/how-to-guides/deployment/flightcontrol-connect-github.png" alt="Image of Connect Github page"/>

<Aside>
  For more information on how to connect your github account, visit{" "}
  <Link
    href="https://www.flightcontrol.dev/docs/getting-started/connecting-github?ref=solid"
    target="_blank"
  >
    Connecting GitHub Accounts to Flightcontrol
  </Link>
</Aside>

## Option 1: Using Dashboard

**Step 1:** Create a Flightcontrol project from the Dashboard. Select a repository for the source.

**Step 2:** Select the GUI Config Type.

**Step 3:** Add a static site service by clicking the `Add a Static Site` option.

<img src="/images/how-to-guides/deployment/flightcontrol-services.png" alt="Image of the Flightcontrol sevices"/>

**Step 4:** Add an output directory, `dist`.

**Step 5:** Add any environment variables your project might need.

<img src="/images/how-to-guides/deployment/flightcontrol-static-website.png" alt="Image of the Flightcontrol Static Website"/>

**Step 6:** Link your AWS Account.

<img src="/images/how-to-guides/deployment/Flightcontrol-link-AWS.png" alt="Image of how to link AWS account on Flightcontrol"/>

**Step 7:** Submit the new project form.

## Option 2: Using Code

Use the flightcontrol.json configuration for the project

**Step 1:** Create a Flightcontrol project from your dashboard. Select a repository for the source.

**Step 2:** Select the flightcontrol.json Config Type.

 <img src="/images/how-to-guides/deployment/flightcontrol-config-option.png" alt="Image of config options on Flightcontrol"/>

**Step 3:** Add a new file at the root of your repository called `flightcontrol.json`. Here's an example configuration that creates a static site service for your Solid app:

```json filename="flightcontrol.json"
{
  "$schema": "https://app.flightcontrol.dev/schema.json",
  "environments": [
    {
      "id": "production",
      "name": "Production",
      "region": "us-west-2",
      "source": {
        "branch": "main"
      },
      "services": [
        {
          "id": "my-static-solid",
          "buildType": "nixpacks",
          "name": "My static solid site",
          "type": "static",
          "domain": "solid.yourapp.com",
          "outputDirectory": "dist",
          "singlePageApp": true
        }
      ]
    }
  ]
}
```

<Aside>
  For more information, build and deployment instructions, and features visit
  the{" "}
  <Link href="https://www.flightcontrol.dev/docs?ref=solid" target="_blank">
    Flightcontrol documentation
  </Link>
</Aside>
