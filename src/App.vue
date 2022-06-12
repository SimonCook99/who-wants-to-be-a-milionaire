<template>
  <main class="container-fluid">
    <div class="logo-container">
      <img src="./img/logo.png" alt="logo">
    </div>

    <div class="game-container" v-if="this.giocoTerminato == false">
      <div class="row">
        <div class="col-4 question">
          <p>{{listaDomande[indiceCasuale].domanda}}</p>

          <div class="square up-left"></div>
          <div class="square up-right"></div>
          <div class="square bottom-left"></div>
          <div class="square bottom-right"></div>
        </div>
      </div>

      <div class="row answers">
        <button class="col-6" :ref="listaDomande[indiceCasuale].a" :disabled="this.clickDisabilitato" @click="checkAnswer(listaDomande[indiceCasuale].a)">
          <p>{{listaDomande[indiceCasuale].a}}</p>

          <div class="square up-left"></div>
          <div class="square up-right"></div>
          <div class="square bottom-left"></div>
          <div class="square bottom-right"></div>
        </button>
        <button class="col-6" :ref="listaDomande[indiceCasuale].b" :disabled="this.clickDisabilitato" @click="checkAnswer(listaDomande[indiceCasuale].b)">
          <p>{{listaDomande[indiceCasuale].b}}</p>

          <div class="square up-left"></div>
          <div class="square up-right"></div>
          <div class="square bottom-left"></div>
          <div class="square bottom-right"></div>
        </button>
        <button class="col-6" :ref="listaDomande[indiceCasuale].c" :disabled="this.clickDisabilitato" @click="checkAnswer(listaDomande[indiceCasuale].c)">
          <p>{{listaDomande[indiceCasuale].c}}</p>

          <div class="square up-left"></div>
          <div class="square up-right"></div>
          <div class="square bottom-left"></div>
          <div class="square bottom-right"></div>
        </button>
        <button class="col-6" :ref="listaDomande[indiceCasuale].d" :disabled="this.clickDisabilitato" @click="checkAnswer(listaDomande[indiceCasuale].d)">
          <p>{{listaDomande[indiceCasuale].d}}</p>

          <div class="square up-left"></div>
          <div class="square up-right"></div>
          <div class="square bottom-left"></div>
          <div class="square bottom-right"></div>
        </button>
      </div>
    </div>
    <div class="game-container end" v-else>
      <h1 style="color:white;">Complimenti, hai completato tutte le domande</h1>
    </div>
  </main>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{

      clickDisabilitato: false, //booleano per disabilitare i tasti all'occorrenza
      giocoTerminato: false, //booleano per indicare se il gioco è terminato
      
      listaDomande: [
        {
          domanda: "Chi è stato il primo insegnante boolean?",
          a: "Michele",
          b: "Fabio",
          c: "Chiara",
          d: "Luca",
          rispostaEsatta: "Fabio"
        },
        {
          domanda: "Html è un linguaggio frontend o backend?",
          a: "frontend",
          b: "backend",
          c: "entrambe",
          d: "nessuna delle due",
          rispostaEsatta: "frontend"
        },
        {
          domanda: "Quanto bestemmierò per fare i bordi delle risposte?",
          a: "un casino",
          b: "tantissimo",
          c: "non tanto",
          d: "Voglio morire",
          rispostaEsatta: "tantissimo"
        },
        {
          domanda: "Quale dei seguenti è un linguaggio di programmazione server side?",
          a: "HTML",
          b: "PHP",
          c: "C",
          d: "JS",
          rispostaEsatta: "PHP"
        },
        {
          domanda: "Una funzione che richiama se stessa è detta:",
          a: "Ridondante",
          b: "Iterativa",
          c: "Ripetitiva",
          d: "Ricorsiva",
          rispostaEsatta: "Ricorsiva"
        },
        {
          domanda: "Qual è il limite massimo di caratteri che ha il VARCHAR di SQL?",
          a: "155",
          b: "55",
          c: "255",
          d: "512",
          rispostaEsatta: "255"
        },
      ],
      domandeUscite: [], //array che conterrà gli indici delle domande già uscite, così da non ripeterle
      indiceCasuale: 0 //indice che servirà a pescare una domanda random, dalla lista di domande
    }
  },
  methods: {
    checkAnswer(risposta){
      /* console.log(this.listaDomande);
      console.log("Risposta: " + risposta);
      console.log(this.$refs[risposta]);
      console.log(this.listaDomande[this.indiceCasuale].rispostaEsatta); */

      this.clickDisabilitato = true; //disabilito i click dei pulsanti quando viene controllata la risposta

      //se la risposta passata nella funzione dal click è quella corretta, aggiungo la classe "correct", altrimenti "wrong"
      if(risposta === this.listaDomande[this.indiceCasuale].rispostaEsatta){
        this.$refs[risposta].classList.add("correct");

        setTimeout(this.nextAnswer, 2000, risposta); //dopo 2 secondi passo alla prossima domanda
      }else{
        this.$refs[risposta].classList.add("wrong");
        this.$refs[this.listaDomande[this.indiceCasuale].rispostaEsatta].classList.add("correct");

        setTimeout(this.nextAnswer, 2000, risposta); //dopo 2 secondi passo alla prossima domanda
        
      }
    },
    nextAnswer(risposta){

      //riabilito i click delle risposte, e rimuovo la classe che mostrava la risposta corretta in caso di errore
      this.clickDisabilitato = false;
      this.$refs[this.listaDomande[this.indiceCasuale].rispostaEsatta].classList.remove("correct");


      while(this.domandeUscite.includes(this.indiceCasuale) && this.domandeUscite.length != this.listaDomande.length){
        this.indiceCasuale = Math.floor(Math.random() * this.listaDomande.length);
        console.log("cerco una domanda nuova");
      }

      //se la lunghezza delle domande uscite è uguale alla lista delle domande, il gioco finisce
      if(this.domandeUscite.length == this.listaDomande.length){
        this.giocoTerminato = true;
      }

      /* console.log(this.domandeUscite);
      console.log("DOmanda successiva: " + this.indiceCasuale); */

      this.domandeUscite.push(this.indiceCasuale);

      console.log(this.domandeUscite);
      
      //rimuovo le classi della risposta alla domanda precedente
      this.$refs[risposta].classList.remove("correct");
      this.$refs[risposta].classList.remove("wrong");
    }
  },
  mounted(){
    this.indiceCasuale = Math.floor(Math.random() * this.listaDomande.length);
    console.log(this.indiceCasuale);
    console.log(this.listaDomande);
    this.domandeUscite.push(this.indiceCasuale);
  }
}
</script>

