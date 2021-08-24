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

## Examples

Simple usage with source and target repo would just copy the source to the target by downloading each architecture as one image, rebuild manifest list and push it to target server:
```
pma2docker dobernoeder/helloworld:latest dobernoeder/newplace:1.1.1
```
This command would create the following target tags:
- dobernoeder/newplace:1.1.1     
- dobernoeder/newplace:1.1.1-amd64
- dobernoeder/newplace:1.1.1-arm64
- dobernoeder/newplace:1.1.1-armv7


Download multi-arch image and split it to single images with corresponding suffix and upload them seperately to the target repository. Note that dobernoeder/helloworld has three architectures (amd64, arm64, armv7)
```
pma2docker -s dobernoeder/helloworld:latest dobernoeder/newplace:1.1.1
```
This command would create the following target tags:
- dobernoeder/newplace:1.1.1-amd64
- dobernoeder/newplace:1.1.1-arm64
- dobernoeder/newplace:1.1.1-armv7


To transfer an image from a public repository to an secured target repository, use the `--target-user` and `--target-password` parameter. If just adding `--target-user` you will get prompted for login password during the process (Not recommended for build pipelines). 
```
pma2docker --target-user johndoe --target-password mypassword dobernoeder/helloworld:latest dobernoeder/newplace:1.1.1
```

You can also use `--source-user` and `--source-password` to specify credentials for your source repository.
You do not need to specify any credentials if your local docker instance is already configured to use the repositories.


You maybe want to download a multiarch image only and split it into its single images and push only one architecture by hand to a repository. So you can prepare the single images in your local repository:
```
pma2docker --pull-split dobernoeder/helloworld:latest
```
Afterwards you do have all single images in your local docker instance. Use *docker image ls* to list them and *docker push* to push the single image to your target repository by hand. 


If you would like to include only specific architectures in your target multi-arch to save space on your target server, you might want to use this command:
```
pma2docker --only-arch "amd64,armv7"
```
You can choose out of these list to combine several architectures. You just need to seperate them by a ","
`linux/amd64,inux/arm/v5,linux/arm/v7,linux/arm64/v8,linux/386,linux/mips64le,linux/ppc64le,linux/riscv64,linux/s390x,amd64,armv5,armv7,arm64,i386,mips64le,ppc64le,riscv64,s390x`
This is the official supported list of architectures oriented at the docker tags most images use. You might do have other architecture types in your image manifest lists which should also work as long as you specify them correct on the `--only-arch` parameter.

If you want to store the multiarch image or single images of it to your disk, you would need the following call
```
pma2docker --only-arch "armv7,arm64" --only-single-arch --extract /home/myuser/imagedir dobernoeder/helloworld:latest
```
This stores two files as \*.tar.gz in the folder `/home/myuser/imagedir/, one for each architecture. The name cannot be specified by command line and is *image-$arch.tar.gz*, for example image-amd64.tar.gz for x86_64 architecture.
**Note**: The export of multi architecture images to a file is currently not supported by this tool. The parameter --only-single-arch gets activated automatically when you set the --extract option.
