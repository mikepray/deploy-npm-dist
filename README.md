# deploy-static-site

This action builds an npm project with `npm install && npm run build` and then uses ssh and scp to upload the result to a remote host's `/var/www/html` directory. This is the default web directory for most web servers like nginx and apache, and the npm commands this runs are the default commands most static sites use which are built with npm.

This requires no other dependent automations.

This requires several secrets to be configured in your repository:

- `SSH_PRIVATE_KEY`: The `ed25519` encrypted private key of the user allowed to upload the server
- `SSH_USERNAME`: The username of the user allowed to uplaod to the server
- `HOST`: The hostname of the remote host

## Considerations

I wrote this action because nothing else seemed to do the trick. I use this to deploy an Astro static site to a Digital Ocean droplet. There are many other actions which use Digital Ocean or Astro-specific actions, but none of them were as simple as just using ssh. 
