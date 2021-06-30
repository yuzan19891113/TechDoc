# Unity基本知识

## 保存

资源只有在markdirty才Ctrl +ｓ才会触发保存

**场景**：EditorSceneManager.MarkSceneDirty\(EditorSceneManager.GetActiveScene\(\)\)

**其他资源：**

```text
 string[] result = AssetDatabase.FindAssets(ArtSetting.name);
            if (result.Length != 0)
            {
           
                EditorUtility.SetDirty(ArtSetting);
                AssetDatabase.SaveAssets();
                AssetDatabase.Refresh();
            }
```





