# Live caption demo using a microphone.

The demo Python script
[live_captions.py](/moonshine/demo/live_captions.py) prints streaming captions
based on speech detected in the microphone signal.

The following steps were tested in `uv` virtual environment v0.4.25 created in
Ubuntu 22.04 home folder running on a MacBook Pro M2 virtual machine.

- [Live caption demo using a microphone.](#live-caption-demo-using-a-microphone)
- [Installation.](#installation)
- [Run the demo.](#run-the-demo)
- [Script notes.](#script-notes)
- [Future work.](#future-work)

# Installation.

Moonshine installation steps are available in the
[top level README](/README.md) of this repo.  The same set of steps are included here for Moonshine Torch model.

First install the `uv` standalone installer as
[described here](https://github.com/astral-sh/uv?tab=readme-ov-file#installation).

Create the virtual environment and install dependences for Moonshine.
```console
cd
uv venv env_moonshine_demo
source env_moonshine_demo/bin/activate

uv pip install useful-moonshine@git+https://github.com/usefulsensors/moonshine.git
```

The demo requires these additional dependencies.
```console
uv pip install -r moonshine/moonshine/demo/requirements.txt
```

# Run the demo.

Check your microphone is connected and the microphone volume setting is not
muted in your host OS or system audio drivers.
```console
cd
source env_moonshine_demo/bin/activate

cd moonshine/moonshine

export KERAS_BACKEND=torch

python3 ./demo/live_captions.py
```
Speak in English language to the microphone and observe live captions in the
terminal.  Quit the demo with ctrl + C to see console print of the captions.

An example run.
```console
(env_moonshine_demo) parallels@ubuntu-linux-22-04-02-desktop:~/moonshine/moonshine$ export KERAS_BACKEND=torch
(env_moonshine_demo) parallels@ubuntu-linux-22-04-02-desktop:~/moonshine/moonshine$ python3 ./demo/live_captions.py
Loading Moonshine model ...
/home/parallels/env_moonshine_demo/lib/python3.10/site-packages/keras/src/ops/nn.py:545: UserWarning: You are using a softmax over axis 3 of a tensor of shape torch.Size([1, 8, 1, 1]). This axis has size 1. The softmax operation will always return the value 1, which is likely not what you intended. Did you mean to use a sigmoid instead?
  warnings.warn(
Press Ctrl+C to quit live captions.

^C
Cached captions.
Being in Germany after nearly dying from being poisoned by Russian agents, and you and he walked through the terminal after he landed, and then he was immediately arrested in customs, and imprisoned never to be free again. Did you know at that moment that that may be the last time you were together? I didn't think about that at this moment. I knew that we are going at our homeland. We wanted to go there on you that it was very important for. My husband to cop back to russia to show that he is not afraid to show and to encourage all his supporters not to be afraid i knew that it's very important for him and I knew that it could be dangerous but I knew that he would never do it in another way.
(env_moonshine_demo) parallels@ubuntu-linux-22-04-02-desktop:~/moonshine/moonshine$
```

# Script notes.

The script `live_captions.py` loads the English language version of Moonshine
model.

The script includes logic to detect speech activity and limit the context window
of speech fed to the Moonshine model.  The returned predictions are cached and
printed on exit.  Some hallucinations will be seen when live captioning is
running: one reason is speech gets truncated out of necessity to generate the refresh and timeout transcriptions.  See the final printed captions for the best
transcription result.

If you run this script on a slower processor consider increasing the value of
`REFRESH_SECS` to reduce the period of refresh predictions.  This avoids
overloading the script's main thread and prevents increasingly slower caption updates.  Also consider reducing the value of `MAX_SPEECH_DURATION` to avoid slower model inferencing with longer speech.

# Future work.

* The script uses Python Torch version of the model.  Faster inferencing and caption updates are expected with an ONNX runtime version of the model.
* Short pause detection to mitigate speech truncation with refresh and timeout transcriptions.