## Vue.js

### Declarative Rendering

```Vue
<script>
export default {
    data() {
        return {
            message: 'Hello World!'
        }
    }
}
</script>

<template>
    <h1>{{ message }}</h1>
</template>
```

### Attribute Bindings

```Vue
<script>
export default {
    data() {
        return {
            titleClass: 'title'
        }
    }
}
</script>

<template>
    <h1 :class="titleClass">Make me red</h1>
</template>

<style>
    .title {
        color: red;
    }
</style>
```

### Event Listeners

```Vue
<script>
export default {
    data() {
        return {
            count: 0
        }
    },
    methods: {
        increment() {
            this.count++
        }
    }
}
</script>

<template>
    <button @click="increment">count is: {{ count }}</button>
</template>
```

### Form Bindings

v-model automatically syncs the <input>'s value with the bound state, so we no longer need to use an event handler for that. It works not only on text inputs, but also other input types such as checkboxes, radio buttons, and select dropdowns.

```Vue
<script>
export default {
    data() {
        return {
        text: ''
        }
    }
}
</script>

<template>
    <input v-model="text" placeholder="Type here">
    <p>{{ text }}</p>
</template>
```

### Conditional Rendering

This <h1> will be rendered only if the value of awesome is truthy. If awesome changes to a falsy value, it will be removed from the DOM.

```Vue
<script>
export default {
    data() {
        return {
            awesome: true
        }
    },
    methods: {
        toggle() {
        this.awesome = !this.awesome
        }
    }
}
</script>

<template>
    <button @click="toggle">toggle</button>
    <h1 v-if="awesome">Vue is awesome!</h1>
    <h1 v-else>Oh no 😢</h1>
</template>
```

### List Rendering

```Vue
<script>
// give each todo a unique id
let id = 0

export default {
    data() {
        return {
            newTodo: '',
            todos: [
                { id: id++, text: 'Learn HTML' },
                { id: id++, text: 'Learn JavaScript' },
                { id: id++, text: 'Learn Vue' }
            ]
        }
    },
    methods: {
        addTodo() {
            this.todos.push({ id: id++, text: this.newTodo })
            this.newTodo = ''
        },
        removeTodo(todo) {
            this.todos = this.todos.filter((t) => t !== todo)
        }
    }
}
</script>
    
<template>
    <form @submit.prevent="addTodo">
        <input v-model="newTodo">
        <button>Add Todo</button>
    </form>
    <ul>
        <li v-for="todo in todos" :key="todo.id">
            {{ todo.text }}
            <button @click="removeTodo(todo)">X</button>
        </li>
    </ul>
</template>
```

### Computed Property

```Vue
<script>
let id = 0

export default {
    data() {
        return {
            newTodo: '',
            hideCompleted: false,
            todos: [
                { id: id++, text: 'Learn HTML', done: true },
                { id: id++, text: 'Learn JavaScript', done: true },
                { id: id++, text: 'Learn Vue', done: false }
            ]
        }
    },
    computed: {
        filteredTodos() {
            return this.hideCompleted
                ? this.todos.filter((t) => !t.done)
                : this.todos
        }
    },
    methods: {
        addTodo() {
            this.todos.push({ id: id++, text: this.newTodo, done: false })
            this.newTodo = ''
        },
        removeTodo(todo) {
            this.todos = this.todos.filter((t) => t !== todo)
        }
    }
}
</script>
    
<template>
    <form @submit.prevent="addTodo">
        <input v-model="newTodo">
        <button>Add Todo</button>
    </form>
    <ul>
        <li v-for="todo in filteredTodos" :key="todo.id">
            <input type="checkbox" v-model="todo.done">
            <span :class="{ done: todo.done }">{{ todo.text }}</span>
            <button @click="removeTodo(todo)">X</button>
        </li>
    </ul>
    <button @click="hideCompleted = !hideCompleted">
        {{ hideCompleted ? 'Show all' : 'Hide completed' }}
    </button>
</template>
    
<style>
.done {
    text-decoration: line-through;
}
</style>
```

### Lifecycle and Template Refs

We can request a template ref - i.e. a reference to an element in the template - using the special ref attribute:

```Vue
<p ref="p">hello</p>
```

The element will be exposed on this.$refs as this.$refs.p. However, you can only access it after the component is mounted.

```Vue
<script>
export default {
    mounted() {
        this.$refs.p.textContent = 'mounted!'
    }
}
</script>

<template>
    <p ref="p">hello</p>
</template>
```

### Watchers

```Vue
<script>
export default {
    data() {
        return {
            todoId: 1,
            todoData: null
        }
    },
    methods: {
        async fetchData() {
            this.todoData = null
            const res = await fetch(
                `https://jsonplaceholder.typicode.com/todos/${this.todoId}`
            )
            this.todoData = await res.json()
        }
    },
    mounted() {
        this.fetchData()
    },
    watch: {
        todoId() {
            this.fetchData()
        }
    }
}
</script>
    
<template>
    <p>Todo id: {{ todoId }}</p>
    <button @click="todoId++">Fetch next todo</button>
    <p v-if="!todoData">Loading...</p>
    <pre v-else>{{ todoData }}</pre>
</template>
```

### Components

```Vue
<!-- App.vue -->
<script>
import ChildComp from './ChildComp.vue'

export default {
    components: {
        ChildComp,
    }
}
</script>
    
<template>
    <ChildComp></ChildComp>
</template>
```

```Vue
<!-- ChildComp.vue -->
<template>
    <h2>A Child Component!</h2>
</template>
```

### Props

```Vue
<!-- App.vue -->
<script>
import ChildComp from './ChildComp.vue'

export default {
    components: {
        ChildComp
    },
    data() {
        return {
            greeting: 'Hello from parent'
        }
    }
}
</script>
    
<template>
    <ChildComp :msg="greeting" />
</template>
```

```Vue
<!-- ChildComp.vue -->
<script>
export default {
    props: {
        msg: String
    }
}
</script>
    
<template>
    <h2>{{ msg || 'No props passed yet' }}</h2>
</template>
```