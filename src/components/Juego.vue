<template>
  <section class="src-components-juego">
    <Titulo :title="title" 
    :header="header"
    />
    <Barra 
    :isEasy="isEasy" 
    :won="won" 
    :result="result"
    :newColors="newColors"
    :createEasy="createEasy"
    :createHard="createHard"
    />
    <Cuadrados
    :squares="squares"
    :square="square"
    :checkWin="checkWin"
    />
  </section>
</template>

<script lang="js">
  import Titulo from "./Titulo"
  import Barra from "./Barra"
  import Cuadrados from "./Cuadrados"
  export default  {
    name: 'src-components-juego',
    props: [],
    mounted () {
        this.createHard()
    },
    data () {
      return {
        cantColors: 6, // Juego
        squares: [], // Cuadrados--
        isEasy: true, // Barra
        won: false, // Barra
        square: "", // Cuadrados--
        title: "", // Titulo
        result: "", // Barra
        header: ""
      }
    },
    methods: {
      randomInt() {
            return Math.floor(Math.random() * 256);
        },
        createRandomStringColor() {
            return "RGB(" + this.randomInt() + ", " + this.randomInt() + ", " + this.randomInt() + ")";
        },
        createHard() {
            this.reset()
            if (this.isEasy) {
                this.isEasy = false
            }

            this.cantColors = 6
            this.createSquares()
        },
        createEasy() {
            this.reset()
            if (!this.isEasy) {
                this.isEasy = true
            }
            this.cantColors = 3
            this.createSquares()
        },
        newColors() {
            this.reset()
            if (this.isEasy) {
                this.createEasy()
            } else {
                this.createHard()
            }
        },
        setTitle() {
            this.title = this.squares[Math.floor(Math.random() * this.squares.length)]
        },
        checkWin(id, color) {
            if (this.title == color) {
                this.result = "You Picked Right!"
                this.setAllSame()
                this.won = true
            } else {
                this.result = "Try Again!"
                this.delById(id, color)
            }
            
        },
        delById(id) {
            this.squares.push() //usé esto para que me haga el re-render, no sé si es correcto
            this.squares[id] = null
        },
        setAllSame() {
            for (let i = 0; i < this.squares.length; i++) {
                this.squares[i] = this.title
            }
            this.squares.push() //usé esto para que me haga el re-render, no sé si es correcto
            this.header = this.title
        },
        createSquares() {
            this.squares = []
            for (let i = 0; i < this.cantColors; i++) {
                this.square = this.createRandomStringColor()
                this.squares.push(this.square)
            }
            this.setTitle()
        },
        reset(){
            this.result = null
            this.won = false
            this.header = null
        }
    },
    computed: {

    },
    components: {
        Titulo,
        Barra,
        Cuadrados
    }
}


</script>

<style scoped lang="css">
.src-components-juego {
}
</style>
