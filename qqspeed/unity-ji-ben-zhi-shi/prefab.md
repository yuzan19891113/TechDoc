# Prefab

Prefab \(預製物件\) 是在 Unity 中經常被使用的機制之一，能將 GameObject 以及其 Components 的屬性狀態儲存為單一個 Asset。當 Prefab 屬性調整時，Unity 能夠自動更新放置在場景 \(Scenes\) 中的實體物件 \(Prefab instances\)。

它就像是在使用樣板 \(Template\)，例如在射擊遊戲中將大量使用相同的子彈物件，將子彈物件建立成 Prefab，並藉由該 Prefab 在場景大量建立其實體，調整參數只需要調整單一 Prefab 即可，方便設計者快速調整遊戲參數。

 AssetBundles 是 Unity 用來取代 Resources 所建立的機制，若要從**外部載入 Prefab 使用**，這是唯一的方式。得先將 Prefab 打包成 AssetBundle ，之後在執行階段載入該 AssetBundle，然後讀取其內容取得該 Prefab 來使用。

仅仅Prefab instance能够重新设置parent,

