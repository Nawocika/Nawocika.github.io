
    growup () {
      this.obj = {
        name: 'Wang Ming',
        age: 28,
        favs: ['running', 'swimming', 'dance'],
        brother: {
          name: 'Wang Gang',
          age: 24,
          favs: ['computers', 'eating'],
        }
      }
    },
  }
然后通过growup方法，对对象进行重新赋值,会发现即使deep为false，依然可以触发watch！
在这里插入图片描述

但是，如果只是为了修改某个对象的属性，而让整个对象重新赋值，这种方法听起来就挺蠢的！效率低，消耗内存，还容易出错。万一少了赋值了一个属性，导致属性丢失，前面渲染也会出问题。。。所以这事就当了解，除了某些特殊情况下可以用用。

五、watch监听数组的变化。
再细心一点的读者朋友们可能会发现，我前面有一个yearList，变成了[2000,null,null]，但是却没有在console中看到监听属性的执行？是我没有定义对应的watch吗？
其实我定义了，但是为了避免让大家误解就没有放出来。

    yearList: {
      handler (newValue, oldValue) {
        console.log('年份列表改变了，新值是' + newValue + ',旧值是' + oldValue)
      },
      deep: true
    },
grow函数来修改数据

    grow () {
      this.year++
      this.obj.age++;
      this.yearList.length++
      this.yearList[0]=1955
    },
在这里插入图片描述
大家可以看到，我这里甚至还设置了deep:true，但是依然没有监听到yearList的变动！为什么呢？
其实是因为，我修改yearList的方式是，this.yearList.length++,直接增加了数组的长度，而由于 JavaScript 的限制，Vue 不能检测以下变动的数组：

当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
当你修改数组的长度时，例如：vm.items.length = newLength

官方文档对此的说明：
https://cn.vuejs.org/v2/guide/list.html#注意事项
但是如果我使用vuejs提供的变异方法，例如push()，就可以实现监听！

  methods: {
    grow () {
      this.year++
      this.obj.age++;
      this.yearList.push(this.year)
    },
  }
在这里插入图片描述
如果你想直接修改数组中的某个值，而不是push数据，官方文档给出了2种解决方案
一、Vue.set
Vue.set(vm.items, indexOfItem, newValue)
二、Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)

你也可以使用 vm.$set 实例方法，该方法是全局方法 Vue.set 的一个别名：
vm.$set(vm.items, indexOfItem, newValue)

为了解决数组长度变动的问题，你可以使用 splice：
vm.items.splice(newLength)

六、deep:true设置了后，对象中的对象能否监听到？
既然deep:true设置之后，可以直接监听对象属性的变化，那么对象中的对象能监听到吗？
于是我们把obj.age++注释，增加this.obj.brother.age++，看看王明兄弟的年龄改变，是否会触发watch

    grow () {
      this.year++
      // this.obj.age++;
      this.yearList.push(this.year)
      this.obj.brother.age++
    },
结果依然是可以的。
在这里插入图片描述

七、只监听对象某个属性变化的优化。
deep的意思就是深入观察，监听器会一层层的往下遍历，给对象的所有属性都加上这个监听器，但是这样性能开销就会非常大了，任何修改obj里面任何一个属性都会触发这个监听器里的 handler。

但是在实际开发过程中，我们很可能只需要监听obj中的某几个属性，这样设置deep:true之后就显得很浪费！
于是我们可以使用字符串形式来优化监听。
前面obj的监听可以去掉了！

  watch: {
    'obj.age': {
      handler (newValue, oldValue) {
        console.log('王明的年龄更新了新值是' + newValue + ',旧值是' + oldValue)
      },
      immediate: true
    },
    'obj.brother.age': {
      handler (newValue, oldValue) {
        console.log('王刚的年龄更新了新值是' + newValue + ',旧值是' + oldValue)
      },
      immediate: true
    },
  }
测试发现，完全实现监听效果~~
在这里插入图片描述

结语：在使用watch时，如果需要监听对象的某具体属性变动，尽量使用字符串形式来优化监听！

还有一种方法是通过借助computed属性来搭桥，将对象的某个属性定义为一个computed属性，然后返回该对象的属性值，最后使用watch来监听该属性以实现监听对象属性的变化。但是我觉得这种方法，过于hack，也增加了代码的复杂度，就不在这里提倡了。


