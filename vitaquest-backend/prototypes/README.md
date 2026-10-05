# Habit prototype embeddings

Store one NumPy embedding file per supported habit:

```text
prototypes/
├── gym.npy
├── running.npy
├── reading.npy
├── cooking.npy
└── meditation.npy
```

Each `.npy` file must contain an array of shape `(N, D)` or `(D,)`, where `N` is the number of prototype examples and `D` is the embedding dimension. The server accepts a single embedding or multiple examples and normalizes them when computing similarity.
