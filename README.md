# Page cache replacement environment
Page replacement policy is pretty important for quick memory access through cache in operating systems. This is a fairly simple (and beta) version of such a cache replacement environment. It is assumed that the operating system requests pages through a uniform probability distribution (see `P` in `os_sim.py`). This environment is created by extending the openAI gym class and can be used to train RL agents to optimally replace page cache in OS.

# How?
```python
from environment import CacheEnv
env = CacheEnv(limit=10, n_pages=5, eps_len=10)
s = env.reset()
a = some_policy(s)
s, r, done, observation = env.step(a)
```
- `eps_len`: Length of an episode. An episode ends when `eps_len` number of actions have been taken.
- `limit`: Cache size.
- `n_pages`: No. of unique pages. (Note: Hard code the probability `P` for length `n_pages` in `os_sim.py`)

# Files
- `environment.py` file with the `CacheEnv` class.
- `os_sim.py` is a simulation of simple page request schedule for the operating system.
- `lfu.py` the least frequently used policy.
- `lru.py` the least recently used policy.

# Action space and reward function
- **Action space**: `0<a<n_pages` `a=2` means evict page 2 from the cache.
- **Reward**:
  - If the action is to evict a page that is not in the cache, a heavy negative reward (`-100`) is given.
  - For each cache hit a positive reward of `+1` is given.
  - For each cache miss a negative reward of `-1` is given.

# State representation
A state represents the current state of the cache. For every page in cache there are two counters:
- `lu`: No. of timesteps ago this page was accessed. inspired by ~LRU (Least recently used policy)
- `nt`: No. of times this page was accessed. inspired by ~LFU (Least frequently used policy)

This environment is far from complete right now. I plan on making a lot of changes.
