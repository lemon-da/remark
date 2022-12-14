# LSM（Log Structured Merged Tree）树原理

### LSM树含义（个人理解）

> + 其包括内存和磁盘两部分组成（内存中：memtable 磁盘中：SSTable）
> 
> + 其由多个level组成，最初的level存储在内存中（采用排序的数据结构，例如红黑树等）
> 
> + 每一层level都有一个阈值，到达阈值后会进行合并操作并计入下一层
> 
> + 在更新操作中只允许在内存中原地更新

### 数据插入

> 直接插入在最底层的level（内存中）

### 数据删除

> 1. 所需删除的数据在内存中：查询level0，将所需删除的数据打上墓碑标记
> 
> 2. 所需删除的数据在磁盘中：不管磁盘中的数据，直接在内存中的level0插入该数据的墓碑标记
> 
> 3. 所需删除的数据不存在：同（2）相同，直接插入墓碑标记即可

### 数据修改

> 1. 所需修改的数据在内存中：查询level0中的数据并修改
> 
> 2. 所需修改的数据在磁盘中：直接在内存中的level0内插入新的值
> 
> 3. 所需修改数据不存在：同（2）相同，直接插入新值即可

### 数据查询

> 1. 所需查询数据在内存中：直接返回值
> 
> 2. 所需查询数据在磁盘中：依照level顺序查询
> 
> （使用二分、布隆过滤器等操作可以加速数据查询过程）

### 合并操作（核心）

> 当内存中的level0内存储数据达到阈值便开始进行合并操作，将其顺序写入磁盘中。若此时磁盘内的各个level也达到阈值则进行归并操作，合并到更高层级的level中
