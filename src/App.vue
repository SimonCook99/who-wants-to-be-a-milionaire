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

      <div class="awards-container">

        <div v-for="(award, index) in listaMontepremi" :key="index" :class="index==listaMontepremi.length - 1 ? 'award active' : 'award' ">
          {{award}}
        </div>
        
      </div>
    </div>
    <div class="game-container end" v-else>
      <h1 style="color:white;">Hai completato tutte le domande, ecco i risultati</h1>

      <div class="results">
        <ul class="user">
          <h2>Le tue risposte:</h2>
          <li class="user-answers" v-for="(answer, index) in risposteUtente" :key="index">{{answer}}</li>
        </ul>

        <ul class="CPU">
          <h2>Le risposte corrette:</h2>
          <li v-for="(answer, index) in risposteEsatte" :key="index">{{answer}}</li>
        </ul>
      </div>

      <h2 style="color:white;">Hai risposto correttamente a {{score}} domande</h2>
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
      score: 0,

      
      
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
      indiceCasuale: 0, //indice che servirà a pescare una domanda random, dalla lista di domande

      listaMontepremi: [
        '1.000.000€',
        '500.000€',
        '300.000€',
        '100.000€',
        '50.000€',
        '20.000€',
        '10.000€',
        '5.000€',
        '3.000€',
        '2.000€',
        '1.000€',
        '500€'
      ],
      /* primaDomanda : false, */

      awardIndex: 11, //assegno la lunghezza dell'array "listaMontepremi", servirà per scorrere la lista in base alle risposte dell'utente
      risposteUtente: [],
      risposteEsatte: []

    }
  },
  methods: {
    checkAnswer(risposta){

      let awardsList = document.querySelectorAll('.award'); //mi prendo tutti gli elementi HTML della lista montepremi

      this.risposteUtente.push(risposta);
      this.risposteEsatte.push(this.listaDomande[this.indiceCasuale].rispostaEsatta);


      /* if(this.primaDomanda == false){
        this.primaDomanda = true;
        this.awardIndex = awardsList.length - 1;
      } */


      /*console.log(this.$refs[risposta]); */

      this.clickDisabilitato = true; //disabilito i click dei pulsanti quando viene controllata la risposta

      //se la risposta passata nella funzione dal click è quella corretta, aggiungo la classe "correct", altrimenti "wrong"
      if(risposta === this.listaDomande[this.indiceCasuale].rispostaEsatta){
        this.$refs[risposta].classList.add("correct");
        this.score++;
      

        //passo alla ricompensa successiva, togliendo la classe "active" dal premio corrente e mettendola in quella successiva
        awardsList[this.awardIndex].classList.remove("active");
        this.awardIndex--;
        awardsList[this.awardIndex].classList.add("active");

        setTimeout(this.nextAnswer, 2000, risposta); //dopo 2 secondi passo alla prossima domanda
      }else{
        this.$refs[risposta].classList.add("wrong");
        this.$refs[this.listaDomande[this.indiceCasuale].rispostaEsatta].classList.add("correct");


        //tolgo la classe "active" del premio corrente, e lo resetto al primo stato (cioè l'ultimo dell'array)
        awardsList[this.awardIndex].classList.remove("active");
        this.awardIndex = awardsList.length - 1;
        awardsList[this.awardIndex].classList.add("active");

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
        console.log(this.risposteUtente);
        console.log(this.risposteEsatte);

        setTimeout(this.showResults, 200);
      }

      /* console.log(this.domandeUscite);
      console.log("Domanda successiva: " + this.indiceCasuale); */

      this.domandeUscite.push(this.indiceCasuale);

      console.log(this.domandeUscite);
      
      //rimuovo le classi della risposta alla domanda precedente
      this.$refs[risposta].classList.remove("correct");
      this.$refs[risposta].classList.remove("wrong");
    },

    showResults(){
      let user = document.querySelectorAll('.user-answers');

      for(let i = 0; i < this.risposteUtente.length; i++){
        if(this.risposteUtente[i] == this.risposteEsatte[i]){
          console.log(user);
          user[i].classList.add('correct');
        }else{
          user[i].classList.add('wrong');
        }
      }
    }
  },
  mounted(){
    this.indiceCasuale = Math.floor(Math.random() * this.listaDomande.length);
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
    position: relative;

    &.end{
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
    }

    .results{
      display: flex;
      color: white;
      width: 60%;
      justify-content: space-around;
      margin-top: 20px;

      ul{
        list-style: none;

        .correct{
          color: green;
        }
        .wrong{
          color: red;
        }
      }
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

    .awards-container{
      color: orange;
      position: absolute;
      right: 100px;
      top: 50px;
      width: 100px;

      .active{
        background-color: orange;
        color: black;
        border: 2px solid white;
        border-radius: 5px;
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
