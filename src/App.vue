<template>
  <div>
      <button class="tool" v-for="tool in tools" :key="tool.id" @click="addToResult(tool)">{{ tool.name }}</button>
  </div>
  <div v-show="showOptions">
      <button class="tool" v-for="option in options" :key="option.id" @click="addToResult(option)">{{option.name}}</button>
  </div>
  <div v-if="visualResults">
      <div class="visual-result" v-for="visualResult in visualResults" :key="visualResult.id" :style="{ color: visualResult.color, borderColor: visualResult.color }">
        {{visualResult.name}}
        <div class="visual-result__close" :style="{ borderColor: visualResult.color }" @click="delToResult(visualResult)"></div>
      </div>
  </div>
</template>

<script>
export default {
  data() {
      return {
          tools: [
              {id: 1, name: 'Любой символ', content: '.', color: '#1b3c64'},
              {id: 2, name: 'Любая цифра', content: '\d', color: '#1b3c64'},
              {id: 3, name: 'Любой сивмвол, кроме цифры', content: '\D', color: '#1b3c64'},
              {id: 4, name: 'Любая буква', options: [
                      {id: 1, name: 'Любая буква', content: '\w', color: '#1b3c64'},
                      {id: 2, name: 'Любая латинская буква', content: '[a-zA-Z]', color: '#1b3c64'},
                      {id: 3, name: 'Любая кириллическая буква', content: '[а-яА-Я]', color: '#1b3c64'},
                      {id: 3, ame: 'Любая заглавная латинская буква', content: '[A-Z]', color: '#1b3c64'},
                      {id: 5, name: 'Любая заглавная кириллическая буква', content: '[А-Я]', color: '#1b3c64'},
                      {id: 6, name: 'Любая строчная латинская буква', content: '[a-z]', color: '#1b3c64'},
                      {id: 7, name: 'Любая строчная кириллическая буква', content: '[а-я]', color: '#1b3c64'},
                  ]
              },
              {name: 'Любой символ, кроме буквы', options: [
                      {name: 'Любой сивмвол, кроме любой буквы', content: '\W', color: '#1b3c64'},
                      {name: 'Любой сивмвол, кроме латинской буквы', content: '[^a-zA-Z]', color: '#1b3c64'},
                      {name: 'Любой сивмвол, кроме кириллической буквы', content: '[^а-яА-Я]', color: '#1b3c64'},
                      {name: 'Любой сивмвол, кроме заглавной латинской буквы', content: '[^A-Z]', color: '#1b3c64'},
                      {name: 'Любой сивмвол, кроме заглавной кириллической буквы', content: '[^А-Я]', color: '#1b3c64'},
                      {name: 'Любой сивмвол, кроме строчной латинской буквы', content: '[^a-z]', color: '#1b3c64'},
                      {name: 'Любой сивмвол, кроме строчной кириллической буквы', content: '[^а-я]', color: '#1b3c64'},
                  ]
              },
              {name: 'Пробел', options: [
                      {name: 'Пробел', content: '\s', color: '#1b3c64'},
                      {name: 'Не пробел', content: '\S', color: '#1b3c64'},
                  ]
              },
              {name: 'Граница слова', options: [
                      {name: 'Граница слова', content: '\b', color: '#1b3c64'},
                      {name: 'Что угодно, кроме границы слова', content: '\B', color: '#1b3c64'},
                  ]},
              {name: 'Начало/конец строки', options: [
                      {name: 'Начало строки', content: '^', color: '#1b3c64'},
                      {name: 'Конец строки', content: '$', color: '#1b3c64'},
                  ]
              },
          ],
          methods: [
              {name: 'Группировка открытие', content: '(', color: '#ff5c5c'},
              {name: 'Группировка закрытие', content: ')', color: '#ff5c5c'},
              {name: 'Диапазон открытие', content: '[', color: '#ff5c5c'},
              {name: 'Диапазон закрытие', content: ']', color: '#ff5c5c'},
          ],
          repeat: [
              {name: 'от "может и не быть" до бесконечности', content: '*', color: '#1b3c64'},
              {name: 'от "может и не быть" до 1 раза', content: '?', color: '#1b3c64'},
              {name: 'от "минимум 1 раз" до бесконечности', content: '+', color: '#1b3c64'},
              {name: 'ввести точное значение повторений', color: '#1b3c64'},
              {name: 'ввести диапазон повторений', color: '#1b3c64'},
          ],
          showOptions: false,
          options: [],
          visualResults: [],
          results: '',
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
              console.log(this.visualResults);
          }
      },
      delToResult(elem){
        this.visualResults = this.visualResults.filter(e => e.id !== elem.id);
      }
  }
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
  .tool {
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
  .visual-result {
    display: inline-block;
    position: relative;
    padding: 3px 9px;
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