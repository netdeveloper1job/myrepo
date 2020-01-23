<template>
<v-layout
      text-center
      wrap
    >
	<v-flex md12 lg12 xll2 xs4 sm6>
  <v-content class="mainWrapper white">
    <section class="newsRoomSectionTop" style="border-bottom: 1px solid #EAEAEA;">
      <v-btn fab dark small color="primary" @click.stop="drawer = !drawer" class="btnFilter">
        <v-icon dark>filter_list</v-icon>
      </v-btn>
	  <!-- MOBILE SIDEBAR STARTS -->	
	<v-navigation-drawer v-model="drawer" fixed temporary>
        <v-card flat color="white" class="mobileFilterWarp">
			<v-card-text>
                <v-form>
				<v-autocomplete
				  	v-if="drawer"
                    :loading="searchLoading"
                    :items="newsroomTitle"
                    item-text="title"
                    item-value="link"
                    :search-input.sync="newssearch"
                    @input="pageRedirect"
                    cache-items
                    flat
                    hide-no-data
                    hide-details
                    placeholder="Suche"
                    append-icon="search"
                    solo
                    style="margin-bottom: 16px;"
				></v-autocomplete>
				<v-autocomplete
					v-if="drawer"
					id="land"
                    label="Land"
                    placeholder="Land auswählen"
                    :items="LandList"
                    item-text="name"
                    @input="searchfilter"
                    v-model="sland"
                    item-value="id"
					return-object
					@change="countryonchangefunctions"
					v-on:click.native = "clickreset"
				></v-autocomplete>				
				<v-autocomplete
					v-if="drawer && isCountrynotselected && st == 'Bundesland'"
					id="Bundesland"
                    label="Bundesland"
                    placeholder="bundesland auswählen"
                    :items="StateItems"
                    item-text="StateName"
                    @input="searchfilter"
                    item-value="ID"
					v-model="stateids"
					@change="statechangefunctions"
					v-on:click.native = "clickreset1"
				></v-autocomplete>	
				<v-autocomplete
					v-if="drawer && isCountrynotselected && st == 'Bundesländer'"
					id="Bundesland"
                    label="Bundesländer"
                    placeholder="bundesland auswählen"
                    :items="StateItems"
                    item-text="StateName"
                    @input="searchfilter"
                    item-value="ID"
					v-model="stateids"
					@change="statechangefunctions"
					v-on:click.native = "clickreset2"
				></v-autocomplete>
				<v-autocomplete
					v-if="drawer && isCountrynotselected && st == 'Kantone'"
					id="Bundesland"
                    label="Kantone"
                    placeholder="bundesland auswählen"
                    :items="StateItems"
                    item-text="StateName"
                    @input="searchfilter"
                    item-value="ID"
					v-model="stateids"
					@change="statechangefunctions"
					v-on:click.native = "clickreset3"
				></v-autocomplete>
				<v-autocomplete
					v-if="drawer && isCountrynotselected"
					id="Stadt"
					label="Stadt"
                    :loading="searchLoading"
                    :items="cityItems"
                    item-text="cityName"
                    item-value="cityId"
                    @input="searchfilter"
				    hide-no-data
				    hide-details
                    placeholder="Stadt"
                    style="margin-bottom: 16px;"
					v-model="selectedcityid"
					@change="citychangefunctions"
					v-on:click.native = "clickreset4"
				></v-autocomplete>
				<v-autocomplete	
					v-if="drawer && isCountrynotselected && IsCityselectedForMunicipalty"
					id="Municipality"
                    :loading="searchLoading"
                    :items="MunicipalityItems"
                    item-text="municipality_name"
                    item-value="municipality_id"
                    @input="searchfilter"
				    hide-no-data
				    hide-details
                    placeholder="Ortsteil"
                    style="margin-bottom: 16px; padding-top: 0px; margin-top: -10px;"
					v-model="selectedmunicipalityid"
					@change="municipalitychangefunctions"
					v-on:click.native = "clickreset5"
				></v-autocomplete>
				<!--<v-autocomplete
				  	v-if="!drawer"
					label="Stadt/Ort"
                    :loading="searchLoading"
                    :items="citywithmuncipality"
                    item-text="location"
                    item-value="locationvalue"
                    :search-input.sync="citysearchhere"
                    @input="searchfilter"
					@change="listofmuncipaltyforpostcode"
                    cache-items
                    hide-no-data
                    hide-details
					v-model="citywithmuncipalityid"
                    placeholder="Stadt/Ort"
                    append-icon="search"
                    style="margin-bottom: 16px;"
					clearable
				></v-autocomplete>
				<v-autocomplete
					v-if="!drawer"
					label="PLZ"
                    :loading="searchLoading"
                    :items="postcodes"
                    item-text="title"
                    item-value="id"
                    :search-input.sync="postcodesearch"
                    @input="searchfilter" 
				    hide-no-data
				    hide-details
                    placeholder="Search PLZ"
					v-model="plzorpostcode"
                    style="margin-bottom: 16px;"
					clearable
					append-icon="search"
					:disabled="isCountrynotselected"
					@change="postcodechangefunctions"
				></v-autocomplete>	-->	
				<v-autocomplete
					v-if="drawer && isCountrynotselected"
					id="plz"
					label="Postleitzahl"
                    :loading="searchLoading"
                    :items="postcodes"
                    item-text="title"
                    item-value="id"
                    @input="searchfilter" 
				    hide-no-data
				    hide-details
                    placeholder="PLZ"
					v-model="plzorpostcode"
                    style="margin-bottom: 16px;"
					@change="postcodechangefunctions"
					v-on:click.native = "clickreset6"
				></v-autocomplete>				  
				<!--<v-autocomplete
					v-if="!drawer"
                    :items="GemeindetypeList"
                    item-text="CategoryName"
                    item-value="ID"
                    label="Gemeindetyp:"
                    placeholder="Bitte auswählen"
                    @input="searchfilter"
                    v-model="gemeindetyp"
					clearable
					:disabled="isCountrynotselected"
					@change="listofsubChurchetype"
				></v-autocomplete>	
				<v-autocomplete
					v-if="!drawer"
                    :items="GemeindetypesubList"
                    item-text="SubcatName"
                    item-value="ID"
                    label="Gemeindesubtyp"
                    placeholder="Bitte auswählen"
                    @input="searchfilter"
                    v-model="gemeindesubtyp"
					clearable
					:disabled="isCountrynotselected"
				></v-autocomplete>	-->
				<v-autocomplete
					v-if="drawer && isCountrynotselected"
                    label="Ressort"
                    id="Ressort"
                    placeholder="Bitte auswählen"
                    :items="category"
                    item-text="category"
                    item-value="id"
                    @change="categorySelected"
					@click = "clickreset7"
                    @input="searchfilter"
                    v-model="scategory"					
					
					
				></v-autocomplete>
				<v-autocomplete
					v-if="drawer && isCountrynotselected"
                    id="Subressort"
                    label="Subressort"
                    placeholder="Stichwort einfügen"
                    :items="subcategory"
                    item-text="subcategory"
                    item-value="id"
                    @input="searchfilter"
                    v-model="ssubcategory"
					@click = "clickreset8"
					
				></v-autocomplete>
				  
				<v-combobox
					v-if="drawer && isCountrynotselected"
					v-model="chips"
					:items="themenItems"
					chips
					clearable
					label="Themen"
					multiple
					solo
					item-text="Title"
					item-value="ID"
					@input="searchfilter"
					
				  >
					<template v-slot:selection="{ attrs, item, select, selected }">
					  <v-chip
						v-bind="attrs"
						:input-value="selected"
						close
						@click="select"
						@click:close="remove(item)"
					  >
						<strong>{{ item }}</strong>&nbsp;
						<span>(interest)</span>
					  </v-chip>
					</template>
				</v-combobox>

                  <!--<v-autocomplete
                    label="People:"
                    placeholder="People..."
                    :items="publisher"
                    item-text="title"
                    item-value="pid"
                    @input="searchfilter"
                    v-model="spublisher"
                  ></v-autocomplete>-->
                </v-form>
              </v-card-text>
        </v-card>
      </v-navigation-drawer>
      <v-container class="white py-0">
        <v-layout>
          <v-flex xs6>
          </v-flex>
          <v-flex xs6>
            <v-btn flat class="grey lighten-2 ma-0 right black--text">
              <v-icon class="mr-2">folder</v-icon>0 Lightbox
            </v-btn>
          </v-flex>
        </v-layout>
      </v-container>
    </section>
    <section class="newsRoomSection">
      <v-container fluid grid-list-xl>
        <v-layout row wrap justify-center>
          <v-flex xs12 md3>
            <v-card flat color="white" class="newsRoomFilter">
              <v-card-text>
                <v-form>
				<v-autocomplete
				  	v-if="!drawer"
                    :loading="searchLoading"
                    :items="newsroomTitle"
                    item-text="title"
                    item-value="link"
                    :search-input.sync="newssearch"
                    @input="pageRedirect"
                    cache-items
                    flat
                    hide-no-data
                    hide-details
                    placeholder="Suche"
                    append-icon="search"
                    solo
                    style="margin-bottom: 16px;"
				></v-autocomplete>
				<v-autocomplete
					v-if="!drawer"
					id="land"
                    label="Land"
                    placeholder="Land auswählen"
                    :items="LandList"
                    item-text="name"
                    @input="searchfilter"
                    v-model="sland"
                    item-value="id"
					return-object
					@change="countryonchangefunctions"
					v-on:click.native = "clickreset"
				></v-autocomplete>				
				<v-autocomplete
					v-if="!drawer && isCountrynotselected && st == 'Bundesland'"
					id="Bundesland"
                    label="Bundesland"
                    placeholder="bundesland auswählen"
                    :items="StateItems"
                    item-text="StateName"
                    @input="searchfilter"
                    item-value="ID"
					v-model="stateids"
					@change="statechangefunctions"
					v-on:click.native = "clickreset1"
				></v-autocomplete>	
				<v-autocomplete
					v-if="!drawer && isCountrynotselected && st == 'Bundesländer'"
					id="Bundesland"
                    label="Bundesländer"
                    placeholder="bundesland auswählen"
                    :items="StateItems"
                    item-text="StateName"
                    @input="searchfilter"
                    item-value="ID"
					v-model="stateids"
					@change="statechangefunctions"
					v-on:click.native = "clickreset2"
				></v-autocomplete>
				<v-autocomplete
					v-if="!drawer && isCountrynotselected && st == 'Kantone'"
					id="Bundesland"
                    label="Kantone"
                    placeholder="bundesland auswählen"
                    :items="StateItems"
                    item-text="StateName"
                    @input="searchfilter"
                    item-value="ID"
					v-model="stateids"
					@change="statechangefunctions"
					v-on:click.native = "clickreset3"
				></v-autocomplete>
				<v-autocomplete
					v-if="!drawer && isCountrynotselected"
					id="Stadt"
					label="Stadt"
                    :loading="searchLoading"
                    :items="cityItems"
                    item-text="cityName"
                    item-value="cityId"
                    @input="searchfilter"
				    hide-no-data
				    hide-details
                    placeholder="Stadt"
                    style="margin-bottom: 16px;"
					v-model="selectedcityid"
					@change="citychangefunctions"
					v-on:click.native = "clickreset4"
				></v-autocomplete>
				<v-autocomplete	
					v-if="!drawer && isCountrynotselected && IsCityselectedForMunicipalty"
					id="Municipality"
                    :loading="searchLoading"
                    :items="MunicipalityItems"
                    item-text="municipality_name"
                    item-value="municipality_id"
                    @input="searchfilter"
				    hide-no-data
				    hide-details
                    placeholder="Ortsteil"
                    style="margin-bottom: 16px; padding-top: 0px; margin-top: -10px;"
					v-model="selectedmunicipalityid"
					@change="municipalitychangefunctions"
					v-on:click.native = "clickreset5"
				></v-autocomplete>
				<!--<v-autocomplete
				  	v-if="!drawer"
					label="Stadt/Ort"
                    :loading="searchLoading"
                    :items="citywithmuncipality"
                    item-text="location"
                    item-value="locationvalue"
                    :search-input.sync="citysearchhere"
                    @input="searchfilter"
					@change="listofmuncipaltyforpostcode"
                    cache-items
                    hide-no-data
                    hide-details
					v-model="citywithmuncipalityid"
                    placeholder="Stadt/Ort"
                    append-icon="search"
                    style="margin-bottom: 16px;"
					clearable
				></v-autocomplete>
				<v-autocomplete
					v-if="!drawer"
					label="PLZ"
                    :loading="searchLoading"
                    :items="postcodes"
                    item-text="title"
                    item-value="id"
                    :search-input.sync="postcodesearch"
                    @input="searchfilter" 
				    hide-no-data
				    hide-details
                    placeholder="Search PLZ"
					v-model="plzorpostcode"
                    style="margin-bottom: 16px;"
					clearable
					append-icon="search"
					:disabled="isCountrynotselected"
					@change="postcodechangefunctions"
				></v-autocomplete>	-->	
				<v-autocomplete
					v-if="!drawer && isCountrynotselected"
					id="plz"
					label="Postleitzahl"
                    :loading="searchLoading"
                    :items="postcodes"
                    item-text="title"
                    item-value="id"
                    @input="searchfilter" 
				    hide-no-data
				    hide-details
                    placeholder="PLZ"
					v-model="plzorpostcode"
                    style="margin-bottom: 16px;"
					@change="postcodechangefunctions"
					v-on:click.native = "clickreset6"
				></v-autocomplete>				  
				<!--<v-autocomplete
					v-if="!drawer"
                    :items="GemeindetypeList"
                    item-text="CategoryName"
                    item-value="ID"
                    label="Gemeindetyp:"
                    placeholder="Bitte auswählen"
                    @input="searchfilter"
                    v-model="gemeindetyp"
					clearable
					:disabled="isCountrynotselected"
					@change="listofsubChurchetype"
				></v-autocomplete>	
				<v-autocomplete
					v-if="!drawer"
                    :items="GemeindetypesubList"
                    item-text="SubcatName"
                    item-value="ID"
                    label="Gemeindesubtyp"
                    placeholder="Bitte auswählen"
                    @input="searchfilter"
                    v-model="gemeindesubtyp"
					clearable
					:disabled="isCountrynotselected"
				></v-autocomplete>	-->
				<v-autocomplete
					v-if="!drawer && isCountrynotselected"
                    label="Ressort"
                    id="Ressort"
                    placeholder="Bitte auswählen"
                    :items="category"
                    item-text="category"
                    item-value="id"
                    @change="categorySelected"
					@click = "clickreset7"
                    @input="searchfilter"
                    v-model="scategory"					
					
					
				></v-autocomplete>
				<v-autocomplete
					v-if="!drawer && isCountrynotselected"
                    id="Subressort"
                    label="Subressort"
                    placeholder="Stichwort einfügen"
                    :items="subcategory"
                    item-text="subcategory"
                    item-value="id"
                    @input="searchfilter"
                    v-model="ssubcategory"
					@click = "clickreset8"
					
				></v-autocomplete>
				  
				<v-combobox
					v-if="!drawer && isCountrynotselected"
					v-model="chips"
					:items="themenItems"
					chips
					
					label="Themen"
					multiple
					solo
					item-text="Title"
					item-value="ID"
					@input="searchfilter"
					
				  >
					<template v-slot:selection="{ attrs, item, select, selected }">
					  <v-chip
						v-bind="attrs"
						:input-value="selected"
						close
						@click="select"
						@click:close="remove(item)"
					  >
						<strong>{{ item }}</strong>&nbsp;
						<span>(interest)</span>
					  </v-chip>
					</template>
				</v-combobox>

                  <!--<v-autocomplete
                    label="People:"
                    placeholder="People..."
                    :items="publisher"
                    item-text="title"
                    item-value="pid"
                    @input="searchfilter"
                    v-model="spublisher"
                  ></v-autocomplete>-->
                </v-form>
              </v-card-text>
            </v-card>
          </v-flex>
          <v-flex xs12 md9>
            <v-layout row wrap>
              <v-flex d-flex xs12 md4 v-if="newsroomList" v-for="news in newsroomList">
                <v-card flat color="grey lighten-4">
                   <router-link :to="'/newsroom/'+news.slug" class="caption black--text">
                    <v-img
                      v-if="news.bannerimage"
                      :lazy-src="'http://dev.woobii.com/admin/'+news.bannerimage"
                      :src="'http://dev.woobii.com/admin/'+news.bannerimage"
                      class="newsImage"
                    >
                      <v-layout pa-2 column fill-height class="lightbox white--text">
                       <v-flex shrink>
                     <!--     <span>
                            <i class="far fa-copyright" style="font-size:9px;margin-left: 10px;margin-top: 10px !important;"></i>
							
							
                          </span>-->
						 
						  
						 
						  
                        </v-flex>
                      </v-layout>
                    </v-img>
                    <v-img
                      v-else
                      :src="require(`@/assets/woobii-banner.jpg`)"
                      :lazy-src="require(`@/assets/woobii-banner.jpg`)"
                    ></v-img>
                  </router-link>
				   <div class="image-copyright"> 
														<div class="image-copyright-view">©</div>
														<div class="image-copyright-hover"> 
											<p>©</p>
											<p style="margin-top: -12.5px;margin-left: 20px;"> Bildrechte : {{news.image_rights}} </p>
															<p style="margin-top:1.5px;margin-left: 20px;"> Bildquelle : <a v-bind:href="news.imagesourcelink"target="_blank">{{ news.imagesourcelabel }}</a></p>
															
														<p style="margin-top: 2.2px;margin-left: 20px;"> Lizenz : <a href="http://dev.woobii.com/" target="_blank">{{news.license}}</a> </p>
														
															 </div>
														 </div>
                  <v-card-title class="pb-0">
                    <h4 class="caption text-uppercase mb-2" style="width:100%;position:relative; bottom:2px;">
                      <span class="font-weight-black teaser-link-info">{{news.category}}</span>
                      <span style="color:#f18825;">|</span> {{news.subcategory}}
                    </h4>
					<h4 class="caption text-uppercase mb-2 st" style="position: relative;bottom: 10px;text-transform:none !important">
                      <router-link
                        :to="'/newsroom/'+news.slug"
                        class="caption black--text font-weight-black"
                      >{{news.title}}</router-link>
                    </h4>
                  </v-card-title>
				  

                  <v-card-actions class="py-2 px-3" style="margin-bottom:15px;">
                 
				  <div style="position: absolute;bottom:1px;">
                    <h4 class="caption" style="padding-bottom: 7px;">
                      <router-link :to="'/Kirchenfinder/'+news.comm_slug" class="caption black--text">{{news.comm_name}}</router-link>
                      | {{ news.date | moment("DD.MM.YYYY")}}
                    </h4>
					</div>
                  </v-card-actions>
                </v-card>
              </v-flex>
			<!--   <h1 v-bind:class="[isFinished ? 'finish' : 'load-more']" @click='newsroomlistforloadmore()' v-cloak>{{ buttonText }}</h1>-->
            </v-layout>
            <div class="text-xs-center">
              <!-- <v-pagination
                v-model="page"
                :length="totalcount"
                :total-visible="5"
                @input="onPageChange"
              ></v-pagination>-->
            </div>
          </v-flex>
        </v-layout>
      </v-container>
 
	  <!--<KirchenFinder></KirchenFinder>-->
    </section>
	 <infinite-loading @infinite="newsroomlist"><div slot="no-more"></div>
  <div class="show" slot="no-results"></div></infinite-loading>
  </v-content>
