# Test python docker test

This is a simple template for a docker-based task on the Extrahorizon platform. It just prints "Hello world!" and exits :)


## Gaining access to the docker registry

When ready to try, Extrahorizon will provide a token file, which you can use to authenticate against the docker repository by doing as follows in a shell:

```
cat <token-file> | docker login --username AWS --password-stdin 585697069250.dkr.ecr.eu-central-1.amazonaws.com/exh-sandbox
```

This is, of course, assuming some kind of unix shell (linux, MacOS, ...). You might need to tweak it a bit for Windows.
The idea is that the token file is fed to the `docker login` process through stdin.
If all goes well, you should see `Login Succeeded` being shown.

## Building the docker image

You can build the example docker image as follows:

```
docker build -t 585697069250.dkr.ecr.eu-central-1.amazonaws.com/exh-sandbox:<your-tag> .
```

Please replace `<your-tag>` by whatever tag you choose to give the image. This tag is important because later on we need to tell the task which docker image to use
and having a friendly name is definitely easier than a long hash :)

## Sending the docker image to the register

If the registry is authenticated, you just need to:

```
docker push 585697069250.dkr.ecr.eu-central-1.amazonaws.com/exh-sandbox:<your-tag>
```

Remember to replace `<your-tag>` by whatever tag you chose in the build process


## Running the task

Please notify Extrahorizon when a docker image is uploaded. For now we need to manually manage the task to ensure it is using the
correct image. Later on, we will integrate this functionality in our API so that this manual step is no longer necessary.

## Important

When using a docker-based task, do **not** use the `exh-cli` to manage your task (create/update). The result of doing so is uncertain.
