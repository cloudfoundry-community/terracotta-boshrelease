# BOSH Release for terracotta

One of the fastest & best ways to get [terracotta](http://terracotta.org/) running on any infrastructure is too deploy this bosh release.

## Usage

To use this bosh release, first upload it to your bosh:

```
bosh target BOSH_URL
bosh login
git clone git@github.com:cloudfoundry-community/terracotta-boshrelease.git
cd terracotta-boshrelease
bosh upload release releases/terracotta-1.yml
```

See [examples](https://github.com/cloudfoundry-community/terracotta-boshrelease/tree/master/examples) folder for example deployment files.

Notes:

* run `bosh status` to get your bosh director UUID
* create a security group `terracotta` with at least port 22 open and the ability for all VMs in the group to talk to each other; for example:

![terracotta security group](https://www.evernote.com/shard/s3/sh/1f3f75fb-26d2-4069-940f-931f5645e40c/76420829f911ea25be39113d35d14077/deep/0/EC2%20Management%20Console.png)

Finally, target and deploy. For deployment to a bosh running on aws:

```
bosh deployment path/to/deployment/file.yml
bosh deploy
```

If you deploy more than one instance in a job it assumes that you want replication, and you need to specify the IP address of the master node in your deployment manifest.

## Development


## Create new final release

To create a new final release you need to get read/write API credentials to the [@cloudfoundry-community](https://github.com/cloudfoundry-community) s3 account.

Please email [Dr Nic Williams](mailto:&#x64;&#x72;&#x6E;&#x69;&#x63;&#x77;&#x69;&#x6C;&#x6C;&#x69;&#x61;&#x6D;&#x73;&#x40;&#x67;&#x6D;&#x61;&#x69;&#x6C;&#x2E;&#x63;&#x6F;&#x6D;) and he will create unique API credentials for you.

Create a `config/private.yml` file with the following contents:

``` yaml
---
blobstore:
  s3:
    access_key_id:     ACCESS
    secret_access_key: PRIVATE
```

You can now create final releases for everyone to enjoy!

```
bosh create release
bosh upload release
# test this dev release

git commit -m "updated terracotta"
bosh create release --final
git commit -m "creating vXYZ release"
git tag vXYZ
git push origin master --tags
```