</v-flex>
    </v-layout>
</template>

<script>
import $ from 'jquery'
import axios from 'axios'
import KirchenFinder from './KirchenFinder.vue'
export default {
  name: 'App',
  data () {
    return {
		isFinished: false,
    row: 0, // Record selction position
    rowperpage: 3, // Number of records fetch at a time
    buttonText: 'WEITERE NEWS',
      // baseUrl: process.env.BASE_URL
	  chips: [],
	  themenItems: [],
      isMobile: false,
      drawer: null,
      searchLoading: false,
      newsroomTitle: [],
      newssearch: '',
      mnewssearch: '',
      postcodesearch: '',
      citysearchhere: '',
      municipalitySearch: '',
      newsroomList: [],
      GemeindetypeList: [],
	  MunicipalityItems:[],
	  MunicipalityPostcodeItems:[],
	  PostcodeMunicipalityItems:[],
	  postcodes:[],
      LandList: [],
      cityItems: [],
      StateItems: [],
      category: [],
      subcategory: [],
      publisher: [],
      citywithmuncipality: [],
      land: '',
      page: 1,
      totalcount: 0,
      sland: '000',
      spublisher: '',
      ssubcategory: '',
      scategory: '',
      gemeindetyp: '',
      themenforsearch: [],
	  stateids:'000',
	  citywithmuncipalityid:'',
	  plzorpostcode:'',
	  isCountrydropdown:true,
	  isCountrynotselected:false,	
	  GemeindetypesubList: '',
	  selectedcityid:'000', 	  
	  selectedmunicipalityid:'', 
	  gemeindesubtyp:'',
	  st:'',
	  IsCityselectedForMunicipalty:false,	 
	 pagginginfinte:0,	  
    }
  },
  mounted () {
   // this.newsroomlist()
    // this.listofmuncipalty()
	// this.listofmuncipaltyforpostcode()
	// this.listofpostcodeformuncipality()
    this.listofcountry()
    this.listofcategory()
    this.listofsubcategory()
    this.listofpublisher()
    this.listofthemens()
	this.listOfChurchetype()
	this.bydefaultgrey()
	// this.listofcity()
	// this.listofStates()
  },
  beforeDestroy () {},
  watch: {
    newssearch (val) {
      if (val == null) {
        return
      }
      if (val.length >= 3) {
        val && val !== this.select && this.querySelections(val)
      } else {
        this.newsroomTitle = []
      }
    },
    mnewssearch (val) {
      if (val == null) {
        return
      }
      if (val.length >= 3) {
        val && val !== this.select && this.querySelections(val)
      } else {
        this.newsroomTitle = []
      }
    },
	postcodesearch (val) {
      if (val == null) {
        return
      }
      if (val.length >= 1) {
        val && val !== this.select && this.SearchPlz(val)
      } else {
        this.postcodes = []
      }
    },
	citysearchhere (val) {
	  if (val == null) {
        return
      }
      if (val.length >= 1) {
        val && val !== this.select && this.searchcitynames(val)
      } else {
        this.cityItems = []
      }
    },
	municipalitySearch (val) {
	  if (val == null) {
        return
      }
      if (val.length >= 1) {
        val && val !== this.select && this.searchmunicipalitynames(val)
      } else {
        this.MunicipalityItems = []
      }
    },
  },
  methods: {
	    bydefaultgrey:function(){	
	
			$('#land').css('color', 'lightgrey');
			$('#Bundesland').css('color', 'lightgrey');
			$('#Stadt').css('color', 'lightgrey');
			$('#Municipality').css('color', 'lightgrey');
			$('#plz').css('color', 'lightgrey');
			
  },
    pageRedirect: function (event) {
      this.$router.push('/newsroom/' + event)
      this.churchdata(this.$route.params.slug)
    },
    querySelections (v) {
      var self = this
      this.searchLoading = true
      // Simulated ajax query
      axios
        .get('/churcheview/newsroombyname?t=' + v)
        .then(function (response) {
          if (response.data.status == true) {
            self.newsroomTitle = response.data.newsroom
            self.searchLoading = false
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
    },
    categorySelected: function (event) {
      this.listofsubcategory(event)
    },
    onPageChange: function (pageNum) {
      let all = true
      this.newsroomlist(pageNum - 1, all)
    },
    searchfilter: function (pageNum = 1) {
      this.newsroomlist(0, true)
    },
    newsroomlist: function ($state,pageNum = 0, all = true) {
      var e = this
      var strQuery = ''
	  // console.log(e.chips)
	  var selectedcol = []
	  for (var i = 0; i < e.chips.length; i++) {
      
        selectedcol.push(e.chips[i].ID)
      }

	   if (e.sland.id) {
        strQuery += '&sland=' + e.sland.id
      }
	  if (e.stateids) {
        strQuery += '&stateids=' + e.stateids
      } 
	  // if (e.citywithmuncipalityid) {
        // strQuery += '&citywithmuncipalityid=' + e.citywithmuncipalityid
      // }	
	  if (e.selectedcityid) {
        strQuery += '&cityid=' + e.selectedcityid
      } 
	  if (e.selectedmunicipalityid) {
        strQuery += '&municipalityid=' + e.selectedmunicipalityid
      } 	  
      if (e.plzorpostcode) {
        strQuery += '&plzorpostcode=' + e.plzorpostcode
      }
      if (e.gemeindetyp) {
        strQuery += '&gemeindetyp=' + e.gemeindetyp
      }
      if (e.scategory) {
        strQuery += '&scategory=' + e.scategory
      }
      if (e.ssubcategory) {
        strQuery += '&ssubcategory=' + e.ssubcategory
      }
      if (e.spublisher) {
        strQuery += '&spublisher=' + e.spublisher
      }
      if (e.chips) {
        strQuery += '&themen=' + selectedcol.toString()
      }	  
	  if (e.city) {
        strQuery += '&city=' + e.city
      }

      axios
        .get(
          '/churcheview/newsroomlist?p=' + e.pagginginfinte + strQuery
        )
        .then(function (response) {
          if (response.data.status == true) {
              if(response.data.newsroom.newsroomList){
				 e.pagginginfinte += 1;
				e.newsroomList.push(...response.data.newsroom.newsroomList);
				$state.loaded();
			  } else{
				  $state.complete();
			 setTimeout(function(){
		   $state.reset();
	  },5000)
			  }
            if (response.data.newsroom.newsroomcount > 0) {
				
              if (response.data.newsroom.newsroomcount >= 12) {
                let count = response.data.newsroom.newsroomcount

                let div = count / 12
                let mod = count % 12

                if (mod >= 0) {
                  e.totalcount = div + 1
                } else {
                  e.totalcount = div
                }
              } else {
                e.totalcount = 0
              }
            } else {
              e.totalcount = 0
            }
          }
        })
    },
	newsroomlistforloadmore: function () {
      var e = this
      var strQuery = ''
	  // console.log(e.chips)
      axios
        .get(
          '/churcheview/newsroomlist?row=' + this.row + '&rowperpage=' + this.rowperpage + strQuery
        )
        .then(function (response) {
          if (response.data.status == true) {
			  if(e.row==0){
				 e.newsroomList = ""; 
			  }
            // e.newsroomList = response.data.newsroom.newsroomList
			// console.log(response.data.newsroom.newsroomList);
			if(response.data.newsroom.newsroomList){
				e.row+=e.rowperpage;
				var len = e.newsroomList.length;
				if(len > 0){
					e.buttonText = "Loading ...";
					setTimeout(function() {
						e.buttonText = "WEITERE NEWS";
							// Loop on data and push in posts
							for (let i = 0; i < response.data.newsroom.newsroomList.length; i++){
							   e.newsroomList.push(response.data.newsroom.newsroomList[i]); 
							} 
						 },500);
					   }else{
						  e.newsroomList = response.data.newsroom.newsroomList;
					   }
					}else{
					   e.buttonText = "No more records avaiable.";
					   e.isFinished = true;
				 }
				// alert(len)
			}            
          })
    },
    listofcountry: function () {
      var e = this
      axios
        .get('/adminglobal/getallcountry')
        .then(function (response) {
          if (response.data.status == true) {
            e.LandList = response.data.countries.country
			e.LandList.unshift({name:"Alle Länder", id:'000'});
          }
        })
    },			
    listofStates: function (countryid) {
      var e = this
      axios
        .get('/adminglobal/getAllStates/?id=' + countryid)
        .then(function (response) {
          if (response.data.status == true) {				
            e.StateItems = response.data.states.state;
			if(countryid == 2){
			e.StateItems.unshift({StateName:"Alle Bundesländer", ID:'000'});
			}else if(countryid == 3){
				e.StateItems.unshift({StateName:"Alle Kantone", ID:'000'});
			}else if(countryid == 1){
				e.StateItems.unshift({StateName:"Alle Bundesland", ID:'000'});
			}
          }
        });
    },
    searchcitynames: function () {
      var e = this
	  var strQuery = ''
	  // if(valuesof){
		   // strQuery += '?value=' + valuesof
	  // }
	  if (e.sland.id) {
        strQuery += '?countryid=' + e.sland.id
      }
	  if (e.stateids) {
        strQuery += '&stateids=' + e.stateids
      } 
      axios
        .get('/adminglobal/getcitybysearch/'+strQuery)
        .then(function (response) {
          if (response.data.status == true) {
            e.cityItems = response.data.cities.citydata;
			// if(e.stateids=='000'){
			// e.cityItems.unshift({cityName:"Alle Städte", cityId:'000'});
			// }
			if(e.cityItems.length == 1 && e.sland.name!="Schweiz"){
				 e.IsCityselectedForMunicipalty=true;
				$('#Stadt').css('color', 'black');
				 $('#Municipality').css('color', 'black');
				$('#plz').css('color', 'black');
				e.selectedcityid = response.data.cities.citydata[0].cityId
				
			}
			else{
				e.cityItems.unshift({cityName:"Alle Städte", cityId:'000'});
			}
          }
        })
    },
    searchmunicipalitynames: function () {
      var e = this
	  var strQuery = ''
	  // if(valuesof){
		   // strQuery += '?value=' + valuesof
	  // }
	  if (e.sland.id) {
        strQuery += '?countryid=' + e.sland.id
      }
	  if (e.stateids) {
        strQuery += '&stateids=' + e.stateids
      } 
	  if (e.selectedcityid) {
        strQuery += '&cityid=' + e.selectedcityid
      } 
      axios
        .get('/adminglobal/getmunicipalitybysearch/'+strQuery)
        .then(function (response) {
          if (response.data.status == true) {
            e.MunicipalityItems = response.data.municipalities.municipalities;
			// if(e.selectedcityid =='000'){
			// e.MunicipalityItems.unshift({municipality_name:"Alle Gemeinden", municipality_id:'000'});
			// }
			if(e.MunicipalityItems.length==1 && e.sland.name!="Schweiz"){
				 $('#Municipality').css('color', 'black');
				$('#plz').css('color', 'black');
				e.selectedmunicipalityid = response.data.municipalities.municipalities[0].municipality_id
				 e.IsCityselectedForMunicipalty=true;
			}
			else{
				e.MunicipalityItems.unshift({municipality_name:"Alle Ortsteile", municipality_id:'000'});
			}
			if(e.MunicipalityItems.length==''){
			 e.IsCityselectedForMunicipalty = false;
			}
          }
        })
    },	
    SearchPlz: function () {
      var e = this
	  var strQuery = ''
	  // if(valuesof){
		   // strQuery += '?value=' + valuesof
	  // }
	  if (e.sland.id) {
        strQuery += '?countryid=' + e.sland.id
      }
	  if (e.stateids) {
        strQuery += '&stateids=' + e.stateids
      } 
	  if (e.selectedcityid) {
        strQuery += '&cityid=' + e.selectedcityid
      } 
	  if (e.selectedmunicipalityid) {
        strQuery += '&municipalityid=' + e.selectedmunicipalityid
      } 	  
	  // if (e.citywithmuncipalityid) {
        // strQuery += '&citywithmuncipalityid=' + e.citywithmuncipalityid
      // }	
      axios
        .get('/adminglobal/getpostcodes/'+strQuery)
        .then(function (response) {
          if (response.data.status == true) {
            e.postcodes = response.data.postcode.postcodes;
			if(e.postcodes.length==1){
				$('#plz').css('color', 'black');
				e.plzorpostcode = response.data.postcode.postcodes[0].title;
			} else {
				e.postcodes.unshift({title:"Alle PLZ", id:'000'})
			}
          }
        })
    },  
	listOfChurchetype: function (countryid) {
      var e = this
      axios
        .get('/adminglobal/listOfChurchetype/?countryid=' + countryid)
        .then(function (response) {
          if (response.data.status == true) {
            e.GemeindetypeList = response.data.churchetypes.churchetype;
          }
        })
    },
	listofsubChurchetype: function (catId){
	  var e = this
      axios
        .get('/adminglobal/listOfSubChurchetype/?catid='+catId)
        .then(function (response) {
          if (response.data.status == true) {
            e.GemeindetypesubList = response.data.churchetypes.churchetype;
          }
        })
	},
    listofcategory: function () {
      var e = this
      axios
        .get('/churcheview/newsroomcategory')
        .then(function (response) {
          if (response.data.status == true) {
            e.category = response.data.catgory
          }
        })
    },
    listofsubcategory: function (cat = 1) {
      var e = this
      axios
        .get('/churcheview/newsroomsubcategory/?c=' + cat)
        .then(function (response) {
          if (response.data.status == true) {
            e.subcategory = response.data.subcatgory
          }
        })
    },
	listofthemens: function () {
      var e = this
      axios
        .get('/churcheview/listOfthemen')
        .then(function (response) {
          if (response.data.status == true) {
			  // console.log(response.data.themen)
            e.themenItems = response.data.themen
          }
        })
    },	
	countryonchangefunctions: function(countryobj){
		console.log(countryobj);
		setTimeout(function(){
		$('#Bundesland').removeAttr('placeholder');
			}, 1);
			setTimeout(function(){
			$('#Stadt').removeAttr('placeholder');
				}, 1);
			setTimeout(function(){
			$('#plz').removeAttr('placeholder');
				}, 1);
		var e = this
		$('#land').css('color', 'black');
		if(countryobj.id == 1){
			this.st = "Bundesland";
		}else if(countryobj.id == 2){
			this.st = "Bundesländer";
		}else if(countryobj.id == 3){
			this.st = "Kantone";
		}
		if(countryobj.id){
			
		if(countryobj.name=="Schweiz")
		{
			e.IsCityselectedForMunicipalty=false;
			
		} 
			e.isCountrynotselected = true;
			e.StateItems = [];
			e.cityItems = [];
			e.MunicipalityItems = [];			
			e.postcodes = [];
			e.stateids = '000';			
			e.selectedcityid = '000';			
			e.selectedmunicipalityid = '';			
			e.plzorpostcode = '000';			
			this.listofStates(countryobj.id);
			this.listOfChurchetype(countryobj.id);
			this.SearchPlz();			
			this.searchcitynames();			
			this.searchmunicipalitynames();			
		}
		else {
			e.isCountrynotselected = false;
		}
	},
	statechangefunctions: function(event){
		var e = this
		$('#Bundesland').css('color', 'black');
		if(event){
			e.cityItems = [];
			e.MunicipalityItems = [];			
			e.postcodes = [];		
			e.selectedcityid = '000';			
			e.selectedmunicipalityid = '';			
			e.plzorpostcode = '000';			
			this.SearchPlz();
			this.searchcitynames();
			this.searchmunicipalitynames();
		}
	},
	citychangefunctions: function(event){
		var e = this
		$('#Stadt').css('color', 'black');
		if(event){
			if(e.sland.name=='Schweiz'){
			e.IsCityselectedForMunicipalty = false;
			} else {
				e.IsCityselectedForMunicipalty = true;
			}
		//	e.IsCityselectedForMunicipalty = true;
		e.selectedmunicipalityid='000';
			e.MunicipalityItems = [];			
			e.postcodes = [];		
			//e.selectedmunicipalityid = '';			
			e.plzorpostcode = '000';
			this.getStateofCity(event);			
			this.SearchPlz();
			this.searchmunicipalitynames();
			// this.searchcitynames();
		}
		else {
			e.IsCityselectedForMunicipalty = false;
		}
	},
	municipalitychangefunctions: function(event){
		var e = this
		$('#Municipality').css('color', 'black');
		if(event){	
		this.SearchPlz();
			e.postcodes = [];			
			e.plzorpostcode = '';
			this.getCityStateOfMunicipality(event)
		}
	},
	postcodechangefunctions: function(event){
		var e = this
			$('#plz').css('color', 'black');
		if(event){	
			this.getCityStateMunicipalityOfPostcode(event)
		}
	},	
	getStateofCity: function (cityid) {
      var e = this
      axios
        .get('/adminglobal/getStateOfCity/?cityid=' + cityid)
        .then(function (response) {
          if (response.data.status == true) {
			  
			  			 $('#Bundesland').css('color', 'black');
			  e.stateids = response.data.state.state[0].StateID;
			  e.searchcitynames();
          }
        })
    },		
	getCityStateOfMunicipality: function (munciId) {
      var e = this
      axios
        .get('/adminglobal/getCityStateOfMunicipality/?munciId=' + munciId)
        .then(function (response) {
          if (response.data.status == true) {
			  // console.log(response.data.state.state[0])
			  $('#Bundesland').css('color', 'black');
			$('#Stadt').css('color', 'black');
			  e.stateids = response.data.state.state[0].Stateid;
				e.selectedcityid = response.data.state.state[0].city_id;
			  // e.municipality_id = response.data.state.state[0].munciID;
			  // e.citysearchhere = response.data.state.state[0].city_name;
          }
        })
    },		
	getCityStateMunicipalityOfPostcode: function (postcode) {
      var e = this
      axios
        .get('/adminglobal/getCityStateMunicipalityOfPostcode/?postcode=' + postcode)
        .then(function (response) {
          if (response.data.status == true) {
			  if(response.data.state.state.length == 1){
				  $('#Bundesland').css('color', 'black');
			$('#Stadt').css('color', 'black');
			$('#Municipality').css('color', 'black');
			  e.stateids = response.data.state.state[0].Stateid;
			  e.selectedcityid = response.data.state.state[0].city_id;
			  e.selectedmunicipalityid = response.data.state.state[0].munciID;
			  // e.citysearchhere = response.data.state.state[0].city_name;
			  // e.municipalitySearch = response.data.state.state[0].municipality_name;
			  } else {
				  // e.stateids = '';
				  // e.selectedcityid = '';
				  // e.selectedmunicipalityid = '';
			  }
          }
        })
    },
    // Searchcitywithmuncipality: function (valuesof) {
      // var e = this
      // axios
        // .get('/adminglobal/getcitywithmuncipality/?value='+valuesof)
        // .then(function (response) {
          // if (response.data.status == true) {
            // e.citywithmuncipality = response.data.dataofcities.citywithmuncipality
          // }
        // })
    // },
     // listofcity: function (stateid) {
      // var e = this
      // axios
        // .get('/adminglobal/getallcity/?id=' + stateid)
        // .then(function (response) {
          // if (response.data.status == true) {
            // e.cityItems = response.data.cities.city
            // // e.dialogLoader = false;
          // }
        // })
    // },
    // listofmuncipalty: function (id) {
      // var e = this
      // axios
        // .get('/adminglobal/getallmuncipalitytype?cityid=' + id)
        // .then(function (response) {
          // e.dialogLoader = false
          // if (response.data.status == true) {
           // e.MunicipalityItems = response.data.allmuncipalitytype
            // // console.log(e.MunicipalityItems);
          // }
        // })
    // },
	// listofmuncipaltyforpostcode:function (id) {
      // var e = this
      // axios
        // .get('/adminglobal/getallmuncipaltyforpostcode?postcodeid=' + id)
        // .then(function (response) {
          // e.dialogLoader = false
          // if (response.data.status == true) {
           // e.postcodes = response.data.allmuncipaltyforpostcodetype
            // // console.log(e.MunicipalityItems);
          // }
        // })
    // },
	// listofpostcodeformuncipality:function (id) {
      // var e = this
      // axios
        // .get('/adminglobal/getallpostcodeformuncipalty?muncipaltyid=' + id)
        // .then(function (response) {
          // e.dialogLoader = false
          // if (response.data.status == true) {
           // e.citywithmuncipality = response.data.allpostcodefortypemuncipalty.citywithmuncipality
           // // console.log(e.citywithmuncipality);
          // }
        // })
    // },
	clickreset: function (){
			setTimeout(function(){
			$("#land").val("");
			}, 1);
	},
	clickreset1: function (){
			setTimeout(function(){
			$("#Bundesland").val("");
			}, 1);
	},clickreset2: function (){
			setTimeout(function(){
			$("#Bundesland").val("");
			}, 1);
	},clickreset3: function (){
			setTimeout(function(){
			$("#Bundesland").val("");
			}, 1);
	},clickreset4: function (){
			setTimeout(function(){
			$("#Stadt").val("");
			}, 1);
	},clickreset5: function (){
			setTimeout(function(){
			$("#Municipality").val("");
			}, 1);
	},clickreset6: function (){
			setTimeout(function(){
			$("#plz").val("");
			}, 1);
	},clickreset7: function (){
			setTimeout(function(){
			$("#Ressort").val("");
			}, 1);
	},clickreset8: function (){
			setTimeout(function(){
			$("#Subressort").val("");
			}, 1);
	},

    listofpublisher: function () {
      var e = this
      axios
        .get('/churcheview/newsroompublisher')
        .then(function (response) {
          if (response.data.status == true) {
            e.publisher = response.data.publisher
          }
        })
    },
  },
  components: {
    KirchenFinder
    // 'datetime': datetime
  }
}


</script>
<style scoped>
.progess-custom {
  margin: 0px;
}
/* Load more */
.load-more{
      width: 99%;
  
    background: #0B324F;
    text-align: center;
    color: white;
    padding: 10px 0px;
    font-family: sans-serif;
}

.load-more:hover{
  cursor: pointer;
}

/* Finish */
.finish{
  width: 99%;

  background: darkgray;
  text-align: center;
  color: white;
  padding: 10px 0px;
  font-family: sans-serif;
}

/* more link */
.more{
  color: blue;
  text-decoration: none;
  letter-spacing: 1px;
  font-size: 16px;
}

[v-cloak] {
  display:none;
}
#Municipality{
	color:lightgrey;	
}
#land{
	color:lightgrey;	
}
#Stadt{
	color:lightgrey;
}
#Bundesland{
	color:lightgrey;
}
#plz{
	color:lightgrey;
}

