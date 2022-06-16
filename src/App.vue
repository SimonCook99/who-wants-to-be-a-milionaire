<template>
  <main class="container-fluid">
    <div class="logo-container">
      <img src="./img/logo.png" alt="logo">
    </div>

    <div class="game-container" v-if="this.giocoTerminato == false">
      <div class="row">
        <div class="col-5 question">
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

        <div v-for="(award, index) in listaMontepremi" :key="index" :class="index==listaMontepremi.length - 1 ? 'award active' : index==6 ? 'award checkpoint' : 'award' ">
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
      <h2 id="result"></h2>
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
      domandeUscite: [], //array che conterrà gli indici delle domande già uscite, così da non ripeterle
      indiceCasuale: 0, //indice che servirà a pescare una domanda random, dalla lista di domande

      awardIndex: null, //assegno la lunghezza dell'array "listaMontepremi", servirà per scorrere la lista in base alle risposte dell'utente
      
      checkpointRaggiunto: false, //booleano per indicare se è stato raggiunto il checkpoint del montepremi (10.000€)

      listaDomande: [
        {
          domanda: "Che differenza c'è tra l'operatore di concatenazione in PHP, con quello di Javascript?",
          b: "Javascript usa '+' mentre PHP usa '&' ",
          a: "Javascript usa '.' mentre PHP usa '+'  ",
          c: "Javascript usa '+' mentre PHP usa '.' ",
          d: "Non ci sono differenze di concatenazione",
          rispostaEsatta: "Javascript usa '+' mentre PHP usa '.' "
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
          domanda: "Quale di questi è un selettore CSS valido?",
          a: "ul li:first-child",
          b: "input[type=radio]:checked",
          c: "button:hover:not(#example)",
          d: "tutte le risposte precedenti",
          rispostaEsatta: "tutte le risposte precedenti"
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
          domanda: "L'acronimo HTML sta per...",
          a: "HyperText Machine Language",
          b: "HyperText Markup Language",
          c: "HighText Marking Language",
          d: "HyperText Marking Language",
          rispostaEsatta: "HyperText Markup Language"
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
          domanda: "Se non viene specificato ASC o DESC a seguito di un ORDER BY in SQL, il suo valore di default sarà: ",
          a: "ASC",
          b: "DESC",
          c: "nessuno dei due",
          d: "Nessun valore di default",
          rispostaEsatta: "ASC"
        },
        {
          domanda: "Per convenzione, di solito i nomi dei Model su Laravel sono sempre al...",
          a: "plurale",
          b: "singolare, senza spazi tra le parole",
          c: "singolare, con spazi quando necessario",
          d: "Nessuno dei precedenti",
          rispostaEsatta: "singolare, senza spazi tra le parole"
        },
        {
          domanda: "Qual è il limite massimo di caratteri che ha il VARCHAR di SQL?",
          a: "155",
          b: "55",
          c: "255",
          d: "512",
          rispostaEsatta: "255"
        },
        {
          domanda: "Quali sono i metodi presenti nelle migrations in Laravel?",
          a: "run() e delete()",
          b: "update() e delete()",
          c: "up() e down()",
          d: "execute() e rollback()",
          rispostaEsatta: "up() e down()"
        },
        {
          domanda: "Quale comando di SQL viene utilizzato per unire i dati di una o più tabelle?",
          a: "SELECT",
          b: "JOIN",
          c: "PRODUCT",
          d: "FUSION",
          rispostaEsatta: "JOIN"
        },
        {
          domanda: "Quale dei seguenti non è un evento in Javascript?",
          a: "onclick",
          b: "onmousescroller",
          c: "onmouseover",
          d: "onmousemove",
          rispostaEsatta: "onmousescroller"
        },
      ],

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
        '500€',
        '0€'
      ],

      risposteUtente: [], //questi 2 array saranno utilizzati per mostrare i risultati delle domande alla fine del gioco
      risposteEsatte: [],
      

    }
  },
  methods: {
    checkAnswer(risposta){

      let awardsList = document.querySelectorAll('.award'); //mi prendo tutti gli elementi HTML della lista montepremi
      
      //l'utente guadagnerà il checkpoint quando lo supererà, ecco perchè la variabile è 20.000 anche se il checkpoint è 10.000
      let checkPoint = '20.000€'; 

      this.risposteUtente.push(risposta);
      this.risposteEsatte.push(this.listaDomande[this.indiceCasuale].rispostaEsatta);

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

        //sistemo l'indice della ricompensa attiva, in base al fatto che l'utente abbia raggiunto o meno il checkpoint
        if(this.checkpointRaggiunto){
          this.awardIndex = 6
        }else{
          this.awardIndex = awardsList.length - 1;
        }

        awardsList[this.awardIndex].classList.add("active");
        /* console.log(awardsList[this.awardIndex]); */

        setTimeout(this.nextAnswer, 2000, risposta); //dopo 2 secondi passo alla prossima domanda
        
      }

      //quando la ricompensa attiva arriva al checkpoint, setto la variabile a true
      if(document.querySelector('.award.active').innerText == checkPoint){
        this.checkpointRaggiunto = true;
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

        let risultatoMontepremi = document.querySelector(".active").innerText;

        //mostro i risultati, mostrando il montepremi guadagnato
        setTimeout(this.showResults, 200, risultatoMontepremi);
      }

      this.domandeUscite.push(this.indiceCasuale);
      
      //rimuovo le classi della risposta alla domanda precedente
      this.$refs[risposta].classList.remove("correct");
      this.$refs[risposta].classList.remove("wrong");
    },

    showResults(montepremi){
      let user = document.querySelectorAll('.user-answers');

      //scorro tra le risposte utente, e assegno la classe corretta in base alla risposta esatta o sbagliata
      for(let i = 0; i < this.risposteUtente.length; i++){
        if(this.risposteUtente[i] == this.risposteEsatte[i]){
          user[i].classList.add('correct');
        }else{
          user[i].classList.add('wrong');
        }
      }

      document.getElementById("result").innerText = "Hai guadagnato " + montepremi;
    }
  },
  mounted(){
    this.indiceCasuale = Math.floor(Math.random() * this.listaDomande.length);
    this.domandeUscite.push(this.indiceCasuale);

    this.awardIndex = this.listaMontepremi.length - 1;

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
      color: white;

      h2#result{
        color: orange;
      }
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

    .square{
      position: absolute;
      background-color: #11093A;
      width: 30px;
      height: 30px;
      z-index: 1;

      &.up-left{
        top: -16px;
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

    .question{
      color: white;
      position: relative;
      margin: 20px 0px;
      text-align: center;
      /* padding: 10px; */
      border: 2px solid white;
      display: flex;
      justify-content: center;
      /* clip-path: polygon(90% 25%, 100% 50%, 90% 75%, 10% 75%, 0 50%, 10% 25%); */

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

      .checkpoint{
        color: white;
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
