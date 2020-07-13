<template>
  <div id="app">
    <table>
      <th>
        <td>Prop</td>
        <td>Comment</td>
      </th>
      <tr>
        <td>value</td>
        <td>Prop for v-model</td>
      </tr>
      <tr>
        <td>type</td>
        <td>Default: text.</td>
      </tr>
      <tr>
        <td>name</td>
        <td>Required. To connect label and input via id</td>
      </tr>
      <tr>
        <td>Placeholder</td>
        <td></td>
      </tr>
      <tr>
        <td>label</td>
        <td>Required. In case of animated label placeholder will be the same as label</td>
      </tr>
      <tr>
        <td>is-multiline</td>
        <td>Default: false. Turn this to true to display textarea instead of input</td>
      </tr>
      <tr>
        <td>rows</td>
        <td>Default: 2. Textarea rows</td>
      </tr>
      <tr>
        <td>error</td>
        <td>Bind this to error message. Error clearing should be handled at the parent component. If you want your error to disappear on users input, I'd suggest to use @input="inputError = ''"' where inputError is variable for error in parent component</td>
      </tr>
      <tr>
        <td>theme</td>
        <td>Color scheme. Themes available: light (default), dark, grey</td>
      </tr>
      <tr>
        <td>is-label-animated</td>
        <td>Default: false. Turn it to true if you want your input look like email input in google account sign in form</td>
      </tr>
    </table>
    <div class="row">
      <div class="col light">
        <EInput
          v-model="testInput"
          :error="inputError"
          placeholder="Input test placeholder"
          name="test"
          label="Organization"
          @input="inputError = ''"
        />
      </div>
      <div  class="col dark">
        <EInput
          v-model="testInput"
          :error="inputError"
          placeholder="Yet another placeholder"
          theme="dark"
          name="test1"
          label="Name"
          @input="inputError = ''"
        />
      </div>
      <div class="col grey">
        <EInput
          v-model="testInput"
          :error="inputError"
          theme="gray"
          name="test2"
          label="Position"
          @input="inputError = ''"
        />
      </div>
    </div>
    <div class="row">
      <div class="col light">
        <EInput
          v-model="testInput"
          :error="inputError"
          theme="light"
          name="test3"
          label="Email"
          is-label-animated
          @input="inputError = ''"
        />
      </div>
      <div class="col dark">
        <EInput
          v-model="testInput"
          :error="inputError"
          theme="dark"
          name="test4"
          label="Password"
          is-label-animated
          @input="inputError = ''"
        />
      </div>
      <div class="col grey">
        <EInput
          v-model="testInput"
          :error="inputError"
          theme="gray"
          name="test5"
          label="Login"
          placeholder="Type you login here"
          is-label-animated
          @input="inputError = ''"
        />
      </div>
    </div>
    <button @click="inputError = 'Error test message'">
      Error test
    </button>
  </div>
</template>

<script>
import EInput from './components/e-input';

export default {
  name: 'App',
  components: {
    EInput,
  },

  data: () => ({
    testInput: '',
    inputError: '',
  }),
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-size: 16px;
  line-height: 1.5;
  margin-top: 60px;
  box-sizing: border-box;
}

.row {
  display: flex;
  margin-top: 30px;
}

.col {
  padding: 30px;
  margin: 0 20px;
  width: 100%;
}

.dark {
  background-color: #090B0C;
}

.grey {
  background-color: #212529;
}

table {
  border-collapse: collapse;
}

td {
  border: 1px solid #333;
  padding: 2px 10px;
}
</style>
