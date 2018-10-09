# 10-703 Project: Manipulation Environment

This is a modification of the KUKA robotic arm environment in PyBullet.

Additional details and instructions to be added soon.

## Requirements

- PyBullet (https://github.com/bulletphysics/bullet3)
- OpenAI Gym (https://github.com/openai/gym).

## Installation Instructions

Once you have Gym and PyBullet, you should be able to directly use the environment. 

To verify that everything is correctly set up, run `python3 KukaTest_10703.py /direct/path/to/items/directory`. You should be able to see a visualizer of the KUKA arm and be able to control the arm using the debug sliders. The test program will run episodes until you terminate it. 


## Using the OpenAI Gym Interface

Note that the environment inherits from KukaDiverseObjectEnv, so the implementation of many environment components can be found in `https://github.com/bulletphysics/bullet3/blob/master/examples/pybullet/gym/pybullet_envs/bullet/kuka_diverse_object_gym_env.py`.

### Action Space

The action space can be made either discrete or continuous using the `isDiscrete` parameter of the environment constructor. 

If discrete, then the action space consists of 7 actions:
  - `0`: No action.
  - `1`: Negative `dX`.
  - `2`: Positive `dX`.
  - `3`: Negative `dY`.
  - `4`: Positive `dY`.
  - `5`: Negative `dA`.
  - `6`: Positive `dA`.

  If the "height hack" is not used, then there are 9 actions instead. Actions 1-4 are the same and the others are:
  - `5`: Negative `dZ`.
  - `6`: Positive `dZ`.
  - `7`: Negative `dA`.
  - `8`: Positive `dA`.

If continuous, then the action space consists of a 3-tuple:
  `(dX, dY, dA)`

  If the "height hack" is not used, then it is a 4-tuple instead:
  `(dX, dY, dZ, dA)`

`dX`, `dY`, and `dZ` are the offsets along the x-axis, y-axis, and z-axis, resptectively. `dA` is the vertical angle offset. The end effector will automatically attempt to grasp when close to the bin. 

### Observation Space

The default observation returned by `step` is an RGB image whose height and width is determined by the `height` and `width` passed to the environment constructor. 

A feature vector observation of the current state can be obtained using `get_feature_vec_observation()`. This consists of the arm's position and orientation, the distance and angle of the block, and the block's class label (number of the URDF file used when loading the item). If using this representation, you may want to expand the final value into one binary variable per potential URDF file. 

### Use

The environment can be used as shown in `KukaTest_10703.py`:
```python
env = KukaVariedObjectEnv(item_path)

# The created environment can then be used as any other 
# OpenAI gym environment. For example:
state = env.reset()
done = False
while not done:
  # Sample a random action.
  action = env.action_space.sample()
  # Run a simulation step using the sampled action.
  new_state, reward, done, info = environment.step(action)
  state = new_state
```

## Troubleshooting

[no content yet; check back later]