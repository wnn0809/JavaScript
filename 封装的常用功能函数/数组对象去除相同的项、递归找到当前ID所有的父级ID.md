记录下项目开发中遇到的两个实际需求。

## 1.数组对象中去除相同ID的项

从后台接口拿到的数据格式是数组对象，但是会存在前后调用同一个接口数据项重复的现象。导致页面中数据显示有重复的。所以在从接口取到数据以后，去除数组中有相同项的对象。

比如我从接口拿到的数据如下：

```javascript
list: [
   { mapcode: 7, name: '快鱼', price: 2 },
   { mapcode: 8, name: '海澜之家', price: 2 },
   { mapcode: 9, name: '森马', price: 2 },
   { mapcode: 13, name: '华为mate10', price: 4 },
   { mapcode: 14, name: '华为mate20', price: 4 },
   { mapcode: 13, name: '华为mate30', price: 4 },
   { mapcode: 8, name: '华为mate40', price: 15 },
 ]
```

mapcode代表一个位置标志，勾选复选框时传的值，如果mapcode相同，则会同时勾选mapcode相等的复选框，所以需要去重。下面`this.filterRepeat(this.list)`调用封装的函数。

```javascript
this.filterRepeat(list)
```

```javascript
//  数组对象去重
filterRepeat(arr) {
  // 去掉相同id的项目
  /*
  @element: 当前遍历到的元素。
  @index: 当前遍历到的索引。
  @self: 数组本身。
  */
  arr = arr.filter((element, index, self) => {
    return self.findIndex(el => el.mapcode === element.mapcode) === index
  })
  return arr
}
```

filter和findIndex、find：

filter() 方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。 常用于过滤。

findIndex()方法返回数组中满足提供的测试函数的第一个元素的索引。否则返回-1。

find() 方法返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined。


## 2.递归找到所在ID的所有父级元素


对于菜单通常都不止只有一级，往往是一个树形结构。最近的一个需求点击当前菜单需要拿到其所有的父级来回显展示。后端接口数据类似下面这样。

```
meauList: [
   { meauId: 7, name: '快鱼', parentId: 2 },
   { meauId: 8, name: '海澜之家', parentId: 2 },
   { meauId: 9, name: '森马', parentId: 2 },
   { meauId: 13, name: '华为mate10', parentId: 4 },
   { meauId: 14, name: '华为mate20', parentId: 4 },
   { meauId: 15, name: '华为mate30', parentId: 4 },
   { meauId: 16, name: '华为mate40', parentId: 15 },
   { meauId: 17, name: '华为mate50', parentId: 16 },
   { meauId: 4, name: '华为', parentId: 1 },
   { meauId: 5, name: 'ViVo', parentId: 1 },
   { meauId: 6, name: 'OPPO', parentId: 1 },
   { meauId: 10, name: '长虹', parentId: 3 },
   { meauId: 11, name: '飞利浦', parentId: 3 },
   { meauId: 12, name: '松下', parentId: 3 },
   { meauId: 1, name: '手机' },
   { meauId: 2, name: '服装' },
   { meauId: 3, name: '家电' }
 ]
```

使用递归实现本次需求，将所有的父级以一级二级三级的顺序写在一个数组中返回。封装函数如下：

```javascript
geTopParentId(jsonData, id) {
     let arr = []
     let n = id
     let rev = (jsonData, id) => {
       jsonData.forEach(element => {
       if (element.meauId=== n) {
       // 递归出口
         if (element.parentId) {
           n = element.parentId
           arr.unshift(n)
           rev(jsonData, n)
         }
       }
     })
     return arr
     }
     arr = rev(jsonData, id)
     console.log('arr', arr)
   }
 }
```

比如我点击的当前菜单ID是17，调用函数

```javascript
this.geTopParentId(this.meauList, 17)
```
得到结果：

[16,15,4,1]


因为项目使用的是vue，所以调用函数时会用到this。