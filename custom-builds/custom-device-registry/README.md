# Custom device registry development

This is an example how to deploy a custom protocol device registry, for developing with S2I.
Please ensure to update your settings:

    oc apply -f .
    SECRET=$(oc -n enmasse-infra get iotconfig default  -o 'jsonpath={.status.authenticationServicePSK}')
    oc set env dc/iot-custom-device-registry "HONO_AUTH_VALIDATION_SHARED_SECRET=$SECRET" "HONO_REGISTRY_SVC_SIGNING_SHARED_SECRET=$SECRET"
