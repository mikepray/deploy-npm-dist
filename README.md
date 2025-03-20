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
  workflow_dispatch:  # Allows manual triggering from GitHub UI

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3
        
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '20'
          cache: 'npm'
      
      - name: Deploy to production server
        uses: mike-pray/static-site-deploy-action@v1  # Replace with your username/repo
        with:
          host: ${{ secrets.PROD_HOST }}
          username: ${{ secrets.SSH_USERNAME }}
          ssh-key: ${{ secrets.SSH_PRIVATE_KEY }}
          source-directory: 'dist'
          destination-directory: '/var/www/html'
          # Optional parameters with their defaults
          # build-command: 'npm run build'
          # skip-build: 'false'
          # clean-destination: 'true'
          # ssh-options: '-o StrictHostKeyChecking=accept-new'
      
```

## Considerations

I wrote this action because nothing else seemed to do the trick. I use this to deploy an Astro static site to a Digital Ocean droplet. There are many other actions which use Digital Ocean or Astro-specific actions, but none of them were as simple as just using ssh. 
