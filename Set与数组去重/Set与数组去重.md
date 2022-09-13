<a name="TvdMn"></a>
# Set简介
Set与数组,函数一样也属于 _对象_<br />**Set 对象允许你存储任何类型的唯一值, 无论是原始值或者是对象引用**

---

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663093366990-48be7403-1ade-4d7e-9090-425926a4f151.png#clientId=u8daa93ea-62d7-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=40&id=u71fa2124&margin=%5Bobject%20Object%5D&name=image.png&originHeight=50&originWidth=281&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24544&status=done&style=none&taskId=u124bf225-01b6-4723-80c9-d4a9bfd3748&title=&width=224.8)<br />Set类似数组可以存储不同类型的值,但是只允许存储不同类型的唯一值<br />当输入相同类型的相同值是,Set会_**自动去重**_<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663093419970-13abff22-9084-4add-aeea-9c6644dc5509.png#clientId=u8daa93ea-62d7-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=38&id=u1cfd8c4a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=47&originWidth=307&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30399&status=done&style=none&taskId=u8953259b-8249-4883-b7ad-f73cdec21f0&title=&width=245.6)

---

对相同的类型 undefined和null  去重<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663093576369-d86cf614-88b7-4c62-b74d-9d173dcf9c75.png#clientId=u8daa93ea-62d7-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=218&id=ua80046d4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=273&originWidth=657&originalType=binary&ratio=1&rotation=0&showTitle=false&size=112525&status=done&style=none&taskId=uaea91419-5e93-4762-ac8f-e960ae29185&title=&width=525.6)

---

对同一对象进行去重<br />**3个a变成1个a**<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663093623352-f3dad366-f253-4277-9ee3-13f00044f3a4.png#clientId=u8daa93ea-62d7-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=153&id=u08cac13a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=191&originWidth=351&originalType=binary&ratio=1&rotation=0&showTitle=false&size=67625&status=done&style=none&taskId=u34e75bd1-2add-4fc2-a1e2-80964444a0a&title=&width=280.8)

---

<a name="ce3Ra"></a>
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
**Output:**<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663094237894-ead53767-f88b-45b2-bb25-9b2ffd2e0781.png#clientId=ud9134a65-34bb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=28&id=u75e16344&margin=%5Bobject%20Object%5D&name=image.png&originHeight=35&originWidth=424&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2188&status=done&style=none&taskId=ued8d30a2-fb4e-4671-bcc4-dd8291a646e&title=&width=339.2)
<a name="DiO3k"></a>
## 缺点
<a name="gdZ9A"></a>
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
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663094557343-e969eb6e-68bb-4f20-92d5-50b06d5cee5f.png#clientId=ud9134a65-34bb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=26&id=u55ab93d7&margin=%5Bobject%20Object%5D&name=image.png&originHeight=33&originWidth=313&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1813&status=done&style=none&taskId=u202a54f1-a157-4c6c-8b89-23de7c8f2bf&title=&width=250.4)
<a name="d1JNH"></a>
### 无法统计对象
如下代码: 当传入多个对象时,都会变成_['1', '2', '3', '[object Object]']_
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
**Output:**<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663095788050-2fe3dc3b-c8a7-4c24-a8d1-60ed640daf44.png#clientId=u0bed46b6-d627-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=29&id=u4639184a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=36&originWidth=543&originalType=binary&ratio=1&rotation=0&showTitle=false&size=3301&status=done&style=none&taskId=ue2c1698a-6999-4513-8b97-08d6a75d4c6&title=&width=434.4)


<a name="JTMdd"></a>
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
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663096306990-6a8fe37c-86c2-43d8-b4a5-04ef4fa20f84.png#clientId=u0bed46b6-d627-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=245&id=u71379aaa&margin=%5Bobject%20Object%5D&name=image.png&originHeight=306&originWidth=545&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16800&status=done&style=none&taskId=u5c19d591-df0f-4974-81bd-3c630bc2080&title=&width=436)

<a name="ejsT0"></a>
# 对于面试提问数组去重的完整回答
如果不能使用ES6语法,我会用一种算法,这种算法是这样的<br />定义一个数组接收,定义一个对象 <br />遍历这个数组,将这个数组的值作为一个对象的唯一下标储存起来,设置为true.<br />这样如果数组是 1,2,3.那么得到的对象就有三个下标分别为123,他们的值都为true.<br />然后把这个hash对象的key遍历一下,放到新的数组,return出来.但是不太完美,<br />该方法无法分别数字和字符串,还有如果数组里面有对象,就统计不了.以为如果对象作为下标的话,js会调用toString方法将对象转换成字符串<br />如果可以用ES6的语法,就直接用new Set(array)然后用Array.from将set对象转换成数组就ok了
<a name="kwnAe"></a>
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
<a name="QyF5M"></a>
# 补充
<a name="yLOay"></a>
## !!! 只支持一种数组下标就是字符串
> 当传入别的对象类型作为数组下标时,JS会调用toString方法将该对象转换成String类型

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
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663094798246-68eb4216-e053-48cf-8910-08ab2cab25a1.png#clientId=ud9134a65-34bb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=32&id=YosYK&margin=%5Bobject%20Object%5D&name=image.png&originHeight=40&originWidth=232&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2703&status=done&style=none&taskId=u834e3b23-63fb-4ab5-9aed-31afa6a0df4&title=&width=185.6)<br />输出的是字符串

---

传入对象作为下标
```javascript
        var hash = {}
        // 传入对象{name: 'object'} 作为下标
        hash[{name: 'object'}] = true
        // 输出hash对象
        console.log(hash);// {[object Object]: true}
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663095319792-00ddad68-f530-40c9-b319-0f6155d28f72.png#clientId=u0bed46b6-d627-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=26&id=ubcaf7a59&margin=%5Bobject%20Object%5D&name=image.png&originHeight=33&originWidth=344&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2730&status=done&style=none&taskId=u3bf7df1a-4e78-4a41-abda-49e57099b7f&title=&width=275.2)<br />传入所有对象都会变成  _{[object Object]: true}_<br /> 当JS发现传入下标的值是对象时就会调用toString方法<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/32474867/1663095678915-4cf71837-5452-48b4-9151-5cdaa26dab5a.png#clientId=u0bed46b6-d627-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=74&id=uca304c0e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=93&originWidth=177&originalType=binary&ratio=1&rotation=0&showTitle=false&size=28662&status=done&style=none&taskId=u1bd224c3-b167-448a-9689-ea7803e8100&title=&width=141.6)
