---
title: "OpenAI gym with Mujoco-py 1"
layout: post
tag:
- github
- blog
- posting
- tag
- test
img: indigo/indigo.png
blog: false
author: gemst1
summary: "Github Jekyll blog posting test"
permalink: /:title
---

### Environment
- Ubuntu 16.04
- [Mujoco 150 linux](https://mujoco.org)
- [Mujoco-py 1.50.1.35](https://github.com/openai/mujoco-py)
- [Openai gym](https://github.com/openai/gym)

---

#### 1. Make Mujoco Model
First of all, to simulate Mujoco in openai gym, we need a MuJoCo XML model file in its native **MJCF** format. You can find details about a modeling process in [here](http://www.mujoco.org/book/modeling.html).
I made 'hand_angle.xml' model to simulate.

#### 2. Build gym environment
After making the model, we can build gym environment to simulate the mujoco model in openai gym framework.
1. Put the model ('hand_angle.xml') to '~your path to /gym/gym/envs/mujoco/assets'
2. Make a new environment python file ('yourenvname.py') in '~ your path to /gym/gym/envs/mujoco'. In my case, 'hand.py'.
I refer to the pre existing files given by gym framework. They provide some examples kind of 'ant.py', 'half_cheetah.py' etc. for newbies like me.
The envirionment python file is composed as below.

{% highlight yml %}
import numpy as np
from gym import utils
from gym import spaces
from gym.envs.mujoco import mujoco_env

class HandEnv(mujoco_env.MujocoEnv, utils.EzPickle):
    def __init__(self):
        mujoco_env.MujocoEnv.__init__(self, 'hand_angle.xml', 5)
        utils.EzPickle.__init__(self)
        low = np.array([0, 0, 0, 0])
        high = np.array([0.05, 0.05, 0.04, 0.065])
        est_high = np.array([0.3, 0.3, 1.5, 1.5])

    def _step(self, action):
        self.do_simulation(action, self.frame_skip)
        ob = self._get_obs()
        done = None
        reward = 0
        return ob, reward, done, {}

    def _get_obs(self):
        return np.concatenate([
            self.model.data.qpos.flat[4:8]
        ])

    def reset_model(self):
        qpos = self.init_qpos
        qvel = self.init_qvel
        self.set_state(qpos, qvel)
        return self._get_obs()

    def viewer_setup(self):
        self.viewer.cam.trackbodyid = 2
        self.viewer.cam.distance = self.model.stat.extent * 1.8
        self.viewer.cam.elevation = -20
{% endhighlight %}
