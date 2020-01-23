<template>
<v-layout
      text-center
      wrap
    >
	<v-flex md12 lg12 xll2 xs4 sm6>
  <v-content class="mainWrapper white">
    <section class="kitchenFinderSectionTop">
      <v-btn fab dark small color="primary" @click.stop="drawer = !drawer" class="btnFilter">
        <v-icon dark>filter_list</v-icon>
      </v-btn>	  
	  <!-- MOBILE SIDEBAR STARTS -->	  
      <v-navigation-drawer v-model="drawer" fixed temporary>
        <v-card flat color="white" class="mobileFilterWarp">
				<v-card flat color="white" class="kitchenFinderFilter">
              <v-card-text>
                <v-form>
				<v-autocomplete
					v-if="drawer"
					:loading="searchLoading"
					:items="churcheTitle"
					item-text="title"
					item-value="link"
					:search-input.sync="search"
					@input="pageRedirect"
					cache-items					
					hide-no-data
					hide-details
					placeholder="Suche"
					append-icon="search"					
					style="margin-bottom: 16px;"
				></v-autocomplete>			  
				<v-autocomplete
					v-if="drawer"
					id="orgtype"
                    label="Organisationstyp"
                    placeholder="Organisationstyp"
                    :items="organizationTypes"
                    item-text="name"
                    @input="mapCenterloation"
                    v-model="orgtype"
                    item-value="name"
					@change="countrylistopen"	
					v-on:click.native="clickresetorg"
			  ></v-autocomplete>				
				<v-autocomplete
					v-if="drawer"
						id="land"
                    label="Land"
                    placeholder="Alle Länder"
                    :items="LandList"
                    item-text="name"
                    @input="mapCenterloation"
                    v-model="sland"
                    item-value="id"
					return-object
					@change="countryonchangefunctions"
					v-on:click.native="clickreset"
				></v-autocomplete>				
				<v-autocomplete
					v-if="drawer && isCountrynotselected && st == 'Bundesland'"
					id="Bundesland"
                    label="Bundesland"
                    placeholder="bundesland auswählen"
                    :items="StateItems"
                    item-text="StateName"
                    @input="mapCenterloation"
                    item-value="ID"
					v-model="stateids"
					return-object
					@change="statechangefunctions"		
					v-on:click.native="clickreset1"
				></v-autocomplete>
				<v-autocomplete
					v-if="drawer && isCountrynotselected && st == 'Bundesländer'"
					id="Bundesland"
                    label="Bundesländer"
                    placeholder="bundesland auswählen"
                    :items="StateItems"
                    item-text="StateName"
                    @input="mapCenterloation"
                    item-value="ID"
					v-model="stateids"
					return-object
					@change="statechangefunctions"		
					v-on:click.native="clickreset2"
				></v-autocomplete>
				<v-autocomplete
					v-if="drawer && isCountrynotselected && st == 'Kantone'"
					id="Bundesland"
                    label="Kanton"
                    placeholder="bundesland auswählen"
                    :items="StateItems"
                    item-text="StateName"
                    @input="mapCenterloation"
                    item-value="ID"
					v-model="stateids"
					return-object
					@change="statechangefunctions"		
					v-on:click.native="clickreset3"
				></v-autocomplete>	
				<v-autocomplete
					v-if="drawer && isCountrynotselected"
					label="Stadt"
					id="Stadt"
                    :loading="searchLoading"
                    :items="cityItems"
                    item-text="cityName"
                    item-value="cityId"
                    @input="mapCenterloation"
				    hide-no-data
				    hide-details
                    placeholder="Stadt"
                    style="margin-bottom: 16px;"
					v-model="selectedcityid"
					return-object
					@change="citychangefunctions"	
					v-on:click.native="clickreset4"
				></v-autocomplete>
				<v-autocomplete	
					v-if="drawer && isCountrynotselected && IsCityselectedForMunicipalty"
					id="Municipality"
                    :loading="searchLoading"
                    :items="MunicipalityItems"
                    item-text="municipality_name"
                    item-value="municipality_id"
                    @input="mapCenterloation"
				    hide-no-data
				    hide-details
                    placeholder=""
                    style="margin-bottom: 16px; padding-top: 0px; margin-top: -10px;"
					v-model="selectedmunicipalityid"
					return-object
					@change="municipalitychangefunctions"
					v-on:click.native="clickreset5"
				></v-autocomplete>		
				<!--<v-autocomplete	
					v-if="!drawer"
					label="Stadt/Ort"
                    :loading="searchLoading"
                    :items="citywithmuncipality"
                    item-text="location"
                    item-value="locationvalue"
                    :search-input.sync="citysearchhere"
                    @input="mapCenterloation"
				    hide-no-data
				    hide-details
                    placeholder="Stadt/Ort"
                    style="margin-bottom: 16px;"
					v-model="citywithmuncipalityid"
					clearable
					:disabled="isCountrynotselected"
				></v-autocomplete>				  
				<v-autocomplete
					v-if="!drawer"
					label="PLZ"
                    :loading="searchLoading"
                    :items="postcodes"
                    item-text="title"
                    item-value="id"
                    :search-input.sync="postcodesearch"
                    @input="mapCenterloation" 
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
					label="Postleitzahl  "
                    :loading="searchLoading"
                    :items="postcodes"
                    item-text="title"
                    item-value="id"
                    @input="mapCenterloation" 
				    hide-no-data
				    hide-details
                    placeholder="Alle PLZ"
					v-model="plzorpostcode"
					return-object
                    style="margin-bottom: 16px;"
					@change="postcodechangefunctions"
					v-on:click.native="clickreset6"
				></v-autocomplete>				
				<v-label v-if="isCountrynotselected">
                    <span class="custom">Umfeld-Radius</span>
				</v-label>
				<v-slider v-if="isCountrynotselected" v-model="radius" class="mt-0" :max="2000" :min="0" @change="mapCenterloation" thumb-label></v-slider>                 
				<v-autocomplete
					v-if="drawer && isorganizationTypeChurche && isCountrynotselected"
					id="Konfession"
                    :items="GemeindetypeList"
                    item-text="CategoryName"
                    item-value="ID"
                    label="Konfession"
                    placeholder="Bitte auswählen"
                    @input="mapCenterloation"
                    v-model="gemeindetyp"
					@change="listofsubChurchetype"
					v-on:click.native="clickreset7"
				></v-autocomplete>	
				<v-autocomplete
					v-if="drawer && isChurchetypeSelected && isorganizationTypeChurche && isCountrynotselected"
					id="SubcatName"
                    :items="GemeindetypesubList"
                    item-text="SubcatName"
                    item-value="ID"
                    placeholder="Bitte auswählen"
                    @input="mapCenterloation"
                    v-model="gemeindesubtyp"
					@change="listofSubSubchurcheType"
					style="padding-top: 0px; margin-top: -10px;"
					v-on:click.native="clickreset8"
				></v-autocomplete>			
				<v-autocomplete
					v-if="drawer && isChurcheSubtypeSelected && isorganizationTypeChurche && isCountrynotselected"
					id="subsubCatName"
                    :items="GemeindetypesubsubList"
                    item-text="subsubCatName"
                    item-value="ID"
                    placeholder="Bitte auswählen"
                    @input="mapCenterloation"
                    v-model="GemeindesubSubtyp"
					style="padding-top: 0px; margin-top: -10px;"
					v-on:click.native="clickreset9"
				></v-autocomplete>			
				<v-autocomplete
					v-if="drawer && isorganizationTypeChurche && isCountofgemeindegrobe"
					id="visitor"
                    :items="communitySize"
                    label="Gemeindegröße"
                    placeholder="Bitte auswählen"
                    v-model="gemeindegrobe"
					item-text="visitor_range"	
					item-value="ID"
                    @input="mapCenterloation"
					@change="gemendegrobecolorchange"
					v-on:click.native="clickreset10"
				></v-autocomplete>					
				<v-autocomplete
					v-if="drawer && isorganizationTypeChurche"
					id="Spraches"
                    :items="mylanguage"
                    label="Sprache"
                    placeholder="Alle Sprachen"
                    v-model="sprache"
                    @input="mapCenterloation"
					@change="getlanguagemultiple"
					item-text="languagename"
                    item-value="id"
					multiple
					></v-autocomplete>
                </v-form>
                <div class="dealsWrap" v-if="isorganizationTypeChurche">
                  <h4 class="body-1">Angebote</h4>
                  <v-tooltip v-if="offers" v-for="(offer, index) in offers" bottom>
                    <v-img
                      slot="activator"
                      :src="'http://dev.woobii.com/admin/'+offer.dealImg"
                      class="imgDeal"
                    >
                      <label>
                        <input
                          type="checkbox"
                          name="offer[]"
                          @input="mapCenterloation"
                          :value="offer.dealId"
                        >
                        <span class="backgroundColor"></span>
                      </label>
                    </v-img>
                    <span>{{offer.deal}}</span>
                  </v-tooltip>
                </div>
              </v-card-text>
            </v-card>
        </v-card>
      </v-navigation-drawer>    
	  <!-- MOBILE SIDEBAR ENDS -->
    </section>
    <section class="kitchenFinderSection">
      <v-container fluid grid-list-xl>
        <v-layout row wrap justify-center>
          <v-flex xs12 md3>
		  <!-- FULL SIDEBAR STARTS -->
			<v-card flat color="white" class="kitchenFinderFilter">
              <v-card-text>
                <v-form>
				<v-autocomplete
					v-if="!drawer"
					:loading="searchLoading"
					:items="churcheTitle"
					item-text="title"
					item-value="link"
					:search-input.sync="search"
					@input="pageRedirect"
					cache-items					
					hide-no-data
					hide-details
					placeholder="Suche"
					append-icon="search"					
					style="margin-bottom: 16px;"
				></v-autocomplete>			  
				<v-autocomplete
					v-if="!drawer"
					id="orgtype"
                    label="Organisationstyp"
                    placeholder="Organisationstyp"
                    :items="organizationTypes"
                    item-text="name"
                    @input="mapCenterloation"
                    v-model="orgtype"
                    item-value="name"
					@change="countrylistopen"	
					v-on:click.native="clickresetorg"
			  ></v-autocomplete>				
				<v-autocomplete
					v-if="!drawer"
						id="land"
                    label="Land"
                    placeholder="Alle Länder"
                    :items="LandList"
                    item-text="name"
                    @input="mapCenterloation"
                    v-model="sland"
                    item-value="id"
					return-object
					@change="countryonchangefunctions"
					v-on:click.native="clickreset"
				></v-autocomplete>				
				<v-autocomplete
					v-if="!drawer && isCountrynotselected && st == 'Bundesland'"
					id="Bundesland"
                    label="Bundesland"
                    placeholder="bundesland auswählen"
                    :items="StateItems"
                    item-text="StateName"
                    @input="mapCenterloation"
                    item-value="ID"
					v-model="stateids"
					return-object
					@change="statechangefunctions"		
					v-on:click.native="clickreset1"
				></v-autocomplete>
				<v-autocomplete
					v-if="!drawer && isCountrynotselected && st == 'Bundesländer'"
					id="Bundesland"
                    label="Bundesländer"
                    placeholder="bundesland auswählen"
                    :items="StateItems"
                    item-text="StateName"
                    @input="mapCenterloation"
                    item-value="ID"
					v-model="stateids"
					return-object
					@change="statechangefunctions"		
					v-on:click.native="clickreset2"
				></v-autocomplete>
				<v-autocomplete
					v-if="!drawer && isCountrynotselected && st == 'Kantone'"
					id="Bundesland"
                    label="Kanton"
                    placeholder="bundesland auswählen"
                    :items="StateItems"
                    item-text="StateName"
                    @input="mapCenterloation"
                    item-value="ID"
					v-model="stateids"
					return-object
					@change="statechangefunctions"		
					v-on:click.native="clickreset3"
				></v-autocomplete>	
				<v-autocomplete
					v-if="!drawer && isCountrynotselected"
					label="Stadt"
					id="Stadt"
                    :loading="searchLoading"
                    :items="cityItems"
                    item-text="cityName"
                    item-value="cityId"
                    @input="mapCenterloation"
				    hide-no-data
				    hide-details
                    placeholder="Stadt"
                    style="margin-bottom: 16px;"
					v-model="selectedcityid"
					return-object
					@change="citychangefunctions"	
					v-on:click.native="clickreset4"
				></v-autocomplete>
				<v-autocomplete	
					v-if="!drawer && isCountrynotselected && IsCityselectedForMunicipalty"
					id="Municipality"
                    :loading="searchLoading"
                    :items="MunicipalityItems"
                    item-text="municipality_name"
                    item-value="municipality_id"
                    @input="mapCenterloation"
				    hide-no-data
				    hide-details
                    placeholder=""
                    style="margin-bottom: 16px; padding-top: 0px; margin-top: -10px;"
					v-model="selectedmunicipalityid"
					return-object
					@change="municipalitychangefunctions"
					v-on:click.native="clickreset5"
				></v-autocomplete>		
				<!--<v-autocomplete	
					v-if="!drawer"
					label="Stadt/Ort"
                    :loading="searchLoading"
                    :items="citywithmuncipality"
                    item-text="location"
                    item-value="locationvalue"
                    :search-input.sync="citysearchhere"
                    @input="mapCenterloation"
				    hide-no-data
				    hide-details
                    placeholder="Stadt/Ort"
                    style="margin-bottom: 16px;"
					v-model="citywithmuncipalityid"
					clearable
					:disabled="isCountrynotselected"
				></v-autocomplete>				  
				<v-autocomplete
					v-if="!drawer"
					label="PLZ"
                    :loading="searchLoading"
                    :items="postcodes"
                    item-text="title"
                    item-value="id"
                    :search-input.sync="postcodesearch"
                    @input="mapCenterloation" 
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
					label="Postleitzahl  "
                    :loading="searchLoading"
                    :items="postcodes"
                    item-text="title"
                    item-value="id"
                    @input="mapCenterloation" 
				    hide-no-data
				    hide-details
                    placeholder="Alle PLZ"
					v-model="plzorpostcode"
					return-object
                    style="margin-bottom: 16px;"
					@change="postcodechangefunctions"
					v-on:click.native="clickreset6"
				></v-autocomplete>				
				<v-label v-if="isCountrynotselected">
                    <span class="custom">Umfeld-Radius</span>
				</v-label>
				<v-slider v-if="isCountrynotselected" v-model="radius" class="mt-0" :max="5000" :min="0" @change="mapCenterloation" thumb-label></v-slider>                 
				<v-autocomplete
					v-if="!drawer && isorganizationTypeChurche && isCountrynotselected"
					id="Konfession"
                    :items="GemeindetypeList"
                    item-text="CategoryName"
                    item-value="ID"
                    label="Konfession"
                    placeholder="Bitte auswählen"
                    @input="mapCenterloation"
                    v-model="gemeindetyp"
					@change="listofsubChurchetype"
					v-on:click.native="clickreset7"
				></v-autocomplete>	
				<v-autocomplete
					v-if="!drawer && isChurchetypeSelected && isorganizationTypeChurche && isCountrynotselected"
					id="SubcatName"
                    :items="GemeindetypesubList"
                    item-text="SubcatName"
                    item-value="ID"
                    placeholder="Bitte auswählen"
                    @input="mapCenterloation"
                    v-model="gemeindesubtyp"
					@change="listofSubSubchurcheType"
					style="padding-top: 0px; margin-top: -10px;"
					v-on:click.native="clickreset8"
				></v-autocomplete>			
				<v-autocomplete
					v-if="!drawer && isChurcheSubtypeSelected && isorganizationTypeChurche && isCountrynotselected"
					id="subsubCatName"
                    :items="GemeindetypesubsubList"
                    item-text="subsubCatName"
                    item-value="ID"
                    placeholder="Bitte auswählen"
                    @input="mapCenterloation"
                    v-model="GemeindesubSubtyp"
					style="padding-top: 0px; margin-top: -10px;"
					v-on:click.native="clickreset9"
				></v-autocomplete>			
				<v-autocomplete
					v-if="!drawer && isorganizationTypeChurche && isCountofgemeindegrobe"
					id="visitor"
                    :items="communitySize"
                    label="Gemeindegröße"
                    placeholder="Bitte auswählen"
                    v-model="gemeindegrobe"
					item-text="visitor_range"	
					item-value="ID"
                    @input="mapCenterloation"
					@change="gemendegrobecolorchange"
					v-on:click.native="clickreset10"
				></v-autocomplete>					
				<v-autocomplete
					v-if="!drawer && isorganizationTypeChurche"
					id="Spraches"
                    :items="mylanguage"
                    label="Sprache"
                    placeholder="Alle Sprachen"
                    v-model="sprache"
                    @input="mapCenterloation"
					@change="getlanguagemultiple"
					item-text="languagename"
                    item-value="id"
					multiple
					></v-autocomplete>
				
                </v-form>
                <div class="dealsWrap" v-if="isorganizationTypeChurche">
                  <h4 class="body-1">Angebote</h4>
                  <v-tooltip v-if="offers" v-for="(offer, index) in offers" bottom>
                    <v-img
                      slot="activator"
                      :src="'http://dev.woobii.com/admin/'+offer.dealImg"
                      class="imgDeal"
                    >
                      <label>
                        <input
                          type="checkbox"
                          name="offer[]"
                          @input="mapCenterloation"
                          :value="offer.dealId"
                        >
                        <span class="backgroundColor"></span>
                      </label>
                    </v-img>
                    <span>{{offer.deal}}</span>
                  </v-tooltip>
                </div>
              </v-card-text>
            </v-card>
			
			<!-- FULL SIDEBAR ENDS -->
          </v-flex>
          <v-flex xs12 md9>
            <v-layout row wrap>
              <v-flex xs12>
                <v-card flat color="white" class="kFinderMapWrap">
                  <v-card-text class="pa-3">
                   <v-layout row wrap>
                      <v-flex d-flex xs12 md4>
                        <!--<p class="caption mb-0">
						{{txt}} 
                        </p> -->
						
						<p class="caption mb-0" id="alignfirsttext">
						{{txtofnew}} 
                        </p>
                      <!--<span class="display-1 font-weight-bold my-0">{{newcountsof}}</span>-->
					  <span class="display-1 font-weight-bold my-0">{{communitycountaaaaa}}</span>
		<!--<span class="display-1 font-weight-bold my-0">
		{{countrycountofcomm == '' && countorgnizationcomm == '' ? totalcount : countrycountofcomm == '' && countorgnizationcomm != '' ? countorgnizationcomm  : countrycountofcomm != '' && countorgnizationcomm != '' ? countryorgcomm : countrycountofcomm.countrycountofcommunity}}  </span>-->
		
		<!--<span class="display-1 font-weight-bold my-0" v-if="countrycountofcomm =='' && countorgnizationcomm == ''">{{totalcount}}</span>
		<span class="display-1 font-weight-bold my-0" v-else-if="countrycountofcomm =='' && countorgnizationcomm != ''">{{countorgnizationcomm}}</span>
		<span class="display-1 font-weight-bold my-0" v-else-if="countrycountofcomm !='' && countorgnizationcomm != ''">{{countryorgcomm}}</span>
		<span class="display-1 font-weight-bold my-0" v-else>{{countrycountofcomm.countrycountofcommunity}}</span>-->
						
						
                      </v-flex>
                      <v-flex d-flex xs12 md4 v-if="secondtextnew != ''">
                        <!--<p class="caption mb-0">
                        {{secondtxt}}
                        </p>-->
						<p class="caption mb-0">
                        {{secondtextnew}}
                        </p>
                     <span class="display-1 font-weight-bold my-0">
					 {{totalcount}}</span>
                      </v-flex>  
					  <v-flex d-flex xs12 md4 v-if="hidethird">
                        <p class="caption mb-0">
                          Top gemeinden in 
						  <br>Ihrer vorauswahl
                        </p>
                        <span class="display-1 font-weight-bold my-0">{{radiusCircle}} %</span>
                      </v-flex>                     
                    </v-layout>
                  </v-card-text>
                  <GmapMap
                    :center="center"
					:zoom="zoom"
                    map-type-id="roadmap"
                    style="width: 100%; height: 450px"
                  >
                    <GmapCluster>
                      <GmapMarker
                        :key="index"
                        v-for="(m, index) in markers"
                        :position="m"
                        :clickable="true"
                        :draggable="false"
						@click="openInfoWindowTemplate(m)"
                      />
					  <gmap-info-window
						:options="{maxWidth: 300}"
						:position="infoWindow.position"
						:opened="infoWindow.open"
						@closeclick="infoWindow.open=false">
						<div v-html="infoWindow.template">
						
						</div>
						<a :href="'/Kirchenfinder/'+infoWindow.myslug" class="map-link" data-title="East Howe Steps">View Details</a>
						
					</gmap-info-window>
                      <GmapCircle
					  v-if="radiusCircle"
                        :bounds="circleBounds"
                        :center="center"
                        :radius="radius"
                        :options="{editable: false}"
                      ></GmapCircle>
                    </GmapCluster>
                  </GmapMap>
                  <!--<googlemaps-map
                    :center.sync="center"
                    :zoom.sync="zoom"
                    map-type-id="roadmap"
                         style="width: 100%; height: 450px"
                  >:options="mapOptions"
                    @idle="onIdle"
                    @click="onMapClick"
                    :label="{
                        color: marker === currentmarker ? 'white' : 'black',
                        fontFamily: 'Material Icons',
                        fontSize: '20px',
                        text: 'star_rate',
                      }"
                  <!-- User Position-->
                  <!--<googlemaps-user-position @update:position="setUserPosition"/>
                    <googlemaps-marker
                      v-for="(marker, index) of markers"
                      :key="index"
                      :position="marker"
                    >
                    </googlemaps-marker>
                    <!-- <circle :bounds.sync="circleBounds" :center.sync="center" :radius.sync="100000" :options="{editable: true}"></circle>
                  </googlemaps-map>-->
                  <!-- <v-img src="https://cdn.vuetifyjs.com/images/cards/desert.jpg"></v-img> -->
                </v-card>
              </v-flex>
            </v-layout>
            <v-layout row wrap v-if="churchesList">
              <v-flex d-flex xs12 md4 v-for="churche in churchesList">
                <v-card flat color="white" class="kFinderWrap">
                  <router-link :to="'/Kirchenfinder/'+churche.slug" class="caption black--text">
                    <v-img
                      v-if="churche.bannerimage"
                      :lazy-src="'http://dev.woobii.com/admin/'+churche.bannerimage"
                      :src="'http://dev.woobii.com/admin/'+churche.bannerimage"
                      class="kitchenImage"
                    />
                    <v-img
                      v-else
                      :src="require(`@/assets/woobii-banner.jpg`)"
                      :lazy-src="require(`@/assets/woobii-banner.jpg`)"
                    />
                  </router-link>
                  <v-card-text class="pa-0">
                    <router-link :to="'/Kirchenfinder/'+churche.slug" class="caption black--text">
                      <h4 class="body-1 my-2">{{ churche.title }}</h4>
                    </router-link>
                    <!--<div>
                      <v-chip
                        small
                        label
                        dark
                        class="white--text my-1"
                       v-for="tag in splitTag(churche.tags)"
                      >{{tag}}</v-chip>
                    </div>-->
                    <div class="my-2">
                      <v-rating
                        v-model="churche.rating"
                        readonly
                        dense
                        small
                        background-color="grey darken-4"
                        color="grey darken-4"
                    ></v-rating>
                     <!--<v-icon small class="black--text">star</v-icon>
                      <v-icon small class="black--text">star</v-icon>
                      <v-icon small class="black--text">star</v-icon>
                      <v-icon small class="black--text">star_half</v-icon>
                    <v-icon small class="black--text">star_border</v-icon>-->
                   <span class="body-1 font-weight-bold ml-2">{{churche.rating}}</span>
                    </div>
                    <a :href="'/Kirchenfinder/'+churche.slug" class="caption black--text">
                      <v-icon small class="black--text">chevron_right</v-icon>Zur Gemeinde
                    </a>
                  </v-card-text>
                </v-card>
              </v-flex>
            </v-layout>
			<!--<div class="text-xs-center">
              <v-pagination v-if="totalcount>9"
                v-model="page"
                :length="Math.floor(totalcount/9)"
                :total-visible="Math.floor(totalcount/9)"
                @input="onPageChange"
              ></v-pagination>
            </div>	-->	
          </v-flex>
        </v-layout>
      </v-container>
	   <infinite-loading @infinite="churchlist"><div slot="no-more"></div>
  <div class="show" slot="no-results"></div></infinite-loading>
    </section>
  
  </v-content>
  </v-flex>
    </v-layout>
