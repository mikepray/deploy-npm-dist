# deploy-static-site

This action builds an npm project with `npm install && npm run build` and then uses ssh and scp to upload the result to a remote host's `/var/www/html` directory. This is the default web directory for most web servers like nginx and apache, and the npm commands this runs are the default commands most static sites use which are built with npm.

This requires no other dependent automations.

## Usage

Example usage:

```
name: Deploy Website
on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3
        
      - name: Deploy Static Site to Webserver
        uses: mikepray/deploy-static-site@v1
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SSH_USERNAME }}
          ssh-key: ${{ secrets.SSH_PRIVATE_KEY }}
          source-directory: 'dist'
          destination-directory: ${{ secrets.DEPLOY_PATH || '/var/www/html' }}
          node-version: '20'
          build-command: ${{ secrets.BUILD_COMMAND || 'npm run build:prod' }}
      
```

## Considerations

I wrote this action because nothing else seemed to do the trick. I use this to deploy an Astro static site to a Digital Ocean droplet. There are many other actions which use Digital Ocean or Astro-specific actions, but none of them were as simple as just using ssh. 
