<script>
import Carousel from "./components/carousel/Carousel.vue";
import axios from 'axios';
import { ref } from 'vue';

/* Prendiamo le variabili passate dalla chiamata */
const params = new URL(document.location.toString()).searchParams;
const id_evento = params.get("id_e");
const id_utente = params.get("id_u");
console.log(id_evento);
console.log(id_utente);

 /*new class View{
  "nome_utente": string;
  "nome_evento": string;
  "descrizione_evento": string;
  "data_evento": string;
  "ora_evento": string;
  "luogo_evento": string;
  "nome_organizzatore": string;
  "informazioni_evento": string;
  "link_sondaggio_pre_evento": string;
  "link_sondaggio_post_evento": string;
  "prezzo_biglietto_evento": string[];
  "immagini_banner_evento": string[];
  "immagine_secondaria_evento": string;
  "utente_segue_evento": boolean;
}*/

//questa rest va a mockoon
const restCall = "http://localhost:3001/api/datiEvento?id_e=" + id_evento + "&id_u=" + id_utente;

/* Inseriamo in una variabile reattiva chiamata data */
const data = ref(0);

/* Inseriamo nella variabile data il risultato della chiamata al backend */
axios.get(restCall).then(response => {
  console.log(response.data)
  data.value = response.data
})

fetch('/api/videoTrailer')
    .then(response => response.text())
    .then(videoUrl => {
      // Costruisce l'iframe per incorporare il video
      var iframe = document.createElement('iframe');
      iframe.width = '560';
      iframe.height = '315';
      iframe.src = videoUrl;
      iframe.frameborder = '0';
      iframe.allowfullscreen = true;

      // Aggiunge al div 'videoPlayer'
      document.getElementById('videoPlayer').appendChild(iframe);
    })
    .catch(error => {
      console.error('Si è verificato un errore:', error);
    });

export default {
  name: "App",
  components: { Carousel },
  data: () => ({
    id_u: id_utente,
    data: data,
    seguito: null,
    slides: [],
  }),
  methods: {
    seguiEvento() {

      this.data.utente_segue_evento = !this.data.utente_segue_evento;

      if (this.data.utente_segue_evento === true){
        this.seguito = "Seguito";
      }else{
        this.seguito = "NonSeguito";
      }


      /* INSERIRE REST */

    }
  },
  beforeMount() {
    if (this.data.utente_segue_evento === true){
      this.seguito = "Seguito";
    }else{
      this.seguito = "NonSeguito";
    }
  },
  mounted() {
    this.slides = this.data.immagini_banner_evento;
  }
}

</script>

