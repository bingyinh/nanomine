<template>
  <div class="simtools">
    <h1>{{ msg }}</h1>
    <v-container>
      <v-layout row wrap>
        <v-flex d-flex xs12>
          <v-card-text>
            <p class="text-xs-left">
              We provide online version of physics-based simulation tools that take advantage of our most recent
              development in viscoelasticity and dielectric relaxation theories for interphase analysis in nanocomposites,
              statistical structure-property correlation models, as well as materials optimization methods.
            </p>
          </v-card-text>
        </v-flex>
        <v-flex d-flex xs1>
        </v-flex>
        <v-flex d-flex xs12 sm5 md4>
          <v-img src="/nmstatic/img/prop_simu.png" aspect-ratio="1.7" contain xs12 sm6></v-img>
        </v-flex>
        <v-flex d-flex xs12 sm6 md7>
          <v-card-text>
            <h4 class="text-xs-left">
              <span>
                <a href="http://reccr.chem.rpi.edu/polymerizer/index.html" target="_blank">
                  Polymerizer
                </a>
              </span>
            </h4>
            <p class="text-xs-left">
              Polymerizer is an online application for prediction of surface energy for nanocomposites featuring
              user-defined polymer structure input and structure-property MQSPR. This web app is in collaboration with
              and maintained by Rensselaer Polytechnic Institute.
            </p>
          </v-card-text>
        </v-flex>
        <v-flex d-flex xs1>
        </v-flex>
        <v-flex d-flex xs12 sm5 md4>
          <v-img src="/nmstatic/img/FEA_dielectrics.png" contain xs12 sm6></v-img>
        </v-flex>
        <v-flex d-flex xs12 sm6 md7>
          <v-card-text>
            <h4 class="text-xs-left">
              <span>
                <!--router-link to="/FEA2D"-->
                <h3>Prediction of Dielectric Constant in Nanodielectrics</h3>
                <!--/router-link-->
              </span>
            </h4>
            <div class="text-xs-left">
              <h3>Under construction. Not currently available.</h3>
              This web app simulates the dielectric spectroscopy in nanocomposites with explicit implementation of
              microstructure dispersion as well as interphase relaxation model. This project is in collaboration with
              Rensselaer Polytechnic Institute.
           </div>
          </v-card-text>
        </v-flex>
        <v-flex d-flex xs1>
        </v-flex>
        <v-flex d-flex xs12 sm5 md4>
          <v-img src="/nmstatic/img/MCFEA.jpg" contain xs12 sm6></v-img>
        </v-flex>
        <v-flex d-flex xs12 sm6 md7>
          <v-card-text>
            <h4 class="text-xs-left">
              <span>
                <!--router-link to="/MCFEA"-->
                <h3>Integration of Monte Carlo and Finite Element Analysis in Nanodielectrics</h3>
                <!--/router-link-->
              </span>
            </h4>
            <div class="text-xs-left">
            <h3>Under construction. Not currently available.</h3>
              This web app simulates the charge transport in dielectric polymers and their nanocomposites by compling
              finite element analysis and Monte Carlo. This project is in collaboration with Rensselaer Polytechnic
              Institute.
            </div>
          </v-card-text>
        </v-flex>
        <v-flex d-flex xs2>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
export default {
  name: 'SimTools',
  data () {
    return {
      msg: 'Material Property Simulation'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  img {
    width: 240px;
    height: 220px;
  }
  h3 {
    color: #096ff4;
  }
  h4 {
    text-transform: uppercase;
  }
  h1 {
    margin-top: 10px;
    background-color: black;
    color: white;
  }
</style>
