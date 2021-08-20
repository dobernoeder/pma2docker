# pma2docker - Push MultiArch 2 Docker
This is a script to copy multi architecture docker images from one repository to another or to a new tag without loosing platforms. This works with docker.io and other repositories.

## The manual process

### Normal pull-push of images
When you see a multi architecture image on docker hub you would specify it in the pull command. Normally the image is being pulled for the platform you are running the *docker pull* from. This is time and space efficient because you would not need images for other architectures most of the time.

In case you want to transfer one image from your private registry to a public one such as docker hub, you would need to pull every image for each plattform from your source registry to have them all locally on your system. Then you would like to push all of them to the target registry. When you try, you won't find a --platform parameter on *docker push* sadly. Unfortunately you do have three or more different images pulled which maybe have been overwritten on your local machine with each pull. You do no longer have a multi architectural image.

### Create Manifests
You may want to create several images for different architectures and want to make a multi arch single image. How this is working, please consider reading [this article](https://www.docker.com/blog/multi-arch-build-and-images-the-simple-way/).
The *docker manifest* command helps us to create a multi arch image out of several single architecture images. So this would help to re-create the image when we downloaded a single image for each platform.

### Using Digest with pull
The problem of overwriting the images for different architectures on the local disk can be solved by specifying a digest. Every image does have a digest you can find with *docker manifest inspect* on a multi arch image. You just need to pull the image for each platform by this digest, at least for referencing them for the pull from the repository. Every Image would need a different name to reference for building the manifest. So you can see which image belongs to which architecture on your local disk.

### Puzzling the multi arch image
Creating the manifest with *docker manifest create* command to have a multiarch image again is quite easy as described in the above hyperlink. From now on you have a multi arch image that can be pushed to every repository.


## How this script works
It automates the steps described above, no big deal with that.

1. Check if Source Image exists
2. Add 'latest' tag if not specified as parameter
3. Read manifest from source repository
4. Pull image for all architectures and renaming them
5. Push single architecture image to your target repo
6. Create new multi-arch manifest on your local disk
7. Combining platform architectures to one multi-arch manifest
8. Pushing the multi-arch manifest to the target repository

Thats it! No magic, just code!

## Things to keep in mind
- The command *docker manifest* is still experimental, you may be careful using it and this script
- You need to enable experimental features to use *docker manifest* command, just read the [official documentation](https://docs.docker.com/engine/reference/commandline/manifest/)