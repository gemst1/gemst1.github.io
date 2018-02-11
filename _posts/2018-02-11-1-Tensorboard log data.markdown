---
title: "How to get full data log from TensorBoard?"
layout: post
tag:
- TensorFlow
- TensorBoard
- Data log
- Tensor size
- Data size
img: indigo/indigo.png
blog: true
author: gemst1
summary: "The way to get full learning data log from TensorBoard"
permalink: /:title
---

### 1. How to run TensorBoard?

[TensorBoard](https://www.tensorflow.org/programmers_guide/summaries_and_tensorboard) is a visulizing tool, which visualize a learning process or data log by reading TensorFlow events files. The event files contain summary data that is generated when running TensorFlow. You can run TensorBoard, use the following command in terminal window.

```sh
tensorboard --logdir=your/path/to/log-directory
```

Then terminal window shows an address which can view the TensorBoard. Type the address in web browser address bar to view the TensorBoard which looks like below picture. If your log directory contains several event files then TensorBoard visualize all of them. Thus, you sould arrange the event files carefully.

![Pic](https://github.com/gemst1/gemst1.github.io/blob/master/assets/images/TensorBoard.png?raw=true)
<figcaption class="caption">TensoBoard</figcaption>

### 2. How to download the scalar data?

To download the learning data, check the `Show data download links` at left top of the TensorBoard window. Then you can see the download links at bottom of a each data log plot. TensorBoard provides two types of data log for download `.csv` and `.json`. If you click `run to download`, then the download links for each data type are activated and you can download data.

But these data files contain only **1000 steps of full learning steps** (if your learning step is less than 1000, the file contains all of learning data log, maybe. I didn't check about it actually) because default setting of the TensorBoard for data log size is 1000. So, if you need the learning data for every step, you should change the setting.

### 3. How to get full data log from TensorBoard?

You can find the setting information, maybe, at `/usr/local/lib/python3.5/dist-packages/tensorboard/backend/application.py` (the directory could be differeny slightly depends on your development environment). At 56th line in the `application.py`, in my case, you can find some lines as below.

```python
DEFAULT_TENSOR_SIZE_GUIDANCE = {
    scalar_metadata.PLUGIN_NAME: 1000,
    image_metadata.PLUGIN_NAME: 10,
    audio_metadata.PLUGIN_NAME: 10,
    histogram_metadata.PLUGIN_NAME: 500,
}
```

The scalar data log size which we want to change is depends on the `scalar_metadata.PLUGIN_NAME` which is setted 1000 as default. To make TensorBoard contain scalar data of every step, we shuold change the value to `0` as the following.

```python
DEFAULT_TENSOR_SIZE_GUIDANCE = {
    scalar_metadata.PLUGIN_NAME: 0,
    image_metadata.PLUGIN_NAME: 10,
    audio_metadata.PLUGIN_NAME: 10,
    histogram_metadata.PLUGIN_NAME: 500,
}
```

After chaning the value `1000` to `0`, rerun the TensorFlow and then you can get full data log of your learning process following the step 2.
