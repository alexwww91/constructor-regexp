<template>
  <div class="tools">
      <h2>Основные переменные</h2>
      <button class="tool" v-for="tool in tools" :key="tool.id" @click="addToResult(tool)">{{ tool.name }}</button>
      <div class="options" v-show="showOptions">
        <button class="tool" v-for="option in options" :key="option.id" @click="addToResult(option)">{{option.name}}</button>
      </div>
      <div v-if="errore" class="errore">{{errore}}</div>
  </div>
  <div class="methods">
      <h2>Группировки</h2>
      <button class="method" v-for="method in methods" :key="method.id" @click="addToResult(method)">{{ method.name }}</button>
  </div>
  <div class="repeats">
      <h2>Повторения</h2>
      <button class="repeat" v-for="repeat in repeats" :key="repeat.id" @click="addToResult(repeat)">{{ repeat.name }}</button>
  </div>
  
  <div v-if="visualResults.length">
    <div class="visual-results" ref="myid">
      <div class="visual-result" v-for="visualResult in visualResults" :key="visualResult.id" :style="{ color: visualResult.color, borderColor: visualResult.color }">
        {{visualResult.name}}
        <div class="visual-result__close" :style="{ borderColor: visualResult.color }" @click="delToResult(visualResult)"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
      return {
          tools: [
              {name: 'Любой символ', content: '.', color: '#1b3c64', type: 'tool'},
              {name: 'Любая цифра', content: '\d', color: '#1b3c64', type: 'tool'},
              {name: 'Любой сивмвол, кроме цифры', content: '\D', color: '#1b3c64', type: 'tool'},
              {name: 'Любая буква', options: [
                      {name: 'Любая буква', content: '\w', color: '#1b3c64', type: 'tool'},
                      {name: 'Любая латинская буква', content: '[a-zA-Z]', color: '#1b3c64', type: 'tool'},
                      {name: 'Любая кириллическая буква', content: '[а-яА-Я]', color: '#1b3c64', type: 'tool'},
                      {name: 'Любая заглавная латинская буква', content: '[A-Z]', color: '#1b3c64', type: 'tool'},
                      {name: 'Любая заглавная кириллическая буква', content: '[А-Я]', color: '#1b3c64', type: 'tool'},
                      {name: 'Любая строчная латинская буква', content: '[a-z]', color: '#1b3c64', type: 'tool'},
                      {name: 'Любая строчная кириллическая буква', content: '[а-я]', color: '#1b3c64', type: 'tool'},
                  ]
              },
              {name: 'Любой символ, кроме буквы', options: [
                      {name: 'Любой сивмвол, кроме любой буквы', content: '\W', color: '#1b3c64', type: 'tool'},
                      {name: 'Любой сивмвол, кроме латинской буквы', content: '[^a-zA-Z]', color: '#1b3c64', type: 'tool'},
                      {name: 'Любой сивмвол, кроме кириллической буквы', content: '[^а-яА-Я]', color: '#1b3c64', type: 'tool'},
                      {name: 'Любой сивмвол, кроме заглавной латинской буквы', content: '[^A-Z]', color: '#1b3c64', type: 'tool'},
                      {name: 'Любой сивмвол, кроме заглавной кириллической буквы', content: '[^А-Я]', color: '#1b3c64', type: 'tool'},
                      {name: 'Любой сивмвол, кроме строчной латинской буквы', content: '[^a-z]', color: '#1b3c64', type: 'tool'},
                      {name: 'Любой сивмвол, кроме строчной кириллической буквы', content: '[^а-я]', color: '#1b3c64', type: 'tool'},
                  ]
              },
              {name: 'Пробел', options: [
                      {name: 'Пробел', content: '\s', color: '#1b3c64', type: 'tool'},
                      {name: 'Не пробел', content: '\S', color: '#1b3c64', type: 'tool'},
                  ]
              },
              {name: 'Граница слова', options: [
                      {name: 'Граница слова', content: '\b', color: '#1b3c64', type: 'tool'},
                      {name: 'Что угодно, кроме границы слова', content: '\B', color: '#1b3c64', type: 'tool'},
                  ]},
              {name: 'Начало/конец строки', options: [
                      {name: 'Начало строки', content: '^', color: '#1b3c64', type: 'tool'},
                      {name: 'Конец строки', content: '$', color: '#1b3c64', type: 'tool'},
                  ]
              },
          ],
          methods: [
              {name: 'Группировка открытие', content: '(', color: '#ff5c5c', type: 'method'},
              {name: 'Группировка закрытие', content: ')', color: '#ff5c5c', type: 'method'},
              {name: 'Диапазон открытие', content: '[', color: '#ff5c5c', type: 'method-range'},
              {name: 'Диапазон закрытие', content: ']', color: '#ff5c5c', type: 'method-range'},
          ],
          repeats: [
              {name: 'Повторяется от "может и не быть" до бесконечности', content: '*', color: '#29A366', type: 'repeat'},
              {name: 'Повторяется от "может и не быть" до 1 раза', content: '?', color: '#29A366', type: 'repeat'},
              {name: 'Повторяется от "минимум 1 раз" до бесконечности', content: '+', color: '#29A366', type: 'repeat'},
              {name: 'Повторяется ввести точное значение повторений', color: '#29A366', type: 'repeat'},
              {name: 'Повторяется ввести диапазон повторений', color: '#29A366', type: 'repeat'},
          ],
          showOptions: false,
          options: [],
          visualResults: [],
          results: '',
          errore: '',
      }
  },
  methods: {
      addToResult(elem) {
          if (elem.options) {
              this.showOptions = true;
              this.options = elem.options;
          } else {
              let addedElement = Object.assign({}, elem); //делаю копирование, потому что если по ссылке добавлять elem.id, то в visualResults предыдущие запушенные тоже будут меняться из-за того что ссылка)
              addedElement.id = Date.now();
              this.visualResults.push(addedElement);
              this.showOptions = false;
              this.options = [];
              this.errore = '';
              let block = this.$refs.myid;
              if (block == undefined) {
                return;
              }else{
                setTimeout(() => block.scrollLeft = 9999999, 1);
              }
          }
      },
      delToResult(elem){
        this.visualResults = this.visualResults.filter(e => e.id !== elem.id);
      }
  },
}
</script>

