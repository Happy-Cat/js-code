## vue diff算法
**先根据真实DOM生成虚拟DOM，然后在数据变动的时候生成一个新的Vnode，然后将新的Vnode与旧的Vnode进行节点对比**

**diff 算法只会对同级节点进行比较，vue 对算法进行优化，如果对比新旧节点发现不值得继续比较，就会将新的节点（包括子节点）直接替换旧的节点，而不继续进行子节点的比较**

**当新旧父节点判断值得比较，那么就会继续对其子节点进行比较**

**遵循的规则为：**

**1.如果两个子节点相同这直接return**

**2.如果新节点存在子节点，而就节点不存在子节点，则直接实例化插入真实dom**

**3.如果新节点不存在子节点，而旧节点存在子节点，那么就将真实dom的子节点删除**

**4.如果新旧节点都有文本且不相同，那么直接将真实的dom文本节点替换成新的文本节点**

**5.如果新旧节点都有子节点，那么将对子节点进行比较，具体比较规则是先对同级子节点记录首位和末位，同时比较oldS&&newS,oldE&&newE,oldS&&newE,oldE&&newS,如果匹配上则将真实的dom移动到对应位置，如果匹配不上则直接使用当前位置插入真实dom，然后指针向中间移动，最终比较，如果旧节点先遍历完，就将新节点中未遍历的按照index插入真实dome，如果新节点先遍历完，就将旧节点中未遍历的从真实dom中删除**
