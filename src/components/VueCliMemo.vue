<template>
  <div>
    <ul>
      <li v-for="memo in memos" :key="memo">
        <span>{{ memo.split('\n')[0] }}</span>
      </li>
    </ul>

    <div @click="displayForm">＋</div>
    <div v-if="isAddable">
      <textarea v-model="newMemo"></textarea>
      <div>
        <button @click="addMemo">Add</button>
        <button @click="cancel">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      newMemo: '',
      memos: [],
      isAddable: false,
    }
  },
  mounted () {
    this.memos = JSON.parse(localStorage.getItem('memos')) || [];
  },
  methods: {
    displayForm(){
      this.isAddable = true
      this.newMemo = ''
    },
    addMemo () {
      this.memos.push(this.newMemo)
      this.saveMemo()
    },
    saveMemo () {
      localStorage.setItem('memos', JSON.stringify(this.memos))
      this.cancel()
    },
    cancel () {
      this.isAddable = false
      this.newMemo = ''
    }
  },
}
</script>
