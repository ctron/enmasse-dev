# Custom protocol adapter development

This is an example how to deploy a custom protocol adapter, for developing with S2I. It is based on the
`iot-sigfox-adapter`, so please ensure to update your settings:

    oc apply -f .
    PASSWORD=$(oc -n enmasse-infra get iotconfig default -o 'jsonpath={.status.adapters.http.interServicePassword}')
    oc set env dc/iot-sigfox-adapter "HONO_CREDENTIALS_PASSWORD=$PASSWORD" "HONO_DEVICE_CONNECTION_PASSWORD=$PASSWORD" "HONO_REGISTRATION_PASSWORD=$PASSWORD" "HONO_TENANT_PASSWORD=$PASSWORD"