<style lang="scss">

  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .container-fluid{
    padding: 0;
  }

  .logo-container{
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #13158c;

    img{
      height: 200px;
      margin: 20px 0px;
    }
  }

  .game-container{
    background-color: #11093A;
    height: 70vh;

    &.end{
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .row{
      justify-content: center;
    }

    .question{
      color: white;
      position: relative;
      margin: 20px 0px;
      /* padding: 10px; */
      border: 2px solid white;
      display: flex;
      justify-content: center;
      /* clip-path: polygon(10% 0, 88% 0, 100% 50%, 88% 100%, 11% 100%, 1% 50%); */
      /* clip-path: polygon(90% 25%, 100% 50%, 90% 75%, 10% 75%, 0 50%, 10% 25%); */

      .square{
        position: absolute;
        background-color: #11093A;
        width: 30px;
        height: 30px;
        z-index: 1;

        &.up-left{
          top: -15px;
          left: -15px;
          transform: rotate(45deg);
          border-right: 2px solid white;
        }

        &.up-right{
          top: -16px;
          right: -15px;
          transform: rotate(45deg);
          border-bottom: 2px solid white;
        }

        &.bottom-left{
          bottom: -14px;
          left: -15px;
          transform: rotate(45deg);
          border-top: 2px solid white;
        }

        &.bottom-right{
          bottom: -14px;
          right: -15px;
          transform: rotate(45deg);
          border-left: 2px solid white;
        }
      }
    }

    .answers{
      
      width: 50%;
      justify-content: center;
      position: relative;
      left: 50%;
      transform: translate(-50%, 0%);
      justify-content: center;
      align-items: center;
      
      color: white;

      .correct{
        background-color: green!important;
        color: white;
      }
      .wrong{
        background-color: red!important;
        color: white;
      }
      
      .col-sm-12{
        width: 100%;
      }

      .col-6{
        position: relative;
        margin: 10px 20px 10px 0px;
        width: calc(50% - 20px);
        text-align: center;
        border: 2px solid white;
        align-items: center;
        background-color: transparent;
        color: white;
        /* clip-path: polygon(90% 25%, 100% 50%, 90% 75%, 10% 75%, 0 50%, 10% 25%); */



        &:hover:not(.wrong):not(.correct){
          background-color: white;
          color: black;
          cursor: pointer;
        }
      }

      .square{
        position: absolute;
        background-color: #11093A;
        width: 30px;
        height: 30px;
        z-index: 1;

        &.up-left{
          top: -15px;
          left: -15px;
          transform: rotate(45deg);
          border-right: 2px solid white;
        }

        &.up-right{
          top: -15px;
          right: -15px;
          transform: rotate(45deg);
          border-bottom: 2px solid white;
        }

        &.bottom-left{
          bottom: -16px;
          left: -15px;
          transform: rotate(45deg);
          border-top: 2px solid white;
        }

        &.bottom-right{
          bottom: -16px;
          right: -15px;
          transform: rotate(45deg);
          border-left: 2px solid white;
        }
      }


      button:disabled:not(.wrong):not(.correct){
        border: 2px solid white;
        color: white;

        &:hover{
          background-color: transparent;
          color: white;
        }
      }


    }
  }

@media (max-width: 576px) {
  .game-container .answers .col-6{
    width: 100%;
  }

  .col-4{
    width: 75%;
  }
}
</style>
