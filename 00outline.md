How single-arch images get built and pushed to registries:
	* Build an image.
	* Push it to a repository in a registry, probably using a tag, or noting its digest.
	* Tell your friends about your image using the registry/repository:tag name.
	* Destroy the local evidence, never speak of this again.
How single-arch images get pulled and run from registries:
	* Client asks registry for the manifest for an image that matches a given digest or tag in a repository.
	* Registry hands back manifest for the image.
	* Client asks for the layers and other things that make up the image.
	* Client builds root filesystem, sets up namespaces, execs the entry point or command.
How multi-arch images get pulled and run from registries:
	* Client asks registry for the manifest for an image that matches a given digest or tag in a repository.
	* Registry hands back a lists of manifests.
	* Client selects an image from the list.
	* Client asks registry for the manifest for the chosen image.
	* Client asks for the layers and other things that make up the image.
	* Client builds root filesystem, sets up namespaces, execs the entry point or command.
How lists of images get built and pushed to registries:
	* Build an image.  Repeat, repeat, repeat.
	* Push images to a repository in a registry, using unique tags, or noting the digests.
	* Build a list of those images, describing the types of systems they're each best for.
	* Push the list to a registry, probably using a tag, or noting its digest.
	* Tell your friends about your "image" using the registry/repository:tag name.
	* Destroy the local evidence, never speak of this again.
Building manifest lists with buildah:
	* buildah manifest create $list
	* buildah manifest add $list $image
	* buildah manifest push $list $registry/$repository:$tag
	* buildah manifest push --all $list $registry/$repository:$tag
	* buildah rmi $list
Using buildah manifest:
	* add takes arguments
	* our goal is to keep you from needing to use them
Building images for different architectures:
	* Build on actual hardware, push to registry, for each arch.  Build list, push list.
		* Add images using their digests or arch-specific tags.
		* The fastest (or second fastest) option.
	* Build in a VM, push to registry, for each arch.  Build list, push list.
		* Cross-arch VMs exist, are kind of slow.
		* The slowest option.
	* Cross-compile on build host for runtime arch, install onto a suitable base image, push along with list.
		* Be sure to mark each image with the right architecture!
		* The second fastest (or fastest) option.
	* Build for runtime, emulate for RUN (qemu-user-static, more like qemu-user-magic, amirite?), push along with list.
		* Faster than a cross-arch VM, probably slower than actual hardware.
