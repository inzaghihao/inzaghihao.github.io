---
title: VueTodos小记
date: 2017-04-05 10:34:12
tags: [vue, javascript,笔记]
---

经典的vue Todos 案例

#### 关于本地存储localStorage 存储和访问数据

检测浏览器是否支持

```
// Check browser support
if (typeof(Storage) !== "undefined") {
    //...
}else{
    // 抱歉! 不支持 web 存储。
}
```
- 保存数据：localStorage.setItem(key,value);
- 读取数据：localStorage.getItem(key);
- 删除单个数据：localStorage.removeItem(key);
- 删除所有数据：localStorage.clear();
- 得到某个索引的key：localStorage.key(index);

***提示: 键/值对通常以字符串存储，你可以按自己的需要转换该格式。***

- JSON.stringify 可以将对象转换为字符串。
- JSON.parse 方法将字符串转换为 JSON 对象

封装存取本地数据方法
```
var STORAGE_KEY = 'todos-vuejs-2.0'
var todoStorage = {
  fetch: function () {
    var todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
    todos.forEach(function (todo, index) {
      todo.id = index
    })
    todoStorage.uid = todos.length 		
    return todos
  },
  save: function (todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos))
  }
}
```
使用watch 深度监控todos已存入的数据变化，深度监控可以监控到数组以内对象的属性值，一旦数据发生改变，将最新数据存入到本地存储
```
watch: {
    todos: {
      handler: function (todos) {		
        todoStorage.save(todos)
      },
      deep: true
    }
},
```

#### 关于逻辑

在任务输入input表单绑定enter事件触发addTodo 方法，addTodo里首先把input输入v-model双向绑定的值得到并去除首尾空格处理，添加到todos数组中，id设置为每条自增，title对应value值，completed初始为false 未被选中状态，直接是数据驱动处理，最后把newTodo的值 设置为空，即input的value 也为空。
```
<input class="new-todo"
       autofocus="autofocus" autocomplete="off"
       placeholder="What needs to be done?"
       v-model="newTodo"
       @keyup.enter="addTodo" />

***
data: {
    todos: todoStorage.fetch(),
    newTodo: '',
    editedTodo: null,
    visibility: 'all'
},

addTodo: function () {
    var value = this.newTodo && this.newTodo.trim()   //如果this.newTodo 不为undefined。则执行返回&& 后的结果
    if (!value) {
      return
    }
    this.todos.push({
      id: todoStorage.uid++,
      title: value,
      : false
    })
    this.newTodo = ''
},
```
class为main的任务列表主体section 根据有数据的时候显示出来，包括footer 中需要有数据时进行条件筛选的按钮，v-for循环列表显示本地存储已有数据或者新增的数据。 动态绑定class值，根据条目的状态是否被选中或者是否处于被编辑状态增加相应的类名 :class={completed:true,editing: todo == editedTodo}
```
<section class="main" v-show="todos.length" v-cloak>

<ul class="todo-list">
  <li v-for="todo in filteredTodos"
    class="todo"
    :key="todo.id"
    :class="{ completed: todo.completed, editing: todo == editedTodo }">
    <div class="view">
      <input class="toggle" type="checkbox" v-model="todo.completed">
      <label @dblclick="editTodo(todo)">{{ todo.title }}</label>
      <button class="destroy" @click="removeTodo(todo)"></button>
    </div>
    <input class="edit" type="text"
      v-model="todo.title"
      v-todo-focus="todo == editedTodo"
      @blur="doneEdit(todo)"
      @keyup.enter="doneEdit(todo)"
      @keyup.esc="cancelEdit(todo)"/>
  </li>
</ul>
```
使用自定义指令 directives:{'todo-focus':function (el, binding) {}} 绑定双击label进行编辑条目,指令调用以v-开头 v-todo-focus="表达式"，表达式为true则执行，其中binding.value 是指表达式中的计算结果布尔值,el代表绑定当前的元素。

```
directives: {
    'todo-focus': function (el, binding) {
        if (binding.value) {
            el.focus()
        }
    }
}
```
当编辑时保存当前的title 值到一变量，方便取消修改之后的恢复。编辑完成之后enter键或者失去焦点时把editedTodo设置为空，lable和input可以根据类名进行切换，判断修改过后的内容去除首尾空格，并且如果修改成为空时，直接删除当前条目。如按esc编辑取消则把editedTodo设置为空并且把之前保存的title值还原

```
removeTodo: function (todo) {
  this.todos.splice(this.todos.indexOf(todo), 1)
},

editTodo: function (todo) {
  this.beforeEditCache = todo.title
  this.editedTodo = todo
},

doneEdit: function (todo) {
  if (!this.editedTodo) {
    return
  }
  this.editedTodo = null
  todo.title = todo.title.trim()
  if (!todo.title) {
    this.removeTodo(todo)
  }
},

cancelEdit: function (todo) {
  this.editedTodo = null
  todo.title = this.beforeEditCache
},
```
使用计算属性动态给模板元素赋值，根据数据变化发生计算值的改变computed:{remaining:function(){...}}

```
computed: {
    filteredTodos: function () {
      return filters[this.visibility](this.todos)
    },
    remaining: function () {
      return filters.active(this.todos).length
    },
    allDone: {
      get: function () {
        return this.remaining === 0
      },
      set: function (value) {
        this.todos.forEach(function (todo) {
          todo.completed = value
        })
      }
    }
},

filters: {
    pluralize: function (n) {
      return n === 1 ? 'item' : 'items'
    }
},
```

使用hash值 在window 上绑定hashchange 方法进行过滤筛选显示相对应的列表，全部和已完成和未完成，在data中声明visibility: 'all'，当页面发生hash变化时，首先判断是否是有效的hash值，有效则赋值给visibility，无效则为空，默认显示all

```
// handle routing
function onHashChange () {
  var visibility = window.location.hash.replace(/#\/?/, '')
  if (filters[visibility]) {
    app.visibility = visibility
  } else {
    window.location.hash = ''
    app.visibility = 'all'
  }
}
//绑定url改变事件
window.addEventListener('hashchange', onHashChange)
onHashChange()
```

列表循环计算属性的值，计算属性通过hash值对应的变化过滤返回对应的数据到filteredTodos 上，并在`<li v-for="todo in filteredTodos">`中循环
```
// visibility filters
var filters = {
  all: function (todos) {
    return todos
  },
  active: function (todos) {
    return todos.filter(function (todo) {
      return !todo.completed
    })
  },
  completed: function (todos) {
    return todos.filter(function (todo) {
      return todo.completed
    })
  }
}
computed: {
    filteredTodos: function () {
      return filters[this.visibility](this.todos)
    },
}
```

> 由于本人学识浅薄，记录的是自己所学和自己想法的理解，其中不乏浅显，切勿盲目转载，望查漏补缺~