</template>

<script>

import axios from 'axios'
import $ from 'jquery'
export default {
  name: 'App',
  data () {
    return {
		infoWindow: {
          position: {lat: 0, lng: 0},
          open: false,
          template: '',
		  myslug:''
        },
      rating: 3,
	  txtofnew:'',
	  secondtextnew:'',
	  countryorgcomm_new:'',
	  newcountsof:'',
      // baseUrl: process.env.BASE_URL
      isMobile: false,
      drawer: null,
      center: {
        lat: 51.1657,
        lng: 10.4515
        // lat: 52.52,
        // lng: 13.4
      },
	  pagginginfinte:0,
	  stid:'',
	  ctyid:'',
	  plz:'',
	  countryorgcomm:'',
	  comtotal:0,
	  hidesecond:false,
	  hidethird:false,
	  isCountrydropdown:true,
	  isCountrynotselected:false,
	  isorganizationTypeChurche:false,
	  isCountofgemeindegrobe:true,
	  isChurchetypeSelected:false,
	  isChurcheSubtypeSelected:false,
	  IsCityselectedForMunicipalty:false,
	  filtercountryID:'',
      radiusCircle: 0,
      page: 1, 
      totalcount: 0,
	  communitycountaaaaa:0,
      userPosition: null,
      zoom: 5,
      radiusflag: false,
      radius: 0,
      rateing: [1, 2, 3, 4, 5],
      selectCity: 0,
      land: 0,
      markers: [],
      offers: [],
      churchesList: [],
      circleBounds: {},
      GemeindetypeList: [],
      LandList: [],
      cityItems: [],
	  StateItems:[],
	  MunicipalityItems:[],
	  organizationTypes: ['Alle Organisationen','Hilfswerke', 'Initiativen', 'Kirchen', 'Missionswerke', 'Unternehmen'],
      OrtList: [],
      communitySize: [],
      topCommunity: ['Top Gemeinde', 'Best Gemeinde', 'Best Rating'],
      langauages: ['Französisch','Deutsch','Chinesisch'],
	  mylanguage:[],
      countryflag: false,
      offerData: [],
      angebot: '',
      bewertung: '',
      sprache:'',
      gemeindegrobe: 'Alle Gemeindegrößen',
      gemeindetyp: '000',
	  sland: '000',
	  orgtype:'Alle Organisationen',
	  city: '',
	  municipaltyType: '',
      topGemeinden: '',
      topTotalChurches: 0,
      searchLoading: false,
      churcheTitle: [],
      search: null,
	  postcodesearch: '',
      citysearchhere: '',
      municipalitySearch: '',
	  postcodes:[],
	  citywithmuncipality: [],
	  stateids:'000',
	  st:'',
	  citywithmuncipalityid:'',
	  plzorpostcode:'',
	  beers: [],	
	  GemeindetypesubList: '',
	  selectedcityid:'000', 	  
	  selectedmunicipalityid:'', 
	  gemeindesubtyp:'',
	  GemeindetypesubsubList:'',
	  GemeindesubSubtyp:'',
	  churchesubCategoryLabel:'',
	  txt:'',
	  secondtxt:'',
	  countryid:0,
	  countrycountofcomm:'',
	  countorgnizationcomm:''
	 
	  }
  },
  watch: {
	  bottom(bottom) {
      if (bottom) {
        this.addBeer()
      }
    },
    search (val) {
      if (val.length >= 3) {
        val && val !== this.select && this.querySelections(val)
      } else {
        this.churcheTitle = []
      }
    },
	postcodesearch (val) {
		//alert(val);
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
      // if (val == null) {
        // return
      // }
      // if (val.length >= 1) {
        // val && val !== this.select && this.Searchcitywithmuncipality(val)
      // } else {
        // this.citywithmuncipality = []
      // } 
	  
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
    }
  },

  mounted () {
    //this.churchlist();
    this.listofcountry();
    //this.listofcity();
    this.listofoffer();
	this.listoflanguage();
	this.listofsubChurchetype();
	this.listofgemeindegrobe();
	this.bydefaultgrey();
	this.bydefaulttext();
	this.textandcountmanagement('Alle Organisationen','000','000');
	
	// this.getgeolocation('China');
	// this.listOfChurchetype()
    // this.listofmuncipalty()
	// this.listofcity()
//	this.listofStates('000');

    // Initially load some items.
    
  },
  methods: {
openInfoWindowTemplate (item) {
console.log(item)
        this.infoWindow.position = item
        this.infoWindow.open = true
		this.infoWindow.template=item.title
		this.infoWindow.myslug=item.slug
		
   },	  
  clickresetorg: function (){
			setTimeout(function(){
			$("#orgtype").val("");
			}, 1);
	},
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
			$("#Konfession").val("");
			}, 1);
	},clickreset8: function (){
			setTimeout(function(){
			$("#SubcatName").val("");
			}, 1);
	},
	clickreset9: function (){
			setTimeout(function(){
			$("#subsubCatName").val("");
			}, 1);
	},
	clickreset10: function (){
			setTimeout(function(){
			$("#visitor").val("");
			}, 1);
	},
	clickreset11: function (){
		// this.sprache = "";
			setTimeout(function(){
			$("#Spraches").val("");
			}, 1);
	},

  bydefaultgrey:function(){	
			$('#orgtype').css('color', 'lightgrey');
			$('#land').css('color', 'lightgrey');
			$('#Bundesland').css('color', 'lightgrey');
			$('#Stadt').css('color', 'lightgrey');
			$('#Municipality').css('color', 'lightgrey');
			$('#plz').css('color', 'lightgrey');
			$('#Konfession').css('color', 'lightgrey');
			$('#Spraches').css('color', 'lightgrey');
			$('#land').removeAttr('placeholder');
			
  },
  bydefaulttext:function(){
	  var e = this
	  this.txt="Anzahl an Organisationen";
	
	 
  },
	  getlanguagemultiple:function(id){
		 // alert(id);
		  this.sprache=id;
		  this.mapCenterloation()
	  },
	
    pageRedirect: function (event) {
      this.$router.push('/Kirchenfinder/' + event)
    },
    querySelections (v) {
      var self = this
      this.searchLoading = true
      // Simulated ajax query
      axios
        .get('/churcheview/churchebyname?t=' + v)
        .then(function (response) {
          if (response.data.status == true) {
            self.churcheTitle = response.data.churche
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

    splitTag: function (value) {
      if (!value) return ''
      value = value.split(',')
      return value
    },
    centerOnUser () {
      if (this.userPosition) {
        this.center = this.userPosition
      }
    },
    setUserPosition (position) {
      this.userPosition = position
    },

	
getgeolocation: function (location) {
		//alert(location)
		var splitted = location.split(";", 2);
		// alert(splitted);
		if(splitted[0] == 'Alle Länder' && splitted[1] == 'coun'){
			splitted[0] = 'Deutschland';	
				splitted[1] = 'coun';
			}		
		if(splitted[1] == 'zip'){
			splitted[0] = parseInt(splitted[0],10);
			var e = this
      axios
        .get('https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyD-rdRygIIUU40ksaF8ZGZ1n8rfsMdM0xc&components=postal_code:'+splitted[0]+'')
        .then(function (response) {	
		if(response.data.results!=""){
			e.center = response.data.results[0].geometry.location;
			console.log(response.data.results[0].geometry.location);
			e.zoom=15;
		}
		})
		}else{
		// alert(splitted[1]);
		var e = this
      axios
        .get('https://maps.googleapis.com/maps/api/geocode/json?address='+splitted[0]+'&key=AIzaSyD-rdRygIIUU40ksaF8ZGZ1n8rfsMdM0xc')
        .then(function (response) {	
			
			console.log(response.data);
			if(response.data.results!="")
			{
				e.center = response.data.results[0].geometry.location;
				// e.getallgeolocation(response.data.results[0].geometry.location);
				if(splitted[1] == 'coun'){
				e.zoom=5;
				}else if(splitted[1] == 'state'){
				e.zoom=6;
				}else if(splitted[1] == 'city'){
				e.zoom=11;
				}else if(splitted[1] == 'muncipality'){
				e.zoom=13.5;
				}else if(splitted[1] == 'zip'){
				e.zoom=15;
				}
			}else{
				e.zoom=5;
			}
        })	
		}
},
	countrylistopen: function(event){
		//alert(event)
		
		
		var e = this
		//alert(e.sland.id);
		if(e.sland.id && e.sland.id != '000'){
			var countryid = e.sland.id;
		} else {
			var countryid = '000';
		}
		e.textandcountmanagement(event,countryid);
		if(event){
		$('#orgtype').css('color', 'black');
		
		e.isCountrydropdown = false;
		e.GemeindetypesubsubList = [];
		e.GemeindetypesubList = [];
		}  else {
			e.isCountrydropdown = true;
			e.isCountrynotselected = false;
		}
		if(event=='Kirchen'){
			var Hilfswerke='Kirchen';
			e.txt='Anzahl an Kirchen';
		   this.countoforganisationtype(Hilfswerke);			
			e.isorganizationTypeChurche = true;
			this.listofoffer();
			this.listofgemeindegrobe();
		} else {
			
			e.isorganizationTypeChurche = false;
		}
		
		
		
		
	},

    mapCenterloation: function (event) {
		//alert(event)
      var e = this
      var cityData = ''
	  e.pagginginfinte = 0;
      e.churchesList = [];
	  setTimeout(function(){
		   e.churchlist('',0, 0, 0, false)
	  },500)
	 
	  e.markerCount(this.center.lat, this.center.lng, e.markers)
      // if (Number.isInteger(parseInt(event))) {
        // cityData = e.OrtList.filter(col => {
          // return col.id.match(event)
        // })
      // } else {
        // if (typeof event === 'object') {
          // if (event.target.name == 'offer[]') {
            // var index = e.offerData.indexOf(event.target.value)
            // if (index > -1) {
              // e.offerData.splice(index, 1)
            // } else {
              // e.offerData.push(event.target.value)
            // }
          // }
        // } else {
          // e.churchlist(0, e.selectCity, e.land, false)
        // }
      // }
      // if (cityData.length == 1) {
        // e.center.lat = parseFloat(cityData[0].lat)
        // e.center.lng = parseFloat(cityData[0].lng)
        // e.zoom = 10
        // e.radiusflag = true
        // e.selectCity = cityData[0].id
        // e.page = 1
        // e.churchlist(0, e.selectCity, e.land, false)
        // e.markerCount(e.center.lat, e.center.lng, e.markers)
      // } else {
        // if (!Number.isInteger(parseInt(event))) {
          // e.churchlist(0, e.selectCity, e.land, false)
        // } else {
          // e.churchlist(0, e.selectCity, e.land, false)

          // // e.center.lat = parseFloat("48.116667");
          // // e.center.lng = parseFloat("14.866667");
        // }
      // }
    },
	
	// onchangeofPostcode:function (id) {
      // var e = this
      // axios
        // .get('/adminglobal/getallDataOnchangeOfPostcode?postcodeid=' + id)
        // .then(function (response) {
          // e.dialogLoader = false
          // if (response.data.status == true) {
           // // e.postcodes = response.data.allmuncipaltyforpostcodetype
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
	listofgemeindegrobe:function(countryid)
	{
		if(countryid){
			var idd = countryid;
		}else{
			var idd = 0;
		}
	//	alert(countryid);
		var e = this
      axios
        .get('/adminglobal/getallgemeindegrobe/?id=' + idd)
        .then(function (response) {
          if (response.data.status == true) {
            e.communitySize = response.data.gemeindegrobes.gemeindegrobe
			e.communitySize.unshift({visitor_range:"Alle Gemeindegrößen", id:'000'});

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
				e.StateItems.unshift({StateName:"Alle Bundesländer", ID:'000'});
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
	  if (e.stateids.ID) {
        strQuery += '&stateids=' + e.stateids.ID;
		// console.log(e.stateids);
      } 
      axios
        .get('/adminglobal/getcitybysearch/'+strQuery)
        .then(function (response) {
          if (response.data.status == true) {
			
            e.cityItems = response.data.cities.citydata;
			if(e.cityItems.length == 1 && e.sland.name!="Schweiz" ){
				 e.IsCityselectedForMunicipalty=true;
				  $('#Stadt').css('color', 'black');
				 $('#Municipality').css('color', 'black');
				$('#plz').css('color', 'black');
				e.selectedcityid = response.data.cities.citydata[0].cityId				
			} else {
				
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
	  if (e.stateids.ID) {
        strQuery += '&stateids=' + e.stateids.ID
      } 
	  if (e.selectedcityid.cityId) {
        strQuery += '&cityid=' + e.selectedcityid.cityId
      } 
      axios
        .get('/adminglobal/getmunicipalitybysearch/'+strQuery)
        .then(function (response) {
          if (response.data.status == true) {
			  
            e.MunicipalityItems = response.data.municipalities.municipalities;
			

			// console.log(e.MunicipalityItems);
			// if(e.selectedcityid == '000'){
				
			// e.MunicipalityItems.unshift({municipality_name:"Alle Gemeinden", municipality_id:'000'})
			// }
			if(e.MunicipalityItems.length==1 && e.sland.name!="Schweiz"){
				 $('#Municipality').css('color', 'black');
				$('#plz').css('color', 'black');
				e.selectedmunicipalityid = response.data.municipalities.municipalities[0].municipality_id;
				 e.IsCityselectedForMunicipalty = true;
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
	  if (e.stateids.ID) {
        strQuery += '&stateids=' + e.stateids.ID
      } 
	  if (e.selectedcityid.cityId) {
        strQuery += '&cityid=' + e.selectedcityid.cityId
      } 
	  if (e.selectedmunicipalityid.municipality_id) {
        strQuery += '&municipalityid=' + e.selectedmunicipalityid.municipality_id
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
    // Searchcitywithmuncipality: function (valuesof) {
      // var e = this
	  // var strQuery = ''
	  // if(valuesof){
		   // strQuery += '?value=' + valuesof
	  // }
	  // if (e.sland) {
        // strQuery += '&countryid=' + e.sland
      // }
	  // if (e.stateids) {
        // strQuery += '&stateids=' + e.stateids
      // } 
      // axios
        // .get('/adminglobal/getcitywithmuncipality/'+strQuery)
        // .then(function (response) {
          // if (response.data.status == true) {
            // e.citywithmuncipality = response.data.dataofcities.citywithmuncipality
          // }
        // })
    // },
    listOfChurchetype: function (countryid) {
      var e = this
      axios
        .get('/adminglobal/listOfChurchetype/?countryid=' + countryid)
        .then(function (response) {
          if (response.data.status == true) {
			  	
            e.GemeindetypeList = response.data.churchetypes.churchetype;
			e.GemeindetypeList.unshift({CategoryName:"Alle Konfessionen", ID:'000'});
			
          }
        })
    },
	listofsubChurchetype: function (catId){
	  var e = this
	  e.gemeindesubtyp='';
	  // e.churchesubCategoryLabel = 
	   $('#Konfession').css('color', 'black');
      axios
        .get('/adminglobal/listOfSubChurchetype/?catid='+catId)
        .then(function (response) {
          if (response.data.status == true) {		    
            e.GemeindetypesubList = response.data.churchetypes.churchetype;
			if(e.GemeindetypesubList.length>=1){
				e.churchesubCategoryLabel = response.data.churchetypes.churchetype[0].CategoryName;
				e.isChurchetypeSelected = true;
			} else {
				e.isChurchetypeSelected = false;
			}
          }
        })
	},	
	listofSubSubchurcheType: function (subcatID){
	  var e = this
      axios
        .get('/adminglobal/listofSubSubchurcheType/?subcatID='+subcatID)
        .then(function (response) {
          if (response.data.status == true) {		    
		  
            e.GemeindetypesubsubList = response.data.churchetypes.churchetype;
			if(e.GemeindetypesubsubList.length>=1){
				e.isChurcheSubtypeSelected = true;
			} else {
				e.isChurcheSubtypeSelected = false;
			}
          }
        })
	},	
	listoflanguage:function (countryid) {
		$('#Spraches').css('color', 'black');
	if(countryid){
			var idd = countryid;
		}else{
			var idd = 0;
		}
      var e = this
      axios
        .get('/adminglobal/getalllanguage/?countryid=' + countryid)
        .then(function (response) {			
          if (response.data.status == true) {
				// e.sprache = '';
				$('#Spraches').css('color', 'black');
            e.mylanguage = response.data.languages.language;		
			e.mylanguage.unshift({languagename:"Alle Sprachen", id:'000'});
          }
        })
    },
    listofoffer: function () {
      var e = this
      axios
        .get('/churcheview/offers')
        .then(function (response) {
          if (response.data.status == true) {
            e.offers = response.data.offer
          }
        })
    },
	countrycountofcommunity:function(countryid){
		//alert("ddhghg");
		   var e = this
      axios
        .get('/adminglobal/countrycountofcommunity/?countryid=' + countryid)
        .then(function (response) {			
          if (response.data.status == true) {			  
           e.countrycountofcomm = response.data.countrycounts.countrycount[0];			
		  // console.log(e.countrycountofcomm);
		
          }
        })
		
	},
	
countoforganisationtype:function(Hilfswerke){
		// alert(Hilfswerke);
		   var e = this
      axios
        .get('/adminglobal/countoforganisationtype/?Hilfswerke=' + Hilfswerke)
        .then(function (response) {			
          if (response.data.status == true) {
				var orgid = Hilfswerke+' '+'org';
			e.listoflanguage(orgid);
			e.listofgemeindegrobe(orgid);
           e.countorgnizationcomm = response.data.countorgs.countorg[0].orgcommcount;			
	//	console.log(e.countorgnizationcomm);
		  
          }
        })
		
	},
	listoforgtypecountryofcommunity:function(countryid,orgtype){
		  var e = this
      axios
	  //  .get('/adminglobal/listoforgtypecountryofcommunity/?countryid' + countryid +'&orgtype=' + orgtype )
       .get('/adminglobal/listoforgtypecountryofcommunity/?orgtype=' + orgtype + '&countryid=' + countryid )
        .then(function (response) {			
          if (response.data.status == true) {			  
           e.countryorgcomm = response.data.countryorgs.countryorg[0].orgcountrycount;		
	//	console.log(e.countorgnizationcomm);
		  
          }
        })
	},
	countryonchangefunctions: function(countryobj){
		setTimeout(function(){
		$('#Bundesland').removeAttr('placeholder');
			}, 1);
			setTimeout(function(){
			$('#Stadt').removeAttr('placeholder');
				}, 1);
			setTimeout(function(){
			$('#plz').removeAttr('placeholder');
				}, 1);
		
		if(countryobj.id == 1){
			this.st = "Bundesland";
		}else if(countryobj.id == 2){
			this.st = "Bundesländer";
		}else if(countryobj.id == 3){
			this.st = "Kantone";
		}
		
		var e = this
		if(e.orgtype && e.orgtype !="Alle Organisationen"){
			var organi = e.orgtype
		} else {
			var organi = 'Alle Organisationen'
		}
		e.textandcountmanagement(organi,countryobj.id);
		$('#land').css('color', 'black');
		var cname = countryobj.name+';'+'coun';
		this.getgeolocation(cname);

		if(countryobj.id && countryobj.id != '000'){
		if(countryobj.name=="Schweiz")
		{
			e.IsCityselectedForMunicipalty=false;
			
		} 
		

			var cid = countryobj.id+' '+'Coun';
			e.hidesecond=true;
			e.isCountrynotselected = true;
			
			e.StateItems = [];
			e.cityItems = [];
			e.MunicipalityItems = [];			
			e.postcodes = [];			
			e.stateids = '000';							
			e.selectedcityid = '000';						
			e.selectedmunicipalityid = '';			
			e.plzorpostcode = '000';	

			this.countrycountofcommunity(countryobj.id);
			this.listoflanguage(cid);
			this.listofgemeindegrobe(cid);
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
	statechangefunctions: function(stateobj){
		//console.log(stateobj);
		var e = this
		$('#Bundesland').css('color', 'black');
		var sname = stateobj.StateName+';'+'state';
		this.getgeolocation(sname);
		if(e.orgtype && e.orgtype !="Alle Organisationen"){
			var organi = e.orgtype;
			
			
		} else if(e.sland.id && e.sland.id!='000'){		
			var countryid=e.sland.id;			
		}
		else if(e.orgtype=="Alle Organisationen" &&( e.sland.id && e.sland.id!='000')){
			var organi ="Alle Organisationen";
			var countryid=this.sland.id;
			
		}

		else if((e.orgtype && e.orgtype !="Alle Organisationen")&&(e.sland.id && e.sland.id!='000')){
					var organi = e.orgtype;
					var countryid=e.sland.id;	
				}		
		
		else {
			var organi = 'Alle Organisationen';
			var countryid='000';
		}
		e.textandcountmanagement(organi,countryid,stateobj.ID);
		if(stateobj){
			e.stid = stateobj.ID+' '+'S';
			e.cityItems = [];
			e.MunicipalityItems = [];			
			e.postcodes = [];		
			e.selectedcityid = '000';			
			e.selectedmunicipalityid = '';			
			e.plzorpostcode = '000';			
			this.SearchPlz();
			this.searchcitynames();
			this.listofgemeindegrobe(e.stid);
			this.listoflanguage(e.stid);
			this.searchmunicipalitynames();
			// this.getCityStateMunicipalityOfPostcode(stateobj.ID);
			
		
		}
	},
	gemendegrobecolorchange:function(){
		$('#visitor').css('color', 'black');
	},
	citychangefunctions: function(cityobj){
	//	console.log(cityobj);
		var e = this
		// alert(e.sland.name);
		$('#Stadt').css('color', 'black');
			var cyid = cityobj.cityName+' '+e.sland.name+';'+'city';
			this.getgeolocation(cyid);
		if(cityobj && cityobj != '000'){
			e.ctyid = cityobj.cityId+' '+'C';
			if(e.sland.name=='Schweiz'){
			e.IsCityselectedForMunicipalty = false;
			} else {
				e.IsCityselectedForMunicipalty = true;
			}
			if(e.selectedcityid.cityName=="Alle Städte")
				{
				e.IsCityselectedForMunicipalty = false;
				}
			e.selectedmunicipalityid='000';
			e.MunicipalityItems = [];			
			e.postcodes = [];		
			
			e.plzorpostcode = '000';
			this.getStateofCity(cityobj.cityId);
			this.listofgemeindegrobe(e.ctyid);	
			this.listoflanguage(e.ctyid);
			this.SearchPlz();
			this.searchmunicipalitynames();
		
			// this.searchcitynames();
		} else {
			e.IsCityselectedForMunicipalty = false;
		}
	},
	
	municipalitychangefunctions: function(municipalityobj){
		//console.log(municipalityobj);
		var e = this
		
		$('#Municipality').css('color', 'black');
		var mid = municipalityobj.municipality_name+' '+e.sland.name+';'+'muncipality';
		this.getgeolocation(mid);
		if(municipalityobj){
		var mnpid = municipalityobj.municipality_id+' '+'munci';
		this.listofgemeindegrobe(mnpid);
		this.listoflanguage(mnpid);
		this.SearchPlz();
			e.postcodes = [];			
			e.plzorpostcode = '';
			this.getCityStateOfMunicipality(municipalityobj.municipality_id);
			
			
		}
	},
	postcodechangefunctions: function(postcodeobj){
		//console.log(postcodeobj);
		var e = this
		$('#plz').css('color', 'black');
		var pid = postcodeobj.title+';'+'zip';
		this.getgeolocation(pid);			
		if(postcodeobj){	
			var pcid = postcodeobj.title+' '+'zip';
			this.listofgemeindegrobe(pcid);
			this.listoflanguage(pcid);
			this.getCityStateMunicipalityOfPostcode(postcodeobj.title);		
			
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
				  // console.log(response.data);
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
	
    churchlist: function ($state,pageNum = 0, city = 0, country = 14, all = true) {
		
		//console.log($state);
      var e = this
	  //e.churchesList = []
      var strQuery = '' 
	  if(e.orgtype){
		  strQuery +='&orgtype=' + e.orgtype
	  }
	  if (e.sland.id) {
        strQuery += '&country=' + e.sland.id
      }
	  if (e.stateids.ID) {
        strQuery += '&stateids=' + e.stateids.ID
      } 
	  // if (e.citywithmuncipalityid) {
        // strQuery += '&citywithmuncipalityid=' + e.citywithmuncipalityid
      // }	
	  if (e.selectedcityid.cityId) {
        strQuery += '&cityid=' + e.selectedcityid.cityId
      } 
	  if (e.selectedmunicipalityid.municipality_id) {
        strQuery += '&municipalityid=' + e.selectedmunicipalityid.municipality_id
      } 	  
      if (e.plzorpostcode.title) {
        strQuery += '&plzorpostcode=' + e.plzorpostcode.title
      }
	  if(e.radius){
		  strQuery += '&radiusof=' + e.radius
	  }
      if (e.gemeindetyp) {
        strQuery += '&gemeindetyp=' + e.gemeindetyp
      }
      if (e.gemeindesubtyp) {
        strQuery += '&gemeindesubtyp=' + e.gemeindesubtyp
      }
      if (e.GemeindesubSubtyp) {
        strQuery += '&GemeindesubSubtyp=' + e.GemeindesubSubtyp
      }
	  if (e.gemeindegrobe) {
        strQuery += '&gemeindegrobe=' + e.gemeindegrobe
      }
      if (e.sprache) {
		  
        strQuery += '&sprache=' + e.sprache
      }
	  if (e.offerData.length > 0) {
        strQuery += '&offerData=' + e.offerData
      }	  
      axios
        .get(
          '/churcheview/churchelist?p=' +
            e.pagginginfinte +
            '&all=' +
            all +
            strQuery
        )
        .then(function (response) {
          if (response.data.status == true) {
			  e.totalcount = response.data.churches.communitycount;
			  if(e.totalcount==0){
				  e.isCountofgemeindegrobe=false
			  }
			  else{
				  e.isCountofgemeindegrobe=true
			  }
			   e.communitycountaaaaa = response.data.churches.communitycountaaaaa;
			                 e.markers = []
              for (var prop in response.data.churches.community) {
                e.markers.push({
                  lat: parseFloat(response.data.churches.community[prop].lat),
                  lng: parseFloat(response.data.churches.community[prop].lng),
				  title: response.data.churches.community[prop].title,
				  slug:response.data.churches.community[prop].slug,
                })
              }
              e.markerCount(e.center.lat, e.center.lng, e.markers)
			  if (response.data.churches.community) {
		  
	
			
			   // console.log(e.pagginginfinte);
			 
		  // if (response.data.churches) {
			   e.pagginginfinte += 1;
				e.churchesList.push(...response.data.churches.community);
				$state.loaded();
			  // console.log(response.data);
              e.topTotalChurches = Math.round(
                (response.data.churches.topChurches /
                  response.data.churches.communitycount) *
                  100
              )

            // } else {
              // e.totalcount = 0
              // e.markers = []
			  //$state.complete();
            // }
        } else {
			$state.complete();
			 setTimeout(function(){
		   $state.reset();
	  },5000)
			// e.churchesList=[];
			// console.log($state.error());
			// alert('asdadsf');
			// console.log("complet " + $state.complete())
			// if($state.error()){
				
			// }
         
		  // e.totalcount = 0
              e.markers = []
        }
            // e.churchesList = response.data.churches.community;
			// e.comtotal = response.data.churches.communitycount;
            
          }
          // console.log(response.status);
          // console.log(response.statusText);
          // console.log(response.headers);
          // console.log(response.config);
        })
    },	
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
    onPageChange: function (pageNum) {
      if (this.countryflag == true) {
        this.selectCity = 0
      }
      let all = false
      if (this.selectCity == 0 && this.land == 0) {
        all = false
      } else {
        all = true
      }
      this.churchlist(pageNum - 1, this.selectCity, this.land, all)
    },
    markerCount: function (lat, lng, marker) {
      let count = 0
      marker.forEach(element => {
        var loc1 = lat + ',' + lng // Marker Radius Co-ords
        var loc2 = element.lat + ',' + element.lng // Marker Co-ords
        var nyc = new google.maps.LatLng(lat, lng)
        var london = new google.maps.LatLng(element.lat, element.lng)
        var distance = google.maps.geometry.spherical.computeDistanceBetween(
          nyc,
          london,
          this.radius
        )
        if (distance > 0) {
          count++
        }
      })
      this.radiusCircle = count
    },
	
	textandcountmanagement: function(organization_name, country_name,state_name){
		var e = this
		
		if(organization_name=='Alle Organisationen' && country_name == '000'){
			$('#alignfirsttext').css('margin-top', '6px');
			e.txtofnew='Anzahl an Organisationen';
			var alleorg='Alle Organisationen';
			e.secondtextnew='';
		}
		else if(country_name != '000' && organization_name != 'Alle Organisationen'){
			if(organization_name=='Hilfswerke' || organization_name=='Missionswerke')
			{
				$('#alignfirsttext').css('margin-top', '0px');
			e.txtofnew='Anzahl an '+organization_name+'n in gewählter Region';
			e.secondtextnew='Anzahl an '+organization_name+'n mit Ihrer Vorauswahl';
			var countryid=e.sland.id;
			} else {
				$('#alignfirsttext').css('margin-top', '0px');
				e.txtofnew='Anzahl an '+organization_name+' in gewählter Region';
			e.secondtextnew='Anzahl an '+organization_name+' mit Ihrer Vorauswahl';
			var countryid=e.sland.id;
			}
		}
		else if(country_name == '000' && organization_name != 'Alle Organisationen'){
			if(organization_name=='Hilfswerke' || organization_name=='Missionswerke')
			{
				$('#alignfirsttext').css('margin-top', '6px');
				
			var Hilfswerke='Hilfswerke';
			e.txtofnew='Anzahl an '+organization_name+'n';
			e.secondtextnew='';
			}
			else{
					$('#alignfirsttext').css('margin-top', '6px');
				var Hilfswerke='Hilfswerke';
			e.txtofnew='Anzahl an '+organization_name+'';
			e.secondtextnew='';
			}
		} 
		else if(country_name != '000' && organization_name == 'Alle Organisationen') {
			$('#alignfirsttext').css('margin-top', '0px');
			e.txtofnew=' Anzahl an Organsiationen in gewählter Region';
			e.secondtextnew=' Anzahl an Organisationen mit Ihrer Vorauswahl';
		}
		e.getCountByFilters(country_name,organization_name,state_name);
	},
	
	getCountByFilters:function(countryid,orgtype,stateid){
	  var e = this
      axios
       .get('/adminglobal/getCountByFilters/?orgtype=' + orgtype + '&countryid=' + countryid +'&stateid='+ stateid)
        .then(function (response) {			
          if (response.data.status == true) {			  
           e.countryorgcomm_new = response.data.countryorgs.countryorg[0];	
		   e.newcountsof = e.countryorgcomm_new.orgcountrycount
          }
        })
	},

}
}

</script>
<!--<script src="https://unpkg.com/vue-scroll-loader"></script>-->
<style>
.progess-custom {
  margin: 0px;
}
.v-responsive.v-image.imgDeal {
  margin-right: 5px !important;
}
#Stadt{
	color:lightgrey;
}
#Municipality{
	color:lightgrey;
}
#Bundesland{
	color:lightgrey;
}
#visitor{
	color:lightgrey;
}
#Spraches{
	color:lightgrey;
}
#Konfession{
	color:lightgrey;
}
#plz{
	color:lightgrey;
}
label.v-label.v-label--active.theme--light {
    margin-top: -5px;
}
/*.v-select__selection.v-select__selection--comma {
    color: lightgrey;
}*/
</style>

