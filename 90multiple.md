#!/bin/bash
base=fedora

cat > Dockerfile <<- EOF
	FROM $base
	RUN dnf -y install iputils && dnf clean all
EOF

declare -A image
for arch in amd64 arm64 ppc64 s390x ; do
	buildah build-using-dockerfile --override-arch $arch --iidfile iid .
	image[$arch]=$(cat iid)
	rm -f iid
done

rm -f Dockerfile

buildah manifest create ${base}-with-iputils
for arch in ${!image[@]} ; do
	flags=
	case $arch in
	arm64|aarch64)
		flags="--arch arm64 --variant v8"
	esac
	buildah manifest add ${base}-with-iputils $flags ${image[$arch]}
done

buildah manifest push --all ${base}-with-iputils docker://nalind/${base}-with-iputils
