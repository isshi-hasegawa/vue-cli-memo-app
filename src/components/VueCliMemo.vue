<template>
  <div>
    <ul>
      <li v-for="(memo, index) in memos" :key="memo" @click="displayForm(index)">
        <span>{{ memo.split('\n')[0] }}</span>
      </li>
    </ul>

    <div @click="displayForm">＋</div>
    <div v-if="isAddable || isEditable">
      <textarea v-model="newMemo"></textarea>
      <div>
        <button v-if="isAddable && !isEditable" @click="addMemo">Add</button>
        <span v-if="!isAddable && isEditable">
          <button @click="updateMemo">Update</button>
          <button @click="deleteMemo">Delete</button>
        </span>
        <button @click="clearMemo">Cancel</button>
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
      isEditable: false,
      editingMemoIndex: undefined,
    }
  },
  mounted () {
    this.memos = JSON.parse(localStorage.getItem('memos')) || []
  },
  methods: {
    displayForm(index){
      if(!isNaN(index)){
        this.isAddable = false
        this.isEditable = true
        this.newMemo = this.memos[index]
        this.editingMemoIndex = index
      } else {
        this.isAddable = true
        this.isEditable = false
        this.newMemo = ''
      }
    },
    addMemo () {
      if (this.newMemo === '') return
      this.memos.push(this.newMemo)
      this.saveMemo()
    },
    updateMemo (){
      if (this.newMemo === '') return
      this.memos.splice(this.editingMemoIndex, 1, this.newMemo)
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
      this.isAddable = false
      this.isEditable = false
      this.newMemo = ''
    }
  },
}
</script>
