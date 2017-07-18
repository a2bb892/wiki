If you do not already have access to the AWS console ask on #iot.

  - mozillagatewaylogs : This bucket stores the logs for other buckets. This bucket should never be accessble to the general public.
 - mozillagateway-dev : This bucket stores the `update.json` and individual nightly releases for the a/b updater system. This bucket should be readable by the public.