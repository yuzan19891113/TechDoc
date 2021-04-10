# Shader加载

### Editor

#### Load Resource

![file loading](../../../.gitbook/assets/image%20%28161%29.png)

![shader ReImport](../../../.gitbook/assets/image%20%28160%29.png)

####                   

#### CreateShader

![](../../../.gitbook/assets/image%20%28157%29.png)

#### Apply shader                                       

pick shader snippet, generate code hash

![](../../../.gitbook/assets/image%20%28162%29.png)

use code hash to find shader cache from shadercache\(assets/library/shadercache\), if not exsit, compile,save to cache

![](../../../.gitbook/assets/image%20%28158%29.png)

