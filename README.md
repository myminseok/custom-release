## build build sourcecode skelleton

https://bosh.io/docs/create-release/


#### ssh into opsmanager

```
mkdir custom-release

cd custom-release

bosh init-release

drwxr-xr-x@ 4 kminseok  staff   128B  1월 19 14:12 config
drwxr-xr-x@ 2 kminseok  staff    64B  1월 19 14:12 jobs
drwxr-xr-x@ 2 kminseok  staff    64B  1월 19 14:12 packages
drwxr-xr-x@ 2 kminseok  staff    64B  1월 19 14:12 src
```

#### develop job 
```
bosh generate-job  custom-pre-start-script
```
and create files.
```
./custom-pre-start-script
./custom-pre-start-script/spec
./custom-pre-start-script/monit
./custom-pre-start-script/templates
./custom-pre-start-script/templates/pre-start.sh
```

#### build and test

```
cd custom-release

bosh reset-release

bosh create-release --force

bosh upload-release

bosh releases | grep custom

bosh update-runtime-config --name=custom-release-addon ./runtimeconfig.yml

bosh config --type=runtime --name=custom-release-addon

bosh -d cf-6e18f0c63108927910d4 manifest > manifest.yml

bosh -d cf-6e18f0c63108927910d4 deploy manifest.yml
```


#### build relase tar file





## add config/final.yml

```
cd custom-release

cat > config/final.yml<<EOF
---
blobstore:
  provider: local
  options:
    blobstore_path: .local_blob_tmp
final_name: custom-release
EOF

```
bosh reset-release

export VERSION=1.0.0

bosh create-release --name=custom-release --final --version=$VERSION

bosh create-release dev_releases/custom-release/custom-release-$VERSION.yml --tarball ./custom-release-$VERSION.tgz

bosh upload-release ./custom-release-$VERSION.tgz

bosh update-runtime-config --name=custom-release-addon ./runtimeconfig.yml

bosh config --type=runtime --name=custom-release-addon

bosh -d TARGET_DEPLOYMENT manifest > manifest.yml

bosh -d TARGET_DEPLOYMENT deploy manifest.yml




### Reference
- https://bosh.io/docs/create-release/
- https://github.com/cloudfoundry/os-conf-release
- https://github.com/rakutentech/bosh-routing-release
