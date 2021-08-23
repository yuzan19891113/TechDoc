# Lua



### 有的Unity对象，在C\#为null，在lua为啥不为nil呢？比如一个已经Destroy的GameObject

其实那C\#对象并不为null，是UnityEngine.Object重载的==操作符，当一个对象被Destroy，未初始化等情况，obj == null返回true，但这C\#对象并不为null，可以通过System.Object.ReferenceEquals\(null, obj\)来验证下。

对应这种情况，可以为UnityEngine.Object写一个扩展方法：

```text
[LuaCallCSharp]
[ReflectionUse]
public static class UnityEngineObjectExtention
{
    public static bool IsNull(this UnityEngine.Object o) // 或者名字叫IsDestroyed等等
    {
        return o == null;
    }
}
```

然后在lua那你对所有UnityEngine.Object实例都使用IsNull判断

```text
print(go:GetComponent('Animator'):IsNull())
```

