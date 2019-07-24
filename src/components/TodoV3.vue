<template>
  <div>
    <p>{{ vTitle }}</p>

    <div class="my-3 text-primary">
      <p>{{ append }}</p>
    </div>
    
    <div class="form-group d-flex">
      <input v-model="todo" type="text" class="form-control" />
      <button class="btn btn-primary" @click="add">Add</button>
    </div>
    <p>[TodoLength: {{ todoLength }}, Items quantity: {{ itemsQuantity }}]</p>

    <div class="list-group">
      <div
        class="list-group-item d-flex justify-content-between"
        v-for="(item,index) in items"
        :key="index"
      >
        <span>{{ item }}</span>
        <button class="close" @click="remove(index)">
          <span>&times;</span>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { value, computed, watch, onCreated, onMounted } from 'vue-function-api';

export default {
  props: {
    title: String,
    initInput: String
  },
  setup(props) {
    const vTitle = computed(() => '-' + props.title + '-');

    const todo = value(props.initInput);
    const todoLength = value(0);

    const items = value(['This', 'is']);
    const itemsQuantity = computed(() => items.value.length);
    const append = value('');

    watch(
      // getter
      [
        () => todo.value.length,
        () => items.value
      ],
      // callback
      ([length, items], [oldLength, oldItems]) => {
        // todo
        todoLength.value = length;

        // items
        append.value = '';
        items.forEach(item => {
          append.value += item + ' ';
        });
      },
      // watch Options
      {
        lazy: false // immediate: true
      }
    )

    /*
    watch(
      // getter
      () => items.value,
      // callback
      (items, oldItems) => {
        append.value = '';
        items.forEach(item => {
          append.value += item + ' ';
        });
      },
      // watch Options
      {
        lazy: false // immediate: true
      }
    )

    watch(
      // getter
      () => todo.value.length,
      // callback
      (length, oldLength) => {
        // todo
        todoLength.value = length;
      },
      // watch Options
      {
        lazy: true // immediate: false
      }
    )
    */

    const add = () => {
      if (todo.value) {
        items.value.push(todo.value);
        todo.value = '';
      }
    };

    const remove = index => {
      items.value.splice(index, 1);
    };

    onCreated(() => {
      console.log('V3 created!');
    })

    onMounted(() => {
      console.log('V3 mounted!');
    })

    return {
      vTitle,
      todo,
      todoLength,
      items,
      itemsQuantity,
      append,
      add,
      remove
    };
  }
};
</script>
