# Test python docker test

This is a simple template for a docker-based task on the Extrahorizon platform. It just prints "Hello world!" and exits :)

However, there are a few steps that need to be done in order to achieve this result:

1. Build your docker image
2. Authenticate yourself to the registry
3. Push the docker image to the registry
4. Notify Extrahorizon

Before starting, Extrahorizon will need to provide you with the following:
* Docker registry URL (`registry-url`) where you can push your docker images
* Username & password to authenticate yourself to the registry.

## 🛠️ Building the docker image

It is important that the [docker file](./Dockerfile) base image is a AWS lambda-compatible runtime, such as

```Dockerfile
FROM public.ecr.aws/lambda/python:3.9
```

You can build the example docker image as follows:

```
docker build -t <registry-url>:<your-tag> .
```

Where `registry-url` is the url provided by Extrahorizon.

Please replace `<your-tag>` by whatever tag you choose to give the image. This tag is important because later on we need to tell the task which docker image to use
and having a friendly name is definitely easier than a long hash :)

## 🔐 Authenticate yourself to the registry

You need to authenticate yourself to the docker registry in order to push images to it. Normally you should only do this once or when your credentials expire. For this, you will need the AWS client `aws`. See [here](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) for instructions on how to install this client.


Once `aws` is set up correctly, use it to authenticate against the docker registry. This is explained [here](https://docs.aws.amazon.com/AmazonECR/latest/userguide/registry_auth.html), but the easiest is probably to do

```
aws ecr get-login-password --region region | docker login --username AWS --password-stdin <registry-url>
```

## 🚚 Sending the docker image to the register

If the registry is authenticated, you need to:

```
docker push <registry-url>:<your-tag>
```

Remember to replace `<your-tag>` by whatever tag you chose in the build process

## ⚙️ Running the task

Please notify Extrahorizon when a docker image is uploaded. For now we need to manually manage the task to ensure it is using the
correct image. Later on, we will integrate this functionality in our API so that this manual step is no longer necessary.

## ⚠️ Important

When using a docker-based task, do **not** use the `exh-cli` to manage your task (create/update). The result of doing so is uncertain.
