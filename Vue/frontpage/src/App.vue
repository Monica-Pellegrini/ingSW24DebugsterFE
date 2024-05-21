<template>
  <div lang="it" id="app" data-v-app>
    <!--skip per l'accessibilità-->
    <div id="hiddenKeys">
      <a accesskey="c" href="#main">vai al contenuto della pagina</a>
      <a accesskey="n" href="#search">vai al menu di navigazione</a>
      <a accesskey="b" href="#BottoneBiglietteria"> vai alla sezione per comprare i biglietti</a>
      <a accesskey="a" href="/Portale/accessibilita.htm"> vai alla sezione Accessibilità</a>
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
          <li>
            <!--Bottoni header-->
            <ul class="filtri">
              <li role="menuitem"><a class="BottoneHeader" href="filtri.html">Filtri</a></li>
            </ul>
            <ul class="itemlist">
              <!--Filtri è vicino al textbox, gli altri sono raggruppati a sinistra-->
              <li role="menuitem"><a class="BottoneHeader" href="iscriviti.html">Iscriviti</a></li>
              <li role="menuitem"><a id="BottoneAccedi"  class="BottoneHeader" href="accedi.html">Accedi</a></li>
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
        <h1>Nome evento</h1>
        <p class="description">Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
        <br>
        <br>
        <h2>Data e ora</h2>
        <p id="data" class="description">Lun, 1 Gen 2024 09:00 - 13:00 CET</p>
        <br>
        <br>
        <h2>Location</h2>
        <a id="location" class="description" href="mappa.html">Via Giuseppe Saragat, 1, 44124 Ferrara FE</a>
        <br>
        <br>
        <br>
        <h2>Organizzato da</h2>
        <a id="organizzatore" class="description" href="profilo_org.html">Dr Nerina Ramlakhan</a>
        <br>
        <br>
        <br>
        <h2>Infromazioni sull'evento</h2>
        <p class="informazioni">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>

        <!--Immagine secondaria-->
        <div id="BoxImmagine2">
          <img id="Immagine2" src="@/images/imm2.jpg" alt="Immagine secondaria dell'evento">
        </div>
        <br>
        <a id="segnala" href="segnala.html">Segnala questo evento</a>
      </section>

      <!--Colonna destra-->
      <aside id="ColonnaDestra">
        <!--Box con prezzo e link biglietteria-->
        <div id="BoxBiglietteria">
          <h2 id="prezzo">9 - 18€</h2>
          <div id="BottoneBiglietteria">
            <a href="biglietteria.html">COMPRA ORA</a>
          </div>
        </div>

        <!--Link vari-->
        <nav id="NavDestra" role="navigation">
          <ul role="menu">
            <li>
              <ul class="LinkDestra">
                <li role="menuitem"><p class="BottoneDestra">Segui</p></li>
                <li role="menuitem" class="chat_live"><a class="BottoneDestra" href="chat.html">Chat</a></li>
                <li role="menuitem" class="chat_live"><a class="BottoneDestra" href="live.html">Live</a></li>
              </ul>
            </li>
            <li role="menuitem" id="BottoneRecensioni"><a class="BottoneDestra" href="recensioni.html">Recensioni</a></li>
            <li role="menuitem" id="BottoneSondaggiPre"><a class="BottoneDestra" href="http://localhost:8080/api/sondaggioPre">Sondaggio pre-evento</a></li>
            <li role="menuitem" id="BottoneSondaggiPost"><a class="BottoneDestra" href="http://localhost:8080/api/sondaggioPost">Sondaggio post-evento</a></li>
          </ul>
        </nav>

        <!--Box video-->
        <div id="video"><!--QUI VA IL VIDEO--></div>

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

<script>
import Carousel from "./components/carousel/Carousel.vue";

export default {
  name: "App",
  components: { Carousel },
  data: () => ({
    slides: [
      "https://img.evbuc.com/https%3A%2F%2Fcdn.evbuc.com%2Fimages%2F690023249%2F12679432051%2F1%2Foriginal.20240205-123236?w=940&auto=format%2Ccompress&q=75&sharp=10&rect=0%2C0%2C1000%2C500&s=76094057b62b34c4ed18f1136e339d01",
      "https://ddragon.leagueoflegends.com/cdn/img/champion/splash/Aphelios_0.jpg",
      "https://ddragon.leagueoflegends.com/cdn/img/champion/splash/Aphelios_1.jpg",
      "https://ddragon.leagueoflegends.com/cdn/img/champion/splash/Aphelios_18.jpg",
      "https://ddragon.leagueoflegends.com/cdn/img/champion/splash/Aphelios_20.jpg",
    ],
  }),
};
</script>

<style scoped>
  @import 'stylesheet.css';
</style>
