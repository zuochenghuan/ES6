##Set
**ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。**

用法：new Set([iterable])

    const set = new Set([1, 2, 3, 4, 4, 4]);
    //[1, 2, 3, 4]

Set结构有以下属性：

Set.prototype.constructor: 构造函数，默认就是Set函数。

set.prototype.size : 返回Set的成员总数。

Set结构有以下方法：
add(value)：添加某个值，返回 Set 结构本身。
delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
has(value)：返回一个布尔值，表示该值是否为Set的成员。
clear()：清除所有成员，没有返回值。
 
    const set = new Set();
     //添加元素
    set.add(1).add(2).add(2);
    set.size;   //2，[1,2]

    //检查元素
    set.has(2);   //true
    set.has(3);   //false

    //删除元素
    set.delete(2);
    set.has(2);    //false

    //清空集合
    set.clear();
    set.size;     //0
  
一些常见应用

    //数组去重
    let arr = [1, 2, 3, 2, 5, 5];
    let unique = [...new Set(arr)];
    // [1, 2, 3, 5]

     //交，并，差集
    let a = new Set([1, 2, 3]);
    let b = new Set([4, 3, 2]);

    // 并集
    let union = new Set([...a, ...b]);
    // Set {1, 2, 3, 4}

    // 交集
    let intersect = new Set([...a].filter(x => b.has(x)));
    // set {2, 3}

    // 差集
    let difference = new Set([...a].filter(x => !b.has(x)));
    // Set {1}
###set的遍历

1.for of

     for(let val of s1.values()){
            console.log('values',val)
      }

2 使用forEach

     s1.forEach(function(item)){
            console.log(item)
      }

##weakSet
1：支持的数据类型不一样，WeakSet值必须是对象

2 ：对象是弱引用，就是WeakSet对象不是一个对象完全拷贝，而是地址引用，不会检测是否被垃圾回收掉。

3.与Set比较，size等属性和claer方法没有 ,不能遍历


##Map

###定义
JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能用字符串当作键。ES6中的Map结构也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

和Object作对比，Object有Key和Value,但是key必须为字符串类型，而Map的key可以任意数据类型。

不能和Set记混了，Map添加元素不是add而是set，取值通过get

用法：new Map([iterable])

    const map = new Map();

属性和方法<br>

- size：返回成员总数。
- set(key, value)：添加键值对到映射中
- get(key)：获取映射中某一个键的对应值
- delete(key)：将某一键值对移除出映射中
- clear()：清空映射中所有的键值对
- entries()：返回一个以二元数组（键值对）作为元素的数组
- has(key)：检查映射中是否包含某一键值对
- keys()：返回一个一当前映射中所有键作为元素的可迭代对象
- values()：返回一个一当前映射中所有值作为元素的可迭代对象
-




     const map = new Map();
 
    //添加键值对
    map.set(1, 'aaa');
    map.set(2, 'bbb').set('string', 'sssss');

    map.get(1);    // 'aaa'
    map.size;      // 3


    //删除键值
    map.delete(2);
    map.has(2);    //false

    //遍历方法
    for(let key of map.keys()){
    console.log(key);
    }
    // 1
    // 'string'

    for(let key of map.values()){
        console.log(key);
      }
    // 'aaa'
    // 'sssss'

    map.forEach(function(value, key, map){
        console.log('key:'+key+', value:'+value)
     })
    // 'key:1, value:aaa'
    // 'key:string, value:sssss'
















