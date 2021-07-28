# Your First Beaker Experiment

Beaker manages experiments as Docker containers so your code can run anywhere on the cloud or on-premise. As an example, this tutorial will show you how to train a Python PyTorch model using LeCun's MNIST dataset of images of handwritten digits.

Don't worry if you don't know about Python, Pytorch, or MNIST data; you won't need to here. This exercise simply shows you how to run a full experiment with Beaker so you can adapt this example to your workload.

## Running your experiment from the CLI

For these instructions to work, you must have already [installed Beaker](install.md). When installed:

1. Go to the existing [MNIST Example experiment's spec page](https://beaker.org/ex/ex_b22vuonnn9tu/spec).

   ![Spec for MNIST example](../images/ex_spec.png)

   The spec format is described in more detail [here](../concept/experiments.md#spec-format)

2. Click **Download** to save the experiment's YAML spec file to a location on your local hard drive.
This specification fully defines a Beaker experiment so it can be run anytime and anywhere.

3. From the command line at this directory, enter:

   ```bash
   beaker experiment create MNIST_Example.yaml
   ```

Once you see the following output your experiment is running:

```bash
Experiment <experiment_id> submitted. See progress at https://beaker.org/ex/<experiment_id>
```

You can see your experiments tasks progress to completion at the URL provided by this output (for example, [https://beaker.org/ex/ex_b22vuonnn9tu/tasks](https://beaker.org/ex/ex_b22vuonnn9tu/tasks)).

If you succeeded in running this example, you are ready to proceed to creating your own Beaker image and dataset, which is the [next example](image.md).
