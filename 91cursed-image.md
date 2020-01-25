#!/bin/sh
buildah manifest create cursed-image
buildah manifest add --override-arch=arm64   --override-os=linux --os=linux --arch=arm64 --variant=v8 cursed-image docker://fedora
buildah manifest add --override-arch=amd64   --override-os=linux --os=linux --arch=amd64              cursed-image docker://centos
buildah manifest add --override-arch=ppc64le --override-os=linux --os=linux --arch=ppc64le            cursed-image docker://debian
buildah manifest add --override-arch=s390x   --override-os=linux --os=linux --arch=s390x              cursed-image docker://alpine
buildah manifest push --all cursed-image docker://nalind/cursed-image
