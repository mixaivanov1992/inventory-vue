<template>
  <div class="container" @drop="onDrop($event)" @dragover.prevent @dragenter.prevent>
    <div class="cell" :data-id="item.id" v-for="item in items" :key="item.id" @click="activeItem">
      <div :class="item.content" draggable="true" @dragstart="startDrag($event, item)"></div>
      <div>{{item.count}}</div>
    </div>
    <RightMenu v-if="active" :active-item="active" :close-right-menu="closeRightMenu" :count-change="countChange"/>
  </div>
</template>

<script>
import './Container.scss';
import RightMenu from './RightMenu/RightMenu.vue';

export default {
  name: 'ContainerComponent',
  data() {
    return {
      items: [
        {id: 1, content: 'content1', count: '55'},
        {id: 2, content: 'content2', count: '4'},
        {id: 3, content: 'content3', count: '10'},
        {id: 4, content: '', count: ''},
        {id: 5, content: '', count: ''},
        
        {id: 6, content: '', count: ''},
        {id: 7, content: '', count: ''},
        {id: 8, content: '', count: ''},
        {id: 9, content: '', count: ''},
        {id: 10, content: '', count: ''},
        
        {id: 11, content: '', count: ''},
        {id: 12, content: '', count: ''},
        {id: 13, content: '', count: ''},
        {id: 14, content: '', count: ''},
        {id: 15, content: '', count: ''},
        
        {id: 16, content: '', count: ''},
        {id: 17, content: '', count: ''},
        {id: 18, content: '', count: ''},
        {id: 19, content: '', count: ''},
        {id: 20, content: '', count: ''},
        
        {id: 21, content: '', count: ''},
        {id: 22, content: '', count: ''},
        {id: 23, content: '', count: ''},
        {id: 24, content: '', count: ''},
        {id: 25, content: '', count: ''},
      ],
      active: '',
    }
  },
  components: {
    RightMenu,
  },
  methods: {
    startDrag(e, item) {
      e.dataTransfer.dropEffect = 'move';
      e.dataTransfer.effectAllowed = 'move';
      e.dataTransfer.setData('itemID', item.id);
      
      const parent = e.target.parentElement.cloneNode(true);
      parent.classList.add('dragging');
      parent.style.position = 'fixed'; parent.style.top = '1000px'; parent.style.right = '1000px';
      document.body.appendChild(parent);
      e.dataTransfer.setDragImage(parent, 75, 75);
    },
    onDrop(e) {
      const itemID = +e.dataTransfer.getData('itemID'),
            changeableID = +e.target.dataset.id;

      if(changeableID){
        const changeableItem = this.items.find((item) => item.id === changeableID);
        if(!changeableItem.content){
          const item = this.items.find((item) => item.id === itemID);
          const {content, count} = item;
          item.content = '';
          item.count = '';
          
          changeableItem.content = content;
          changeableItem.count = count;
        }
      }
    },
    activeItem(e){
      const id = +e.currentTarget.dataset.id;
      const item = this.items.find((item) => item.id === id);
      if(item.content){
        e.currentTarget.classList.add('active');
        this.active = this.items.find((item) => item.id === id)?.content || '';
      }
    },
    closeRightMenu(){
      const cell = document.querySelector('.cell.active');
      cell.classList.remove('active');
      this.active = '';
    },
    countChange(content, count){
      const item = this.items.find((item) => item.content === content);
      item.count = +item.count < +count ? 0 : item.count - count;
    }
  },
  beforeMount() {
    if (localStorage.getItem('items') !== null) {
      this.items = JSON.parse(
        localStorage.getItem('items')
      );
    }
  },
  watch: {
    'items': {
        handler: function () {
          localStorage.setItem('items', JSON.stringify(this.items));
        },
        deep: true,
    },
  },
}
</script>
