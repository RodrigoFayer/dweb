<template>
  <div class="p-10 flex flex-col gap-2 bg-slate-800" style="height: 100vh">
    <div class="w-full justify-end">
      <button class="border bg-slate-500 border-slate-800 text-white rounded-lg p-2" @click="showModal = true">Add To-do</button>
    </div>
    <div class="grid grid-cols-3 gap-3">
      <div class="bg-slate-600 text-white px-2 py-2">
        <span class="pl-1 pr-1.5">To-dos ({{ toDos.length }})</span>
        <draggable
            class="dragArea list-group rounded-xl min-h-full"
            :list="toDos"
            group="toDos"
            item-key="id"
        >
          <template #item="{ element }">
            <div @click="() => edit(element.id, 'toDos')" class="list-group-item flex flex-col gap-0.5 bg-slate-800 p-2 my-2">
              <span>{{element.title}}</span>
              <span>{{element.personInCharge}}</span>
            </div>
          </template>
        </draggable>
      </div>
      <div class="bg-green-600 text-white px-2 py-2">
        <span class="pl-1 pr-1.5">In progress ({{ inProgress.length }})</span>
        <draggable
            class="dragArea list-group rounded-xl min-h-full"
            :list="inProgress"
            group="toDos"
            item-key="id"
        >
          <template #item="{ element }">
            <div @click="() => edit(element.id, 'inProgress')" class="list-group-item flex flex-col gap-0.5 bg-slate-800 p-2 my-2">
              <span>{{element.title}}</span>
              <span>{{element.personInCharge}}</span>
            </div>
          </template>
        </draggable>
      </div>
      <div class="bg-purple-600 text-white px-2 py-2">
        <span class="pl-1 pr-1.5">Finished ({{ finished.length }})</span>
        <draggable
            class="dragArea list-group rounded-xl min-h-full"
            :list="finished"
            group="toDos"
            item-key="id"
        >
          <template #item="{ element }">
            <div @click="() => edit(element.id, 'finished')" class="list-group-item flex flex-col gap-0.5 bg-slate-800 p-2 my-2">
              <span>{{element.title}}</span>
              <span>{{element.personInCharge}}</span>
            </div>
          </template>
        </draggable>
      </div>
    </div>
  </div>

  <Modal v-model="showModal">
    <template v-slot:title>Add new to-do</template>
    <template v-slot:content>
      <form class="flex flex-col gap-2 my-5 w-96">
        <div class="w-full flex items-center gap-2">
          <label class="flex-grow text-xl" for="title">Title</label>
          <input v-model="title" class="bg-gray-900 text-white focus:outline-none" type="text" name="title" id="title" placeholder="Title">
        </div>
        <div class="w-full flex items-center gap-2">
          <label class="flex-grow text-xl" for="personInCharge">Person In Charge</label>
          <input v-model="personInCharge" class="bg-gray-900 text-white focus:outline-none" type="text" name="personInCharge" id="personInCharge" placeholder="Person in Charge">
        </div>
      </form>
    </template>
    <template v-slot:footer>
      <div class="flex justify-start items-center pt-4 gap-2">
        <button class="border border-white text-gray-900 bg-white rounded-lg p-2" @click="confirm">Confirm</button>
        <button class="border border-red-600 text-red-600 rounded-lg p-2" @click="cancel">Cancel</button>
        <button v-if="currentId !== null && currentList !== null" class="border-red-600 bg-red-600 text-white rounded-lg p-2" @click="deleteItem">Delete</button>
      </div>
    </template>
  </Modal>

</template>

<script>
import draggable from "vuedraggable";
import Modal from "../Components/Modal.vue";

export default {
  name: "Show",
  components: {Modal , draggable},
  data() {
    return {
      currentId: null,
      currentList: null,
      title: "",
      personInCharge: "",
      dragging: false,
      toDos: [
        {id: 1, title: "Teste1", personInCharge: "Rodrigo Fayer", status: "toDos"},
        {id: 2, title: "Teste2", personInCharge: "Rodrigo Fayer", status: "toDos"},
      ],
      inProgress: [
        {id: 3, title: "Teste3", personInCharge: "Rodrigo Fayer", status: "inProgress"},
        {id: 4, title: "Teste4", personInCharge: "Rodrigo Fayer", status: "inProgress"},
      ],
      finished: [
        {id: 5, title: "Teste5", personInCharge: "Rodrigo Fayer", status: "finished"},
        {id: 6, title: "Teste6", personInCharge: "Rodrigo Fayer", status: "finished"}
      ],
      showModal: false
    }
  },
  computed:{
  },
  methods: {
    edit(id, key){
      const list = this.$data[key]
      const element = list.filter(item => item.id === id)

      this.title = element[0].title
      this.personInCharge = element[0].personInCharge
      this.currentId = id
      this.currentList = key
      this.showModal = true
    },
    confirm() {
      if (this.currentId === null && this.currentList === null) {
        this.toDos.push({id: 7, title: this.title, personInCharge: this.personInCharge})
      }else if(this.currentId !== null && this.currentList !== null) {
        const list = this.$data[this.currentList]
        const element = list.filter(item => item.id === this.currentId)

        element[0].title = this.title
        element[0].personInCharge = this.personInCharge
      }
      this.currentId = null
      this.currentList = null
      this.title = ""
      this.personInCharge = ""
      this.showModal = false
    },
    deleteItem(){
      const list = this.$data[this.currentList]
      const index = list.findIndex(item => item.id === this.currentId)

      list.splice(index,1)
      this.currentId = null
      this.currentList = null
      this.title = ""
      this.personInCharge = ""
      this.showModal = false
    },
    cancel() {
      this.title = ""
      this.personInCharge = ""
      this.currentId = null
      this.currentList = null
      this.showModal = false
    }
  }
}
</script>