# codereview

 特效分析工具需要分析大批量数据，原始文件夹数据有几万，需要工具分析出哪些老特效是在线性下可以接受的，那些老特效是必须修改的，输出的数量？

最开始的想法是直接批量处理，然后输出txt文件出来。但是遇到很多问题，

1 数量太大，因为需要从gameobj中查看layer,光排除掉ui特效就需要几分钟

2 排除掉ui特效后，数量还有2万，现有分析工具需要分析每个特效平均需要2s，分析完所有特效需要10几个小时，

3 得到结果以后，发现算法还是有限制，不能刷选出所有特效，所以需要支持美术人工查看然后标记状态

直接使用数据库搞定，既cache每个特效数据状态，

```text
public class FxDataCache : ScriptableObject
{

    public List<string> _keys = new List<string>();
    public List<FXInfo> _values = new List<FXInfo>();

    public Dictionary<string, FXInfo> AssetDic = new Dictionary<string, FXInfo>();//guid-- FXInfo
}
```

```text
public enum FXState
{
    FX_NotCollected,//仅仅记录，没有收集
    FX_NotCollected_3DUI,//仅仅记录，没有收集， depercated, use ThreeDUISFX
    FX_NotChecked,//收集了，但是没有检查
    FX_CheckFail,//收集了，但是检查失败
    FX_NotModified,//检查了，但是没有修改
    FX_Passed, //已通过
}
```

也cache每个数据的分析结果FXResult（算法不变）,

```text

[System.Serializable]
public class FXInfo
{
    public int FXPriority;
    public int ApearCount;//Three month ,1%%的提交比率

    public FXState FXAssetState;

    public string FXPath;

    public FXResult MaxResult;

    public bool HasAnalyzed = false;

    public bool ThreeDUISFX = false;//3dui 不确定是用来做ui特效还是3d特效
}

```

益处：

1 特效是不是ui特效，cache下来，不需要反复实例化gameobject

2 每次分析都能增量分析，只需要取出FX\_NotChecked的特效列表处理

3每次分析都能增量分析，只需要FX\_NotModified的特效列表继续优化算法

4美术修改单个特效的状态，相当于修改FXState

5 每种状态的数据都能单独处理，单独输出