<style>
  * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: Rubik;
  }
  button {
    border: none;
    outline: none;
    background: none;
    text-decoration: none;
    font-size: 16px;
  }
  .tools{
    min-height: 133px;
  }
  .tool, .method, .repeat {
    margin: 15px 5px;
    color: #1b3c64;
    padding: 3px 9px;
    border: 2px solid #E5E5E5;
    border-radius: 5px;
    cursor: pointer;
  }
  .tool:hover {
    border-color: #1b3c64;
  }
  .options, .visual-results {
    overflow-x: auto;
    white-space: nowrap;
  }
  .options::-webkit-scrollbar {
    height: 12px;
    padding: 0 5px;
  }
  .options::-webkit-scrollbar-track {
    background: #d3d3d345;
  }
  .options::-webkit-scrollbar-thumb {
    background-color: lightgray;
    border-radius: 20px;
  }
  .visual-results {
    background-color: #d3d3d345;
    border-radius: 10px;
    padding: 15px 10px;
  }
  .visual-results::-webkit-scrollbar {
    height: 12px;
    padding: 0 5px;
  }
  .visual-results::-webkit-scrollbar-track {
    background: #d3d3d345;
  }
  .visual-results::-webkit-scrollbar-thumb {
    background-color: lightgray;
    border-radius: 20px;
  }
  .visual-result {
    display: inline-block;
    position: relative;
    padding: 3px 9px;
    margin: 0 5px;
    background-color: white;
    border: 2px solid #E5E5E5;
    border-radius: 5px;
    width: auto;
    cursor: default;
  }
  .visual-result:hover .visual-result__close {
    display: block;
  } 
  .visual-result__close {
    display: none;
    position: absolute;
    top: -10px;
    right: -10px;
    width: 20px;
    height: 20px;
    background-color: black;
    background-image: url(@/assets/free-icon-cross-mark.png);
    background-position: center;
    border-radius: 10px;
    border: 2px solid #E5E5E5;
    cursor: pointer;
    z-index: 1;
  }
  .visual-result__close:hover {
    box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2);
  }
</style>