# Datasets

Datasets are immutable collections of one or more files.  Datasets can be mounted into experiments
for data re-use and to clearly show what information was needed to run an experiment.
The following example shows how to create and use datasets.

## Preparation: Create Some Files

First create a working directory to contain our example.

```bash
mkdir -p ~/dataset-example
cd ~/dataset-example
```

Next we'll create some example files to upload as datasets.

```bash
mkdir -p multifile/nested
echo 'First file' > multifile/first
echo 'Second file' > multifile/second
echo 'Nested file' > multifile/nested/file
echo 'This is a single file.' > singlefile
```

Verify the file contents. The following command finds all files in the current
directory and prints each file's path and contents.

```
▶ find . -type f -exec echo {} \; -exec cat {} \; -exec echo \;
./multifile/first
First file

./multifile/second
Second file

./multifile/nested/file
Nested file

./singlefile
This is a single file.
```

## Upload

You can upload any file or collection of files as a dataset with a single
command `beaker dataset create`. Beaker treats all datasets as a directory, even
when uploading a single file. In this special case the dataset contains a single
file with the same name as the original file.

```
▶ beaker dataset create --name my-dataset ./some-path
Uploading my-dataset (ds_as6t74lspoc5)...
Done.
```

Notice that each dataset is assigned a unique ID in addition to the name we chose. Any object such
as a dataset can be referred to by its name or ID. IDs are guaranteed to remain stable while names
can change.

```bash
beaker dataset get examples/moby
beaker dataset get ds_1hz9k6sgxi0a
```

## Inspect

A dataset can be inspected with `beaker dataset get`.

```
▶ beaker dataset get examples/moby
ID             WORKSPACE          AUTHOR    CREATED              SOURCE EXECUTION
examples/moby  examples/examples  examples  2018-08-22 15:35:55  N/A
```

A more detailed JSON view is available via `--format=json`

```
▶ beaker dataset get --format=json examples/moby
[
    {
        "id": "ds_1hz9k6sgxi0a",
        "name": "moby",
        "fullName": "examples/moby",
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
        "created": "2018-08-22T22:35:55.223927Z",
        "committed": "2018-08-22T22:35:56.121653Z",
        "description": "Full text of Moby Dick by Herman Melville from https://www.gutenberg.org/files/2701/old/moby10b.txt",
        "storage": {
            ...
        }
    }
]
```

## Download

You can download a dataset to your local drive at any time with `beaker dataset fetch`. Beaker's
`fetch` command follows the same rules as the standard `cp` command. The following example downloads
the single-file dataset to an empty directory. Notice how the original filename is restored by default.

```
▶ beaker dataset fetch examples/moby -o moby
Downloading examples/moby to moby
Files: 1          /          1 [================================================] 100% ✔
Bytes: 1.198 MiB  /  1.198 MiB [================================================] 100% ✔
Completed in 1.7s: 713.5 KiB/s, 1 files/s

▶ ls moby
moby10b.txt
```

## Use Datasets in an Experiment

To demonstrate how to use a dataset in an experiment, we'll run the same `ls`
command we ran above as a Beaker experiment. The code for this experiment's
image can be found [here](../examples/list-files).

```bash
cat << EOF | beaker experiment create -
version: v2-alpha
tasks:
- name: list-files
  image: 
    beaker: examples/list-files
  envVars:
    - name: LIST_DIR
      value: /data
  datasets:
  - mountPath: /data
    source:
      beaker: examples/moby
  result:
    path: /results
  context:
    cluster: ai2/example
EOF
```

This command will print a URL to your experiment, which should complete momentarily.
