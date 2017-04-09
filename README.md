# asyn-flow

1、可以预先规划好流程的每一步，如this.setNext('步骤A').setNext('步骤B')……

2、可以在任何一步决定下一步做什么，如 this.setNext('步骤C')，其实这里的API和上面的一样，只是调用的地方不一样而已。

3、在任何一步中，可以知道当前步是在做什么，前面一步做了什么、下一步准备要做什么，如this.getCurr()、this.getPrev()、this.getNext()。

4、当前步做完后，能将结果告诉下一步（仅仅是下一步能获取到当前步传递的结果，也就是为了保护变量污染，每一步都只能获取到前一步的结果），如 给下一步传值this.nextData({name1:value1,name2:value2,……})、获取上一步传来的值this.stepData(name1)或this.stepData()。

5、可以设置或获取整个流程的全局变量，这样所有的步骤都能共享该变量，如 设置全局变量值this.flowData({name1:value1,name2:value2,……})，获取全局变量值this.flowData(name1)或this.flowData()。

6、上一步可以知道当前步的执行结果，成功 or 失败，如 在上一步中设置this.setNext('步骤B', successFun, failFun)、当前步中通过this.success(args)、this.fail(args)来告诉上一步。

7、当前步可以随时通知下一步开始执行，如this.next()。

8、有些步骤能并行执行，并且要都执行完才能执行下一步，如 this.setNext('步骤A').setNext([步骤B1,步骤B2,步骤B3]).setNext('步骤C')。

9、可以在任何时候知道当前代码流程运行过的轨迹，如flowJS.trace，这对于了解页面的执行过程会比较有帮助。

```js
flowJS({
    init:function(){
        this.setNext('步骤A').setNext('步骤B').setNext('步骤C');
        this.next();
    },
    '步骤A':function(){
		var self = this;
		setTimeout(function(){
			console.log('步骤A 传入参数：');
			self.nextData({name1:'value1', name2:'value2'});
        	self.next();
		},3000);
    },
    '步骤B':function(){
        console.log('步骤B 获取上一步传来的数据：');
        console.log(this.stepData()); //打印结果：Object {name1: "value1", name2: "value2"}
        console.log('步骤B 获取上一步传来的属性name1的值：');
        console.log(this.stepData('name1')); //打印结果：value1
        this.next();
    },
    '步骤C':function(){
        console.log('步骤C 获取上一步传来的数据：');
        console.log(this.stepData()); //打印结果：Object {}
        console.log('步骤C 获取上一步传来的属性name1的值：');
        console.log(this.stepData('name1')); //打印结果：undefined
    }
});
```
