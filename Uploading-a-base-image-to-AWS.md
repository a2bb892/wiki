The steps on this page can now be accomplished by using the `image/image-to-aws.sh` script found in the gateway repository.

The automated build uses AWS to grab the base image zipfile. Construct the base image by following this [page](https://github.com/mozilla-iot/wiki/wiki/Creating-the-base-image-file-for-the-Raspberry-Pi).

Once you have the zip file, create a sha256sum of it:
```
sha256sum gateway-0.2.2.img.zip > gateway-0.2.2.img.zip.sha256sum
```
And upload both the zipfile and the sha256sum file to AWS using a command like:
```
aws s3api put-object --profile wot --bucket mozillagatewayimages --key base/gateway-0.2.2.img.zip.sha256sum --body gateway-0.2.2.img.zip.sha256sum --acl public_read
aws s3api put-object --profile wot --bucket mozillagatewayimages --key base/gateway-0.2.2.img.zip --body gateway-0.2.2.img.zip --acl public_read
```
You can also use the aws cp command:
```
aws s3 cp --acl public-read gateway-0.2.2.img.zip.sha256sum s3://mozillagatewayimages/base/
aws s3 cp --acl public-read gateway-0.2.2.img.zip s3://mozillagatewayimages/base/
```

You can see which base images exist by using a command like:
```
aws s3 ls s3://mozillagatewayimages/base/
```