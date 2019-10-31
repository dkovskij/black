<template>
  <div>
    <div ref="board" class="game-field-wrap" />
    <div class="buttons">
      <button @click="newGame" class="new-game-button">Новая игра</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
    }
  },
  mounted() {
    this.init()
  },
  methods: {
    init() {
      // создаем поле с блоками
      this.create();
      // добавляем поддержку клавиатуры
      window.addEventListener('keydown', this.keyboardEvent, false);
      // добавляем события для кнопок
      // document.getElementById('new').onclick = newGame;
      // мешаем блоки для начала игры
      this.newGame();
    },
    getXY(i) { return 'x' + ( ((i-1) % 4) + 1 ) + 'y' + Math.ceil( (i)/4) },
    create() {
        for(let i = 1; i <= 15; i++) {
          let el = document.createElement('div')
          // class="block block-' + i + ' ' + this.getXY(i) + '"
          el.className = 'block block-' + i + ' ' + this.getXY(i)
          el.innerText = i
            this.$refs.board.appendChild(el)
        }
    },
    keyboardEvent(e) {
      this.$emit('on-move')
        switch(e.keyCode) {
            case 38: this.key('up');    break;
            case 40: this.key('down');  break;
            case 37: this.key('left');  break;
            case 39: this.key('right'); break;
        }
        this.checkWin();
    },
    key( type ) {
      let from, to;
        for(let a = 1; a <= 4; a++)
            for(let b = 1; b <= 3; b++) {
                switch( type ) {
                    case 'up':
                        from = 'x'+a+'y'+(b+1)
                        to   = 'x'+a+'y'+b
                        break
                    case 'down':
                        from = 'x'+a+'y'+(4-b)
                        to   = 'x'+a+'y'+(5-b)
                        break
                    case 'left':
                        from = 'x'+(b+1)+'y'+a
                        to   = 'x'+b+'y'+a
                        break
                    case 'right':
                        from = 'x'+(4-b)+'y'+a
                        to   = 'x'+(5-b)+'y'+a
                        break
                }
                let els = document.querySelectorAll('.' + to)
                if( !els.length ) {
                  let el = document.querySelector('.' + from)
                  el.classList.remove(from)
                  el.classList.add(to)
                  // $('.'+from).removeClass(from).addClass(to)
                  return
                }
            }
    },
    getRandomInt(min, max) { return Math.floor(Math.random() * (max - min + 1)) + min; },
    newGame() {
      this.$emit('new-game')
        for(let a = 1; a <= 1000; a++) {
            switch( this.getRandomInt(1 , 4) ) {
                case 1: this.key('up');    break;
                case 2: this.key('down');  break;
                case 3: this.key('left');  break;
                case 4: this.key('right'); break;
            }
        }
    },
    checkWin() {
        let good = 0;
        for(let i = 1; i <= 15; i++) {
          let el = document.querySelector('.block-'+i)
            if(el.classList.contains(this.getXY(i))) good++
        }
        if(good === 15) alert('Поздравляем! Вы победили!')
    },
    }
}
</script>

<style lang='scss'>
@import '@/assets/styles/game.scss';
</style>
