# EnMasse – source-to-image

Enabling "source-to-image" for an existing EnMasse deployment.

## Enable

You will need to have EnMasse deployed already. Then run:

    ./enable <project-name>

This will create the *Image Streams* and *Build Configs* and configure
the deployments to make use of those.

## Disable

You can switch by by executing:

    ./disable <project-name>
