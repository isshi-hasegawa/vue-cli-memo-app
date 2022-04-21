<template>
  <div>
    <ul>
      <li v-for="(memo, index) in memos" :key="memo.id" @click="displayForm(index)">
        <span>{{ memo.content.split('\n')[0] }}</span>
      </li>
    </ul>

    <div @click="displayForm">＋</div>
    <div v-if="formFlags === 1 || formFlags === 2">
      <textarea v-model="newMemo"></textarea>
      <div>
        <button v-if="formFlags === 1" @click="addMemo">Add</button>
        <span v-if="formFlags === 2">
          <button @click="updateMemo">Update</button>
          <button @click="deleteMemo">Delete</button>
        </span>
        <button @click="clearMemo">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
const ADDING = 1
const EDITING = 2

export default {
  data () {
    return {
      newMemo: '',
      memos: [],
      formFlags: 0,
      editingMemoIndex: undefined,
    }
  },
  mounted () {
    this.memos = JSON.parse(localStorage.getItem('memos')) || []
  },
  methods: {
    displayForm(index){
      if(!isNaN(index)){
        this.formFlags = EDITING
        this.newMemo = this.memos[index].content
        this.editingMemoIndex = index
      } else {
        this.formFlags = ADDING
        this.newMemo = ''
      }
    },
    addMemo () {
      if (this.newMemo === '') return
      const uuid = new Date().getTime().toString(16) + Math.floor(Math.random()).toString(16)
      const memo = {
        id: uuid,
        content: this.newMemo,
      }
      this.memos.push(memo)
      this.saveMemo()
    },
    updateMemo (){
      if (this.newMemo === '') return
      this.memos[this.editingMemoIndex].content = this.newMemo
      this.saveMemo()
    },
    deleteMemo (){
      this.memos.splice(this.editingMemoIndex, 1)
      this.saveMemo()
    },
    saveMemo () {
      localStorage.setItem('memos', JSON.stringify(this.memos))
      this.clearMemo()
    },
    clearMemo () {
      this.formFlags = 0
      this.newMemo = ''
    }
  },
}
</script>
