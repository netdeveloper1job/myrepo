<template>
  
    <v-layout
      text-center
      wrap
    >
	<v-flex md12 lg12 xll2 xs4 sm6>
	<v-container>
	<v-content class="mainWrapper white">
    <section class="hbanner">
      <v-img
        :src="require(`@/assets/WooBii-hbanner.jpg`)"
        :lazy-src="require(`@/assets/WooBii-hbanner.jpg`)"
        gradient="to top right, rgba(0,0,0,.4), rgba(0,0,0,.4)"
        class="grey lighten-2"
        contain
		sm4
		xs4
      >
	  <v-layout slot="placeholder" fill-height align-center justify-center ma-0></v-layout>
	  <v-layout
          fill-height
          column
          align-center
          justify-center
          class="text-xs-center white--text px-4 responsive"
        >
          <h1 class="display-1 text-uppercase">
            Der Digitale Newsroom
            <br>für christliche Gemeinden
          </h1>
          <div
            class="subheading my-4"
          >Verbreiten Sie sekundenschnell Ihre wichtigsten Botschaften, News, Jobangebote und Eventankündigungen in deutschsprachigen christlichen Qualitätsmedien und im beliebten Kirchenfinder.</div>
          <v-tooltip top>
            <v-btn
              depressed
              dark
              large
              href="/pre-made-themes"
              slot="activator"
              color="#FC732D"
            >Jetzt kostenlos testen</v-btn>
            <span>Jetzt kostenlos testen</span>
          </v-tooltip>
        </v-layout>
	  </v-img>
		</section>
		
		
		<section class="hpackages">
      <v-layout column wrap class="py-4">
        <v-flex  text-xs-center>
          <h2
            class="headline mb-4"
          >WooBii - die crossmediale Lösung für lhre christliche Medienarbeit.</h2>
        </v-flex>
        <v-flex xs12>
          <v-container grid-list-xl class="py-0">
            <v-layout row wrap class="hidden-md-and-down">
              <v-flex d-flex xs12 md4 v-for="item in packages">
                <v-card flat color="grey lighten-4">
                  <v-card-title class="justify-center py-4" style="color: #fff;">
                    <div class="text-xs-center">
                      <p style="font-size:19px">{{item.package_name}}</p>
                      <span style="font-size: 36px;">
					  
                        {{item.package_price}}    
											
                      </span>
					 <span><v-icon color="#fff">euro_symbol</v-icon> pro Monat</span>
					 <br><br>
                      <v-tooltip top>
                        <v-btn depressed dark slot="activator" color="#FC732D" style="padding: 0 28px; height:40px; text-transform:none !important;">{{item.description}}</v-btn>
                        <span>{{item.description}}</span>
                      </v-tooltip>
                    </div>
                  </v-card-title>
                  <v-list class="transparent">
                    <v-list-tile v-for="list in item.pointList">
                      <v-list-tile-action>
                        <v-icon size="20" color="#FC732D">done</v-icon>
                      </v-list-tile-action>
                      <v-list-tile-content>
                        <v-list-tile-subtitle>{{list.point}}</v-list-tile-subtitle>
                      </v-list-tile-content>
                    </v-list-tile>
                  </v-list>
                </v-card>
              </v-flex>
            </v-layout>
            <v-layout class="hidden-lg-and-up">
              <v-flex x12>
                <v-tabs
                  centered
                  grow
                  v-model="active"
                  color="white"
                  light
                  slider-color="indigo "
                  class="mobilePackageTab"
                >
                  <v-tab class="grey lighten-4" v-for="item in packages">{{item.package_name}}</v-tab>
                  <v-tab-item v-for="item in packages">
                    <v-card flat>
                      <v-card-text>
                        <v-layout>
                          <v-flex d-flex xs12>
                            <v-card flat color="grey lighten-4">
                              <v-card-title class="justify-center py-4" style="color: #fff;">
                                <div class="text-xs-center">
                                  <h3 class="headline mb-3">{{ item.package_name }}</h3>
                                  <h3 class="headline mb-3">
                                    {{ item.package_price }}
                                    <v-icon color="#fff">euro_symbol</v-icon>pro Mouat
                                  </h3>
                                  <v-tooltip top>
                                    <v-btn
                                      depressed
                                      dark
                                      slot="activator"
                                      color="#FC732D"
                                    >Kostenlos nutzen</v-btn>
                                    <span>Kostenlos nutzen</span>
                                  </v-tooltip>
                                </div>
                              </v-card-title>
                              <v-list class="transparent">
                                <v-list-tile v-for="list in item.pointList">
                                  <v-list-tile-action>
                                    <v-icon size="20" color="#FC732D">done</v-icon>
                                  </v-list-tile-action>
                                  <v-list-tile-content>
                                    <v-list-tile-title>{{ list.point }}</v-list-tile-title>
                                  </v-list-tile-content>
                                </v-list-tile>
                              </v-list>
                            </v-card>
                          </v-flex>
                        </v-layout>
                      </v-card-text>
                    </v-card>
                  </v-tab-item>
                </v-tabs>
              </v-flex>
            </v-layout>
          </v-container>
        </v-flex>
      </v-layout>
    </section>
	  </v-content>
		  </v-container>
	  </v-flex>
    </v-layout>

</template>

<script>
import axios from 'axios'
export default {
  name: 'App',
  data: () => ({
    packages: ''
  }),
  mounted () {
    this.packageList()
  },
  methods: {
    packageList () {
      var e = this
      axios
        .get('/adminglobal/getallpackages')
        .then(function (response) {
          if (response.data.status == true) {
            console.log(response.data.packages)
            e.packages = response.data.packages
          }
        })
        .catch(function (error) {
          if (error.response) {
            // The request was made and the server responded with a status code
            // that falls out of the range of 2xx
            console.log(error.response.data)
            console.log(error.response.status)
            console.log(error.response.headers)
          } else if (error.request) {
            console.log(error.request)
          } else {
            console.log('Error', error.message)
          }
        })
    }
  }
}
</script>