.content-head h4, .content-odd h4 {
font-size: 16px;
}
.teaser-body span {
font-size: 12px;
text-transform: uppercase;
}
.teaser-link-info {
color: #0d2040;
font-weight: bold;
padding: 0 9px 0 0;
}
.teaser-body span.teaser-link-info, a.as_teaser-ressort {
border-right: 1px solid #f18825;
margin-right: 5px;
display: inline-block;
height: 11px;
}
a.caption.black--text.font-weight-black {
    font-size: 16px !important;
}
a.caption.black--text {
    font-size: 10px !important;
    font-family: "LinotypeUnivers-430Regular";
}
.image-copyright {
    position: absolute;
	
    color: #fff;
    font-size: 9px;
    left: 25px;
    cursor: pointer;
	margin-top:-150px;
}
.image-copyright .image-copyright-view {
    display: inline-block;
	
	 margin-left: -9px;
    margin-top: 10px;
}
.image-copyright:hover .image-copyright-hover{
	display: inline-block;
}
.image-copyright:hover .image-copyright-view{
	display:none !important;
}
.image-copyright-view {
    padding-left: 7px;
}
.image-copyright-hover p.pull-right {
    float: left !important;
    margin-left: 0px;
	
}
.image-copyright-hover p {
    margin-bottom: 0;
    line-height: 1.413 !important;
    font-size: 9px !important;
    margin-left: 3px;
}

.image-copyright-hover {
    background-color: rgba(128, 128, 128, 0.78);
    padding: 6px 3px 6px 7px;
margin-left: -13.5px;
    margin-top: 5px;
	    display: none;
	
}
.image-copyright-hover a {
    color: #337ab7 !important;
    line-height: 1.413 !important;
}

.image-copyright-hover p.pull-right {
    float: left !important;
    margin-left: 0px;
}

</style>

