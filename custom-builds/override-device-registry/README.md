# Override image with image stream

This is an example on how to use images from image streams, instead of
directly pulling from quay.io or Docker Hub. It also works with OpenShift
builds, as long as you can build the image in the same way on the cluster.

The following example will replace the `iot-device-registry-infinispan` image.
It uses ImageStreams and a build with the S2I Java image.

## Deploy 

Create the ImageStream and S2I build. Be sure point the build to the Git repository
you want to build from:

    oc apply -f .

This creates the image stream for the S2I image and for the image being built
by the `BuildConfig`. Once the build populated the image stream, you can override
the image.

**Note:** The image stream is configured to "hijack" local, unqualified image names
          and replace them with the internal registry image.

## Image trigger

You can set an image trigger for the deployment/stateful set as well:

    oc set triggers deployment/iot-device-registry --from-image=iot-device-config-infinispan:latest --container device-registry

**Note:** This only works if the annotations of the deployment do not get overwritten
          by any operator.

## Override

Afterwards you need to override the image name of the container you want to
replace, with an unqualified image name, e.g. `iot-device-registry-infinispan:latest`
instead of `quay.io/ctron/iot-device-registry-infinispan:latest`.

There are different ways, with different pros/cons.

For deployments not deployed by any operator, like the `enmasse-operator`, you must directly
modify the deployment.

### Override using `imageOverrides`

Edit the `IoTConfig` and override the image:

~~~yaml
apiVersion: iot.enmasse.io/v1alpha1
kind: IoTConfig
metadata:
  name: default
spec:
  imageOverrides:
    iot-device-registry-infinispan:
      name: iot-device-registry-infinispan:latest 
~~~

From what I know, this currently works only with the IoTConfig. However it is easy
to override, and remove later on.

### Override using env-vars

The deployment of the `enmasse-operator` has a bunch of environment variables,
which override the built in image map.

It is possible to override the image by setting the appropriate environment variable.

    oc set env dc enmasse-operator IOT_DEVICE_REGISTRY_INFINISPAN_IMAGE=iot-device-registry-infinispan:latest

Theoretically you could remove the override later on by executing:

    oc set env dc enmasse-operator IOT_DEVICE_REGISTRY_INFINISPAN_IMAGE-

However, the default deployment of EnMasse sets all environment variables by default, to
override the internal image map, which sometimes contains wrong values. So clearing
the environment variable isn't a safe approach. You need to record the original value
and set this later to remove the override.

This approach works for all images deployed by the `enmasse-operator`.
