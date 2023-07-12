# Backstage Helm Chart

A forked backstage helm chart from https://github.com/backstage/charts/tree/main/charts/backstage.

Rather than adding the `app-config`` values as a config-map, this chart will deploy it as a secret resource and mount it to the backstage pod.

This is so we can manage the app-config from code instead of having it baked into the Dockerfile.