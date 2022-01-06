# AttentionDRLforSafety
This is the guideline of the source code: Attention-based Deep Reinforcement Learning for Autonomous Driving Safety. 

## Project Folders
This repository contains three folders: 

highway-env: adapted from [highway-env]. Modifications include: added arrive reward for roundabout scenario; changed the default duration setting of roundabout scenario to 13s.

rl-agents: The project to compare safety performance of Attention-DQN, and MultiQ-attention-DQN. It is adapted from [rl-agents]. Modifications: added the logic to write safety metrics (success rate, collision rate) of the ego car to Tensorboard; added a LayerNorm layer to the attention network; add the logic to render the attention values on testing episodes;

test: The project to compare safety performance of DQN, PPO, A2C algorithms. The three algorithms are implemented by [stable-baselines]. This folder uses the highway-env in this repository as the simulated driving environment.


## Installation
Software environment:

The project supports Python3. The recommended version is Python 3.6.11.

Anaconda is recommended for managing the environment.

1. Download the repository.

2. Navigate to highway-env and execute ```python setup.py install```

3. Navigate to rl-agents and execute ```python setup.py install```

4. The test folder requires the [stable-baselines] library. Install by executing ```pip install stable-baselines==2.10.2```

5. Install all the other necessary modules when suggested/prompted.

## Training
To reproduce experiment1 results in the thesis, navigate to test/intersection(or roundabout), execute ```python dqn.py/ppo2.py/a2c.py``` to train a DQN/PPO/A2C model in intersection(or roundabout) scenario.

To reproduce experiment2 results in the thesis, navigate to rl-agents/scripts:

For Baseline-DQN, execute ```python experiments.py evaluate configs/RoundaboutEnv/env_kinematics_13.json configs/IntersectionEnv/agents/DQNAgent/baseline.json --train —episodes=60000 --no-display``` to train a Baseline-DQN model in roundabout scenario for 60,000 episodes.

For Attention-DQN, execute ```python experiments.py evaluate configs/RoundaboutEnv/env_kinematics_13.json configs/IntersectionEnv/agents/DQNAgent/ego_attention_2h.json --train —episodes=60000 --no-display``` to train a Attention-DQN model in roundabout scenario for 60,000 episodes.

For Multiq-attention-DQN, execute ```python experiments.py evaluate configs/RoundaboutEnv/env_kinematics_13.json configs/IntersectionEnv/agents/DQNAgent/self_attention_2h.json --train —episodes=60000 --no-display``` to train a Multiq-attention-DQN model in roundabout scenario for 60,000 episodes.

For intersection scenario, the commands are similar, just change the environment config to ```configs/IntersectionEnv/env.json```.

## Testing

To reproduce experiment1 results in the thesis, navigate to test/intersection(or roundabout), execute ```python eval_dqn.py/eval_ppo2.py/eval_a2c.py``` to test the trained DQN/PPO/A2C model in intersection(or roundabout) scenario for 100 episodes. The trained model are saved in the current intersection(or roundabout) directory. Change the model name in the ```eval_*.py``` to the name of the model you want to test. You can also run ```python batch_test.py``` for a batch testing.

To reproduce experiment2 testing results in the thesis, navigate to rl-agents/scripts:

The commands are very similar to training:

Execute ```python experiments.py evaluate configs/IntersectionEnv/env.json configs/IntersectionEnv/agents/DQNAgent/ego_attention_2h.json --test --episodes=100 --recover-from out/IntersectionEnv/DQNAgent/run_20210504-195707_72845/checkpoint-best.tar --no-display``` to test a trained model by Attention-DQN in intersection scenario for 100 episodes. 

If you want to render the testing videos, remove the ```--no-display``` part. 

