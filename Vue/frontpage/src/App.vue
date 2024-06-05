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
  "id_organizzzatore": number;
  "informazioni_evento": string;
  "link_sondaggio_pre_evento": string;
  "link_sondaggio_post_evento": string;
  "prezzo_biglietti_evento": string[];
  "immagini_banner_evento": string[];
  "immagini_secondarie_evento": string[];
  "utente_segue_evento": boolean;
}*/

/* Costruiamo la chiamata rest */
const restCall = "http://localhost:3001/api/datiEvento?id_e=" + id_evento + "&id_u=" + id_utente;

/* Inseriamo in una variabile reattiva chiamata data */
const data = ref(0);

/* Effettuiamo la chiamata per recuperare il video e inseriamo nella variabile data il risultato */
axios.get(restCall).then(response => {
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
    id_u: id_utente,
    data: data,
    seguito: null,
    slides: [],
    link_profilo_utente: null,
    link_mappa: null,
    link_profilo_organizzatore: null,
    link_biglietteria: null,
    link_chat: null,
    link_live: null,
    link_recensioni: null,
    link_segnalazione: null
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

      /* INSERIRE REST */

    }
  },

  beforeMount() {

    if (this.data.utente_segue_evento === true){
      this.seguito = "Seguito";
    }else{
      this.seguito = "NonSeguito";
    }

    /* Costruiamo i link alle altre funzioni con i parametri passati*/
    this.link_profilo_utente = "/ilMioProfilo?id_u=" + id_utente;
    this.link_mappa = "/mappaEvento?id_e=" + id_evento + "&id_u=" + id_utente;
    this.link_profilo_organizzatore = "/visualizzaProfilo?id_o=" + this.data.id_organizzatore + "&id_u=" + id_utente;
    this.link_biglietteria = "/biglietteriaEvento?id_e=" + id_evento + "&id_u=" + id_utente;
    this.link_chat = "/chatEvento?id_e=" + id_evento + "&id_u=" + id_utente;
    this.link_live = "/liveEvento?id_e=" + id_evento + "&id_u=" + id_utente;
    this.link_recensioni = "/recensioniEvento?id_e=" + id_evento + "&id_u=" + id_utente;
    this.link_segnalazione = "/segnalaEvento?id_e=" + id_evento + "&id_u=" + id_utente;
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
              <li role="menuitem"><a class="BottoneHeader" href="/filtri">Filtri</a></li>
            </ul>
            <ul class="itemlist" role="menu">
              <!--Se l'utente non ha fatto l'accesso-->
              <li v-if="id_u === '0'" role="menuitem">
                <ul>
                  <li role="menuitem"><a class="BottoneHeader" href="/iscriviti">Iscriviti</a></li>
                  <li role="menuitem"><a id="BottoneAccedi"  class="BottoneHeader" href="/accedi">Accedi</a></li>
                </ul>
              </li>
              <!--Se l'utente ha fatto l'accesso-->
              <li v-else role="menuitem">
                <ul>
                  <li role="menuitem"><p id="Account" class="BottoneHeader">Il mio account:</p></li>
                  <li role="menuitem"><a id="BottoneProfilo"  class="BottoneHeader" :href="link_profilo_utente">{{ data.nome_utente }}</a></li>
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
        <a id="location" class="description" :href="link_mappa">{{ data.luogo_evento }}</a>
        <br>
        <br>
        <br>
        <h2>Organizzato da</h2>
        <a id="organizzatore" class="description" :href="link_profilo_organizzatore">{{ data.nome_organizzatore }}</a>
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

        <!--Link per segnalare l'evento-->
        <a id="segnala" :href="link_segnalazione">Segnala questo evento</a>
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
            <a :href="link_biglietteria">COMPRA ORA</a>
          </div>
        </div>

        <!--Link vari-->
        <nav id="NavDestra" role="navigation">
          <ul role="menu">
            <li role="menuitem">
              <ul class="LinkDestra" role="menu">
                <!--Bottone per il follow dell'evento-->
                <li role="menuitem" v-if="id_u !== '0'"><a class="BottoneDestra" v-bind:id="seguito" @click="seguiEvento">Segui</a></li>
                <!--Link alla chat dell'evento-->
                <li role="menuitem" class="chat_live"><a class="BottoneDestra" :href="link_chat">Chat</a></li>
                <!--Link alla live dell'evento-->
                <li role="menuitem" class="chat_live"><a class="BottoneDestra" :href="link_live">Live</a></li>
              </ul>
            </li>
            <!--Link alle recensioni dell'evento-->
            <li role="menuitem" id="BottoneRecensioni"><a class="BottoneDestra" :href="link_recensioni">Recensioni</a></li>
            <!--Link ai sondaggi dell'evento-->
            <li v-if="data.link_sondaggio_pre_evento" role="menuitem" id="BottoneSondaggiPre"><a class="BottoneDestra" v-bind:href="data.link_sondaggio_pre_evento">Sondaggio pre-evento</a></li>
            <li v-if="data.link_sondaggio_post_evento" role="menuitem" id="BottoneSondaggiPost"><a class="BottoneDestra" v-bind:href="data.link_sondaggio_post_evento">Sondaggio post-evento</a></li>
          </ul>
        </nav>

        <!--Box del video-->
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
      <!--Link in 3 gruppi da 3-->
      <nav class="footer-nav" role="navigation" aria-label="link footer">
        <!--Prima colonna-->
        <ul class="prima_colonna" role="menu">
          <li id="primo_link_footer" role="menuitem"><a href="/home">Home Page</a></li>
          <li role="menuitem"><a href="/faq">FAQs</a></li>
          <li role="menuitem"><a href="/sponsorizzazioni">Sponsorizzazioni</a></li>
        </ul>
        <!--Seconda colonna-->
        <ul class="seconda_colonna" role="menu">
          <li role="menuitem"><a href="/sup_utente">Supporto utente</a></li>
          <li role="menuitem"><a href="/sup_tecnico">Supporto tecnico</a></li>
          <li role="menuitem"><a :href="link_segnalazione">Segnala questo evento</a></li>
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
