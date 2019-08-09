# Scripts

Some helper scripts.

## uninstall

You can uninstall EnMasse with the `uninstall` script. By default it will uninstall EnMasse
from the `enmasse-infra` project. However you can change that by passing in the
environment variable `PROJECT`:

    ./uninstall

Or:

    PROJECT=foo-bar ./uninstall

The script will pause briefly before doing the actual work, at which point you can
abort with Ctrl-C.

The script will **not** delete the project/namespace, but will also delete cluster wide
resources, like the CRDs. 
