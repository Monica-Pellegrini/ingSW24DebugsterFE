<script>
import Carousel from "./components/carousel/Carousel.vue";
import axios from 'axios';
import { ref } from 'vue';

/* Inseriamo in una variabile reattiva chiamata data */
const data = ref(0);

/* Effettuiamo la chiamata per recuperare i dati dell'evento e inseriamo nella variabile data i risultati */
axios.get("/api/callRESTdatiEvento").then(response => {
  console.log(response.data)
  data.value = response.data
  }).catch(error => {
  console.error('Si è verificato un errore:', error);
  });

/* Effettuiamo la chiamata per recuperare il video */
fetch('/api/videoTrailer')
    .then(response => response.text())
    .then(videoUrl => {
      /* Costruiamo l'iframe per incorporare il video */
      const iframe = document.createElement('iframe');
      iframe.width = '560';
      iframe.height = '315';
      iframe.src = videoUrl;
      iframe.frameborder = '0';
      iframe.allowfullscreen = true;

      /* Aggiungiamo al div 'videoPlayer' */
      document.getElementById('videoPlayer').appendChild(iframe);
    })
    .catch(error => {
      console.error('Si è verificato un errore:', error);
    });

export default {
  name: "App",
  components: { Carousel },
  data: () => ({
    data: data,
    seguito: null,
    slides: [],
  }),

  methods: {

    /* Evento in caso di follow/unfollow */
    seguiEvento() {

      this.data.utente_segue_evento = !this.data.utente_segue_evento;

      if (this.data.utente_segue_evento === true){
        this.seguito = "Seguito";
      }else{
        this.seguito = "NonSeguito";
      }
    },

    /* Evento in caso di accesso al profilo */
    accessoProfilo() {
      this.data.id_u = !this.data.id_u;
    }
  },

  mounted() {
    this.slides = this.data.immagini_banner_evento;

    /* Settiamo il nome dell'evento come titolo della pagina */
    if(this.data.nome_evento){
      document.title = JSON.stringify(this.data.nome_evento).slice(1,-1)
    }

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
  <div lang="it" id="app" data-v-app>
    <!--Skip per l'accessibilità-->
    <div id="hiddenKeys">
      <a accesskey="c" href="#main">Vai al contenuto della pagina</a>
      <a accesskey="n" href="#search">Vai al menu di navigazione</a>
      <a accesskey="b" href="#BottoneBiglietteria">Vai alla sezione per comprare i biglietti</a>
      <a accesskey="a" href="/Portale/accessibilita">Vai alla sezione Accessibilità</a>
      <a accesskey="p" href="#primo_link_footer">Vai al pié di pagina</a>
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

              <!--Textbox per la ricerca-->
              <label></label>
              <input type="text" role="textbox" id="search" name="search" value="" maxlength="50" required placeholder="Cerca . . ." size="50"/>
            </form>
          </li>
        </ul>
      </nav>
      <nav role="navigation" aria-label="Menù User">
        <ul role="menu">
          <li role="menuitem">
            <!--Bottone filtro-->
            <ul class="filtri" role="menu">
              <li role="menuitem"><a class="BottoneHeader" href="/api/callRESTfiltri">Filtri</a></li>
            </ul>
            <ul class="itemlist" role="menu">
              <!--Se l'utente non ha fatto l'accesso-->
              <li v-if="data.id_u === '0'" role="menuitem">
                <ul>
                  <li role="menuitem"><a class="BottoneHeader" href="/api/callRESTiscrizione">Iscriviti</a></li>
                  <li role="menuitem"><a id="BottoneAccedi"  class="BottoneHeader" @click="accessoProfilo">Accedi</a></li>
                </ul>
              </li>
              <!--Se l'utente ha fatto l'accesso-->
              <li v-else role="menuitem">
                <ul>
                  <li role="menuitem"><p id="Account" class="BottoneHeader">Il mio account:</p></li>
                  <li role="menuitem"><a id="BottoneProfilo"  class="BottoneHeader" href="/api/callRESTprofiloUtente">{{ data.nome_utente }}</a></li>
                </ul>
              </li>

            </ul>
          </li>
        </ul>
      </nav>
    </header>
    <!--Fine Header-->

    <!--Inizio Main-->
    <main id="main" class="clearfix" role="main">

      <!--Banner con immagini principali-->
      <div v-if="slides.length !== 0" id="box_banner">
        <carousel :slides="slides" :interval="3000" controls indicators id="banner"></carousel>
      </div>

      <!--Colonna sinistra-->
      <section id="ColonnaSinistra">

        <!--Infromazioni generali sull'evento-->
        <h1>{{ data.nome_evento }}</h1>
        <p class="description">{{ data.descrizione_evento }}</p>
        <br>
        <br>
        <h2>Data e ora</h2>
        <p id="data" class="description">{{ data.data_evento }} {{data.ora_evento}}</p>
        <br>
        <br>
        <h2>Location</h2>
        <a id="location" class="description" href="/api/callRESTmappe">{{ data.luogo_evento }}</a>
        <br>
        <br>
        <br>
        <h2>Organizzato da</h2>
        <a id="organizzatore" class="description" href="/api/callRESTorgprofilo">{{ data.nome_organizzatore }}</a>
        <br>
        <br>
        <br>
        <h2>Informazioni sull'evento</h2>
        <p class="informazioni">{{ data.informazioni_evento }}</p>

        <!--Immagini secondarie-->
        <div v-for="imm_sec in data.immagini_secondarie_evento">
          <div id="BoxImmagine2">
            <img id="Immagine2" v-bind:src="imm_sec" alt="Immagine secondaria dell'evento">
          </div>
        </div>

        <br>

      </section>

      <!--Colonna destra-->
      <aside id="ColonnaDestra">

        <!--Box con prezzo e link biglietteria-->
        <div id="BoxBiglietteria" v-if="data.prezzo_biglietti_evento.length !== 0">
          <!--Visualizzazione del prezzo-->
          <h2 id="prezzo" v-if="data.prezzo_biglietti_evento[0] === 0 && data.prezzo_biglietti_evento.length === 1">Gratis</h2>
          <h2 id="prezzo" v-else-if="data.prezzo_biglietti_evento[0] !== 0 && data.prezzo_biglietti_evento.length === 1">{{data.prezzo_biglietti_evento[0]}} €</h2>
          <h2 id="prezzo" v-else>{{data.prezzo_biglietti_evento[0]}} - {{data.prezzo_biglietti_evento[data.prezzo_biglietti_evento.length - 1]}} €</h2>

          <!--Link alla biglietteria-->
          <div id="BottoneBiglietteria">
            <a href="/api/callRESTbiglietteria">COMPRA ORA</a>
          </div>
        </div>

        <!--Link vari-->
        <nav id="NavDestra" role="navigation">
          <ul role="menu">
            <li role="menuitem">
              <ul class="LinkDestra" role="menu">
                <!--Bottone per il follow dell'evento-->
                <li role="menuitem" v-if="data.id_u !== '0'"><a class="BottoneDestra" v-bind:id="seguito" @click="seguiEvento">Segui</a></li>
                <!--Link alla chat dell'evento-->
                <li role="menuitem" class="chat_live"><a class="BottoneDestra" href="/api/callRESTchat">Chat</a></li>
                <!--Link alla live dell'evento-->
                <li role="menuitem" class="chat_live"><a class="BottoneDestra" href="/api/callRESTlive">Live</a></li>
              </ul>
            </li>
            <!--Link alle recensioni dell'evento-->
            <li role="menuitem" id="BottoneRecensioni"><a class="BottoneDestra" href="/api/callRESTrecensioni">Recensioni</a></li>
            <!--Link ai sondaggi dell'evento-->
            <li role="menuitem" id="BottoneSondaggiPre"><a class="BottoneDestra" href="/api/sondaggioPre">Sondaggio pre-evento</a></li>
            <li role="menuitem" id="BottoneSondaggiPost"><a class="BottoneDestra" href="/api/sondaggioPost">Sondaggio post-evento</a></li>
          </ul>
        </nav>

        <!--Box del video-->
        <div id="videoPlayer"></div>

      </aside>

      <!--Link a inizio pagina-->
      <div id="torna_su">
        <a href="#header"><img id="freccia" role="button" src="@/images/freccia.jpg" alt="Clicca qui per tornare in cima alla pagina"></a>
      </div>

    </main>
    <!--Fine Main-->

    <!--Inizio Footer-->
    <footer>
      <!--Link in 3 gruppi da 3-->
      <nav class="footer-nav" role="navigation" aria-label="link footer">
        <!--Prima colonna-->
        <ul class="prima_colonna" role="menu">
          <li id="primo_link_footer" role="menuitem"><a href="/api/callRESThomepage">Home Page</a></li>
          <li role="menuitem"><a href="/api/callRESTfaq">FAQs</a></li>
          <li role="menuitem"><a href="/api/callRESTsponsor">Sponsorizzazioni</a></li>
        </ul>
        <!--Seconda colonna-->
        <ul class="seconda_colonna" role="menu">
          <li role="menuitem"><a href="/api/callRESTsupportoUtente">Supporto utente</a></li>
          <li role="menuitem"><a href="/api/callRESTsupportoTecnico">Supporto tecnico</a></li>
          <li role="menuitem"><a href="/api/callRESTsegnalaEvento">Segnala questo evento</a></li>
        </ul>
        <!--Terza colonna-->
        <ul class="terza_colonna" role="menu">
          <li role="menuitem"><a href="/privacy">Privacy</a></li>
          <li role="menuitem"> <a href="/cookies">Cookies</a></li>
          <li role="menuitem"><a href="/terms">Termini di utilizzo</a></li>
        </ul>
      </nav>
    </footer>
    <!--Fine Footer-->
  </div>
</template>


<style scoped>
@import 'stylesheet.css';
</style>
