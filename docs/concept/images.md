# Images

Images are Beaker's unit of executable code. A Beaker image combines a Docker image with metadata,
such as its author and description, and an optional richer narrative in markdown. Please refer to
[the wordcount example](https://beaker.org/im/im_qbjvcda1sed7) for an overview.

Like datasets, Beaker images are immutable. The following example shows how to create and use images.

## Preparation: Build a Docker Image

This example refers to the [wordcount](../../examples/wordcount) example, but you can use any Docker
image at your disposal. To build it, copy the example's files and build them with the following
command

```bash
docker build -t wordcount <path/to/wordcount/directory>
```

## Create and Upload

You can create and upload any Docker image as a Beaker image with a single command. Behind the scenes, Beaker pushes your
Docker image to a private repository. This guarantees that the image will remain available and
unchanged for future experiments.

```
▶ beaker image create --name wordcount wordcount
Pushing wordcount as wordcount (im_qbjvcda1sed7)...
The push refers to repository [gcr.io/ai2-beaker-core/public/bduufrl06q5ner2l0440]
172b7a93847f: Preparing
bca0cc28f8e3: Preparing
3a1dff9afffd: Preparing
3a1dff9afffd: Layer already exists
bca0cc28f8e3: Pushed
172b7a93847f: Pushed
latest: digest: sha256:4c70545c15cca8d30b3adfd004a708fcdec910f162fa825861fe138200f80e19 size: 940
Done.
```

Notice that each image is assigned a unique ID in addition to the name we chose. Any object such as
an image, can be referred to by its name or ID. IDs are guaranteed to remain stable while names can
change.

```bash
beaker image get examples/wordcount
beaker image get im_qbjvcda1sed7
```

## Inspect

An image's metadata can be retrieved with `beaker image get`.

```
▶ beaker image get examples/wordcount
ID                  WORKSPACE          AUTHOR    CREATED
examples/wordcount  examples/examples  examples  2018-08-22 15:47:10
```

A more detailed JSON view is available via `--format=json`
```
▶ beaker image get --format=json examples/wordcount
[
    {
        "id": "im_qbjvcda1sed7",
        "name": "wordcount",
        "fullName": "examples/wordcount",
        "owner": {
            "id": "us_gpx6zozipf5o",
            "name": "examples",
            "displayName": "Beaker Examples"
        },
        "author": {
            "id": "us_gpx6zozipf5o",
            "name": "examples",
            "displayName": "Beaker Examples"
        },
        "workspaceRef": {
            "id": "01DQQZ5SFZ9KQXW59PGC7DK7C4",
            "name": "examples"
        },
        "created": "2018-08-22T22:47:10.915236Z",
        "committed": "2018-08-22T22:47:23.422262Z",
        "originalTag": "wordcount",
        "description": "A simple example to count words in text."
    }
]```

## Download

You can pull an image to your local machine at any time with `beaker image pull`.

```
▶ beaker image pull examples/wordcount
Pulling gcr.io/ai2-beaker-core/public/bduufrl06q5ner2l0440 ...
latest: Pulling from ai2-beaker-core/public/bduufrl06q5ner2l0440
...
Digest: sha256:4c70545c15cca8d30b3adfd004a708fcdec910f162fa825861fe138200f80e19
Status: Downloaded newer image for gcr.io/ai2-beaker-core/public/bduufrl06q5ner2l0440:latest
Done.
```

In order to avoid accidentally overwriting your local images, this command assigns Beaker's random
internally assigned tag `gcr.io/ai2-beaker-core/public/bduufrl06q5ner2l0440` by default. To assign
a more human-friendly tag, set it with an additional argument:

```
▶ beaker image pull examples/wordcount friendly-name
```

## Clean Up

To remove Docker images from your system, use `docker image rm` on any of the images created above.
