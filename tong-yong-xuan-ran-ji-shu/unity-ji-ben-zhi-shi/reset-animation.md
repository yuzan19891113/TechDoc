# Reset Animation

对于Animation 调用Rewind seek回第一帧，但是有时候没有play可能不会起效

```
mAnimation.Play(mAnimation.clip.name);
mAnimation.Rewind();
mAnimation.Sample();
mAnimation.Stop(mAnimation.clip.name);
```
