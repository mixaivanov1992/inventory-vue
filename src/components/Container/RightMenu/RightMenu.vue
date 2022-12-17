<template>
  <div class="right_menu">
    <div class="panel">
      <div class="close" @click="closeRightMenu"></div>
      <div :class="activeItem"></div>
      <div class="child1"></div>
      <div class="child2"></div>
      <div class="child3"></div>
      <div class="child4"></div>
      <div class="child5"></div>
      <div class="child6"></div>
      <div class="child7"></div>
      <div class="child8"></div>
      <button class="remove" type="button" @click="changeVisibility">Удалить предмет</button>
      <div v-if="showConfirm" class="wrapper">
        <input type="text" class="counter" placeholder="Введите количество" :value="count" @input="changeCount" />
        <button class="cancel" type="button" @click="changeVisibility">Отмена</button>
        <button class="confirm" type="button" @click="confirm">Подтвердить</button>
      </div>
    </div>
  </div>
</template>

<script>
import './RightMenu.scss';

export default {
  name: 'RightMenu',
  data() {
    return {
      showConfirm: false,
      count: '',
    }
  },
  props: {
    activeItem:{
      type: String,
      required: true
    },
    closeRightMenu:{
      type: Function,
      required: true
    },
    countChange:{
      type: Function,
      required: true
    }
  },
  methods: {
    changeVisibility(){
      this.showConfirm = !this.showConfirm;
    },
    changeCount(e){
      const value = e.target.value.trim();
      if(Number.isInteger(+value) || value === ''){
        this.count = value;
      }else{
        e.target.value = this.count;
      }
    },
    confirm(){
      if(this.count){
        this.showConfirm = !this.showConfirm;
        this.countChange(this.activeItem, this.count);
        this.count = '';
      }
    }
  }
}
</script>
