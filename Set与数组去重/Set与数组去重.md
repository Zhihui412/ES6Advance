# Set简介

Set与数组,函数一样也属于 *对象*

**Set 对象允许你存储任何类型的唯一值, 无论是原始值或者是对象引用**

------

![img](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663093366990-48be7403-1ade-4d7e-9090-425926a4f151.png)

Set类似数组可以存储不同类型的值,但是只允许存储不同类型的唯一值

当输入相同类型的相同值是,Set会***自动去重\***

![img](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663093419970-13abff22-9084-4add-aeea-9c6644dc5509.png)

------

对相同的类型 undefined和null  去重

![img](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663093576369-d86cf614-88b7-4c62-b74d-9d173dcf9c75.png)

------

对同一对象进行去重

**3个a变成1个a**

![img](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663093623352-f3dad366-f253-4277-9ee3-13f00044f3a4.png)

------

# 使用计数排序进行数组去重

```javascript
    var a = [1,2,3,2,1,4,5,3,4]

    function uniq(array){
        var result = []

        var hash = {}

        for(let i = 0; i< array.length; i++){
            hash[array[i]] = true
        }

        for(let key in hash){
            result.push(key)
        }

        return result
    }

    console.log(uniq(a))// ['1', '2', '3', '4', '5']
```

**Output:**

![img](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663094237894-ead53767-f88b-45b2-bb25-9b2ffd2e0781.png)

## 缺点

### 无法区分字符串和数字

如下代码,应该输出 [1,2,3,'2'] ,但是结果是  ['1', '2', '3']

```javascript
    var a = [1,2,3,'2',3,1]

    function uniq(array){
        var result = []

        var hash = {}

        for(let i = 0; i< array.length; i++){
            hash[array[i]] = true
        }

        for(let key in hash){
            result.push(key)
        }

        return result
    }

    console.log(uniq(a))// ['1', '2', '3']
```

![img](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663094557343-e969eb6e-68bb-4f20-92d5-50b06d5cee5f.png)

### 无法统计对象

如下代码: 当传入多个对象时,都会变成*['1', '2', '3', '[object Object]']*

```javascript
        var a = [1, 2, 3, 2, 1, {name: 'Object'},{sex: 'man'},{age: '18'}]

        function uniq(array) {
            var result = []

            var hash = {}

            for (let i = 0; i < array.length; i++) {
                hash[array[i]] = true
            }

            for (let key in hash) {
                result.push(key)
            }

            return result
        }
```

**Output:**

![img](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663095788050-2fe3dc3b-c8a7-4c24-a8d1-60ed640daf44.png)





# ES6中使用Set对象进行数组去重

```javascript
var a = [1, 2, 3, 4,'4','3',2, 1, {name: 'Object'},{name: 'Object'},{sex: 'man'},{age: '18'}]

function uniq(array){
    return Array.from(new Set(array))
    // 采用析构语法
    return [ ... new Set(array)]
}

console.log(uniq(a))// [1, 2, 3, 4,'4','3',{name: 'Object'},{name: 'Object'},{sex: 'man'},{age: '18'}]
```

![img](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663096306990-6a8fe37c-86c2-43d8-b4a5-04ef4fa20f84.png)



# 对于面试提问数组去重的完整回答

如果不能使用ES6语法,我会用一种算法,这种算法是这样的

定义一个数组接收,定义一个对象 

遍历这个数组,将这个数组的值作为一个对象的唯一下标储存起来,设置为true.

这样如果数组是 1,2,3.那么得到的对象就有三个下标分别为123,他们的值都为true.

然后把这个hash对象的key遍历一下,放到新的数组,return出来.但是不太完美,

该方法无法分别数字和字符串,还有如果数组里面有对象,就统计不了.以为如果对象作为下标的话,js会调用toString方法将对象转换成字符串

如果可以用ES6的语法,就直接用new Set(array)然后用Array.from将set对象转换成数组就ok了

# Set几种常用的方法

```javascript
ley mySet =  new Set()

mySet.add(1);
mySet.add(5);
mySet.has(5);		// true
mySet.size; 		// 2 ,相当于数组的length
mySet.delete(5) // true

//迭代整个set
for (let item of mySet) console.log(item)
```

# 补充

## !!! 只支持一种数组下标就是字符串

当传入别的对象类型作为数组下标时,JS会调用toString方法将该对象转换成String类型

分别传入字符串'4'和数字4作为下标

```javascript
var hash = {}
// 传入字符串'4' 作为下标
hash['4'] = '字符串'
// 再传入数字4作为下标
hash[4] = '数字'
// 输出hash对象,应该有两个键值对
// 实际上只有一个,以下面那个为准
console.log(hash)
```

![img](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663094798246-68eb4216-e053-48cf-8910-08ab2cab25a1.png)

输出的是字符串

------

传入对象作为下标

```javascript
        var hash = {}
        // 传入对象{name: 'object'} 作为下标
        hash[{name: 'object'}] = true
        // 输出hash对象
        console.log(hash);// {[object Object]: true}
```

![img](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663095319792-00ddad68-f530-40c9-b319-0f6155d28f72.png)

传入所有对象都会变成  *{[object Object]: true}*

 当JS发现传入下标的值是对象时就会调用toString方法

![img](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663095678915-4cf71837-5452-48b4-9151-5cdaa26dab5a.png)
