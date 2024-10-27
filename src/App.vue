<script>
import { reactive } from 'vue';

class Option {
  constructor(parentName, name, value, isActive) {
    this.parentName = parentName;
    this.name = name;
    this.value = value;
    this.isActive = isActive;
  }
}

export default {
  setup() {
    const state = reactive({
      user: {
        isActive: true,
        name: '',
        email: '',
      },
      alias: {
        isActive: true,
        options: [
          // improvement: move to beforeCreate()
          new Option('alias', 'co', 'checkout', true),
          new Option('alias', 'com', 'checkout master', true),
          new Option('alias', 'ci', 'commit', true),
          new Option('alias', 'st', 'status', true),
          new Option('alias', 'br', 'branch', true),
          new Option('alias', 'hist', 'log --oneline --decorate --graph --date=short', true),
          new Option('alias', 'yaaas', 'push', true),
        ]
      }
    });

    return { state };
  },
};
</script>

<template>
  <main>
    <form>
      <section>
        <h3>user</h3>
        <ul>
          <li>
            <p>
              <label>Name</label>
              <input type="text" v-model="state.user.name">
            </p>
            <li>
            <p>
              <label>Email</label>
              <input type="text" v-model="state.user.email">
            </p>
          </li>
          </li>
        </ul>
      </section>

      <section>
        <h3>alias</h3>
        <ul>
          <li v-for="option in state.alias.options" :key="option.name">
            <label>
              <input type="checkbox" v-model="option.isActive">
              {{ option.name }} = {{ option.value }}
            </label>
          </li>
        </ul>
      </section>
    </form>

    <h1>your .gitconfig preview</h1>
    <pre>
      <code>
        <template v-if="state.user.isActive">
[user]
  name = {{ state.user.name }}
  email = {{ state.user.email }}
        </template>
        <template v-if="state.alias.isActive">
[alias]
          <template v-for="option in state.alias.options">
            <template v-if="option.isActive">
              {{ option.name }} = {{ option.value }}
            </template>
          </template>
        </template>
      </code>
    </pre>
  </main>
</template>