<template>
  <head>
    <title>{{data.nome_evento}}</title>
    <meta
        name="description"
        content="Pagina principale dell'evento contiene informazioni su data, luogo, organizzatore e prezzo">
  </head>
  <div lang="it" id="app" data-v-app @loadstart="onLoad">
    <!--skip per l'accessibilità-->
    <div id="hiddenKeys">
      <a accesskey="c" href="#main">vai al contenuto della pagina</a>
      <a accesskey="n" href="#search">vai al menu di navigazione</a>
      <a accesskey="b" href="#BottoneBiglietteria"> vai alla sezione per comprare i biglietti</a>
      <a accesskey="a" href="/Portale/accessibilita.html"> vai alla sezione Accessibilità</a>
      <a accesskey="p" href="#primo_link_footer">vai al pié di pagina</a>
    </div>
    <!--Inizio Header-->
    <header id="header">
      <nav role="navigation" aria-label="Barra Ricerca">
        <ul>
          <li class="searchbar">
            <!--Form per la ricerca-->
            <form name="searchForm" role="form" method="get">
              <label></label>
              <button type="submit" role="button" id="Bottonelente">
                <img id="submit" role="button" src="@/images/lente.png" alt="Clicca qui per per cercare">
              </button>

              <!--Textbox per ricerca-->
              <label></label>
              <input type="text" role="textbox" id="search" name="search" value="" maxlength="50" required placeholder="Cerca . . ." size="50"/>
            </form>
          </li>
        </ul>
      </nav>
      <nav role="navigation" aria-label="Menù User">
        <ul role="menu">
          <li role="menuitem">
            <!--Bottoni header-->
            <ul class="filtri" role="menu">
              <li role="menuitem"><a class="BottoneHeader" href="filtri.html">Filtri</a></li>
            </ul>
            <ul class="itemlist" role="menu">
              <!--Filtri è vicino al textbox, gli altri sono raggruppati a sinistra-->
              <li v-if="id_u === '0'" role="menuitem"><a class="BottoneHeader" href="iscriviti.html">Iscriviti</a></li>
              <li v-if="id_u === '0'" role="menuitem"><a id="BottoneAccedi"  class="BottoneHeader" href="accedi.html">Accedi</a></li>
              <!--Se l'utente è registrato-->
              <li v-else role="menuitem"><a id="BottoneProfilo"  class="BottoneHeader" href="utente.html">{{ data.nome_utente }}</a></li>
            </ul>
          </li>
        </ul>
      </nav>
    </header>
    <!--Fine Header-->
    <!--Inizio Main-->
    <main id="main" class="clearfix" role="main">
      <!--Immagine principale-->
      <div id="box_banner">
        <carousel :slides="slides" :interval="3000" controls indicators id="banner"></carousel>
      </div>

      <!--Colonna sinistra-->
      <section id="ColonnaSinistra">
        <h1>{{ data.nome_evento }}</h1>
        <p class="description">{{ data.descrizione_evento }}</p>
        <br>
        <br>
        <h2>Data e ora</h2>
        <p id="data" class="description">{{ data.data_evento }} {{data.ora_evento}}</p>
        <br>
        <br>
        <h2>Location</h2>
        <a id="location" class="description" href="mappa.html">{{ data.nome_evento }}</a>
        <br>
        <br>
        <br>
        <h2>Organizzato da</h2>
        <a id="organizzatore" class="description" href="profilo_org.html">{{ data.nome_organizzatore }}</a>
        <br>
        <br>
        <br>
        <h2>Informazioni sull'evento</h2>
        <p class="informazioni">{{ data.informazioni_evento }}</p>

        <!--Immagine secondaria-->
        <div id="BoxImmagine2">
          <img id="Immagine2" v-bind:src="data.immagine_secondaria_evento" alt="Immagine secondaria dell'evento">
        </div>
        <br>
        <a id="segnala" href="segnala.html">Segnala questo evento</a>
      </section>

      <!--Colonna destra-->
      <aside id="ColonnaDestra">

        <!--Box con prezzo e link biglietteria-->
        <div id="BoxBiglietteria">
          <h2 id="prezzo" v-if="data.prezzo_biglietti_evento[0] == 0 && data.prezzo_biglietti_evento.length == 1">Gratis</h2>
          <h2 id="prezzo" v-else-if="data.prezzo_biglietti_evento[0] != 0 && data.prezzo_biglietti_evento.length == 1">{{data.prezzo_biglietti_evento[0]}} €</h2>
          <h2 id="prezzo" v-else>{{data.prezzo_biglietti_evento[0]}} - {{data.prezzo_biglietti_evento[data.prezzo_biglietti_evento.length - 1]}} €</h2>
          <div id="BottoneBiglietteria">
            <a href="biglietteria.html">COMPRA ORA</a>
          </div>
        </div>

        <!--Link vari-->
        <nav id="NavDestra" role="navigation">
          <ul role="menu">
            <li role="menuitem">
              <ul class="LinkDestra" role="menu">
                <li role="menuitem" v-if="id_u !== '0'"><a class="BottoneDestra" v-bind:id="seguito" @click="seguiEvento">Segui</a></li>
                <li role="menuitem" class="chat_live"><a class="BottoneDestra" href="chat.html">Chat</a></li>
                <li role="menuitem" class="chat_live"><a class="BottoneDestra" href="live.html">Live</a></li>
              </ul>
            </li>
            <li role="menuitem" id="BottoneRecensioni"><a class="BottoneDestra" href="recensioni.html">Recensioni</a></li>
            <li role="menuitem" id="BottoneSondaggiPre"><a class="BottoneDestra" v-bind:href="data.link_sondaggio_pre_evento">Sondaggio pre-evento</a></li>
            <li role="menuitem" id="BottoneSondaggiPost"><a class="BottoneDestra" v-bind:href="data.link_sondaggio_post_evento">Sondaggio post-evento</a></li>
          </ul>
        </nav>

        <!--Box video-->
        <div id="videoPlayer"><!--QUI VA IL VIDEO--></div>

      </aside>

      <!--Link a inizio pagina-->
      <div id="torna_su">
        <a href="#header"><img id="freccia" role="button" src="@/images/freccia.jpg" alt="Clicca qui per tornare in cima alla pagina"></a>
      </div>
    </main>
    <!--Fine Main-->
    <!--Inizio Footer-->
    <footer>
      <!--Link da raggruppare in 3 gruppi da 3-->
      <!--Prima colonna-->
      <nav class="footer-nav" role="navigation" aria-label="link footer">
        <ul class="prima_colonna" role="menu">
          <li id="primo_link_footer" role="menuitem"><a href="home.html">Home Page</a></li>
          <li role="menuitem"><a href="faq.html">FAQs</a></li>
          <li role="menuitem"><a href="sponsor.html">Sponsorizzazioni</a></li>
        </ul>
        <!--Seconda colonna-->
        <ul class="seconda_colonna" role="menu">
          <li role="menuitem"><a href="user_sup.html">Supporto utente</a></li>
          <li role="menuitem"><a href="tec_sup.html">Supporto tecnico</a></li>
          <li role="menuitem"><a href="segnala.html">Segnala questo evento</a></li>
        </ul>
        <!--Terza colonna-->
        <ul class="terza_colonna" role="menu">
          <li role="menuitem"><a href="privacy.html">Privacy</a></li>
          <li role="menuitem"> <a href="cookies.html">Cookies</a></li>
          <li role="menuitem"><a href="terms.html">Termini di utilizzo</a></li>
        </ul>
      </nav>
    </footer>
    <!--Fine Footer-->
  </div>
</template>


<style scoped>
@import 'stylesheet.css';
</style>
