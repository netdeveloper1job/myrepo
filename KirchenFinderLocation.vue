<template>
<v-layout
      text-center
      wrap
    >
	<v-flex md12 lg12 xll2 xs4 sm6>
  <v-content class="mainWrapper white">
    <section class="kitchenFinderSectionTop">
      <v-container class="grey lighten-3 py-2">
        <v-layout row wrap>
          <v-flex xs12 md5>
            <v-form>
              <v-autocomplete
                :loading="searchLoading"
                :items="churcheTitle"
                item-text="title"
                item-value="link"
                :search-input.sync="search"
                @input="pageRedirect"
                cache-items
                flat
                hide-no-data
                hide-details
                label="Kirchenname oder Stichwort einfügen"
                append-icon="search"
                solo
              ></v-autocomplete>
              <!-- <v-text-field
                flat
                solo
                class
                placeholder="Gemeindename oder Stichwort einfugen"
                append-icon="search"
              ></v-text-field>-->
            </v-form>
          </v-flex>
        <!--  <v-flex xs12 md2>
            <v-btn small flat class="grey lighten-3 right ma-0 black--text btnSuche">
              Erweiterte Suche
              <v-icon class="ml-1">expand_more</v-icon>
            </v-btn>
          </v-flex>-->
        </v-layout>
      </v-container>
   <!--   <section class="newsRoomSectionTop">
        <v-container class="white py-0">
          <v-layout>
            <v-flex xs12>
              <v-btn flat class="white black--text ma-0 px-0 btnTab">Kirchen</v-btn>
            </v-flex>
          </v-layout>
        </v-container>-->
      </section>
    </section>
    <section class="kitchenFinderSection" v-if="churchesData">
      <v-container fluid grid-list-xl>
        <v-layout row wrap justify-center class="kirchenTopDetails">
          <v-flex xs12 md4>
            <v-dialog light v-model="editDialoglogo" max-width="440px">
              <v-card class="editDialog pa-3">
                <v-card-title class="subheading pd-1">Logo:</v-card-title>
                <v-card-text class="py-0">
                  <v-flex xs12>
                    <v-image-input
                      v-model="imageData"
                      :image-quality="0.85"
                      clearable
					  minScaling="contain"
                      image-format="jpeg"
                    />
                  </v-flex>
                </v-card-text>
                <v-card-actions>
                  <v-btn
                    depressed
                    class="grey shivani"
                    @click="imageData = ''; editDialoglogo = false"
                  >Abbrechen</v-btn>
                  <v-btn depressed dark color="orange darken-1" @click="saveLogo">Speichern</v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
            <v-img
              v-if="churcheslogo"
              v-bind:src="'http://dev.woobii.com/admin/'+churcheslogo"
              max-width="175"
              class="mb-3"
              srcset
              lazy-src
            >
              <v-btn
                fab
                class="right"
                dark
                color="darken-1"
                @click="editDialoglogo = true"
                style="height: 25px;width: 25px;"
                v-if="userRoleCall != 1 && packageConditions.IsUploadLogo"
              >
                <v-icon style="font-size:12px">edit</v-icon>
              </v-btn>
            </v-img>
             <v-img
					v-else-if="!churcheslogo && primaryForm.logochurche && primaryForm.churchesubCategorytwo == 0 && primaryForm.churchesubsubCategorytwo == 0"
					v-bind:src="'http://dev.woobii.com/admin/'+primaryForm.logochurche"
					lazy-src
					max-width="175"
					class="mb-3"
			>
			<v-btn
			fab
			class="right"
			dark
			color="darken-1"
			@click="editDialoglogo = true"
			style="height: 25px;width: 25px;"
			v-if="userRoleCall != 1 && packageConditions.IsUploadLogo"
			>
			<v-icon style="font-size:12px">edit</v-icon>
			</v-btn>
			</v-img>
			<v-img
			v-else-if="!churcheslogo && primaryForm.logosubchurche && primaryForm.churchecategorytwo != 0 && primaryForm.churchesubsubCategorytwo == 0"
			v-bind:src="'http://dev.woobii.com/admin/'+primaryForm.logosubchurche"
			lazy-src
			max-width="175"
			class="mb-3"
			>
			<v-btn
			fab
			class="right"
			dark
			color="darken-1"
			@click="editDialoglogo = true"
			style="height: 25px;width: 25px;"
			v-if="userRoleCall != 1 && packageConditions.IsUploadLogo"
			>
			<v-icon style="font-size:12px">edit</v-icon>
			</v-btn>
			</v-img>
			<v-img
			v-else-if="!churcheslogo && primaryForm.logosubsubchurche && primaryForm.churchecategorytwo != 0 && primaryForm.churchesubCategorytwo != 0 && primaryForm.churchesubsubCategorytwo != 0"
			v-bind:src="'http://dev.woobii.com/admin/'+primaryForm.logosubsubchurche"
			lazy-src
			max-width="175"
			class="mb-3"
			>
			<v-btn
			fab
			class="right"
			dark
			color="darken-1"
			@click="editDialoglogo = true"
			style="height: 25px;width: 25px;"
			v-if="userRoleCall != 1 && packageConditions.IsUploadLogo"
			>
			<v-icon style="font-size:12px">edit</v-icon>
			</v-btn>
			</v-img>
			<v-img
              v-else
              :src="require(`@/assets/woobii-banner.jpg`)"
              :lazy-src="require(`@/assets/woobii-banner.jpg`)"
              max-width="175"
              class="mb-3"
            >
              <v-btn
                fab
                class="right"
                dark
                color="darken-1"
                @click="editDialoglogo = true"
                style="height: 25px;width: 25px;"
                v-if="userRoleCall != 1 && packageConditions.IsUploadLogo"
              >
                <v-icon style="font-size:12px">edit</v-icon>
              </v-btn>
            </v-img>
            <p class="headline mb-2">{{ churchesData[0].title }}
			<v-btn
                fab
                class="right"
                dark
                color="darken-1"
                @click="editPrimaryThings = true"
                style="height: 25px;width: 25px;"
                v-if="userRoleCall != 1"
              >
                <v-icon style="font-size:12px">edit</v-icon>
              </v-btn>
			<!-- <span class="wrap">
                            <v-tooltip right>
                               <h4 class="body-1">Lorem Ipsum is simply dummy text. <br> Lorem Ipsum is simply dummy text. <br>Lorem Ipsum is simply dummy text. <br>Lorem Ipsum is simply dummy text. <br>Lorem Ipsum is simply dummy text. <br>Lorem Ipsum is simply dummy text. <br>Lorem Ipsum is simply dummy text. <br></h4>
							    <v-icon style="font-size:25px" slot="activator" class="imgDeal">info</v-icon>
                            </v-tooltip>
                          </span>-->

			  </p>
            <p class="subheading mb-1">
			
			 <span class="caption" v-if="!churchesData[0].SubcatName && !churchesData[0].subsubCatName">{{ churchesData[0].CategoryName }}</span>
            <span class="caption" v-if="!churchesData[0].subsubCatName">{{ churchesData[0].SubcatName }}</span>
			 <span class="caption" v-if="">{{ churchesData[0].subsubCatName }}</span>

            </p>
			
            <p class="subheading mb-1">
              {{ churchesData[0].city }}
              <br/>
              <span class="caption">{{ churchesData[0].country }}</span>
            </p>
            <p class="subheading mb-2"  v-if="churchesData[0].Ranges">
              {{ churchesData[0].Ranges }} 
              
			  
			  
              <span class="caption">Besucher</span>
              </p>
            
			 <p class="subheading mb-2">
			
			 <span class="caption" v-for="(val,index) in churchesData.languagesfrom">
			 {{val.languagename}}<span v-if="index != churchesData.languagesfrom.length - 1">, </span></span>
			</p>
			<!-- <p class="subheading mb-2">
              {{ churchesData[0].langauages }} 
              <span class="caption">Besucher</span>
            </p>-->

			<v-dialog light v-model="editPrimaryThings" max-width="650px">
                <v-card class="editDialog pa-3">
                  <v-card-title class="subheading pd-1">Gemeindeprofil</v-card-title>
                  <v-card-text class="py-0">
				  <v-flex xs12>
                      <v-text-field
                        label="Community Name"
                        v-model="primaryForm.comm_name"
                        colClasses="xl3 lg3 md3 sm3 xs3"
                      ></v-text-field>
                    </v-flex>

					<v-layout row wrap>

					<v-flex xs12 sm12 md12>
						<v-select
							label="Select Visitor Range"
							v-model="primaryForm.visitorranges"
							search-input="primaryForm.visitorranges"
							:items="visitorRangItems"
							item-text="Ranges"
							item-value="ID"
						  ></v-select>
					</v-flex>
					<v-flex xs12 sm12 md12>
						<v-select
							label="Select Churche Type"
							v-model="primaryForm.churchecategory"
							search-input="primaryForm.churchecategory"
							:items="ChurcheCategories"
							@change="listOfChurcheSubCategories"
							item-text="CategoryName"
							item-value="ID"
						></v-select>
					</v-flex>
					<v-flex xs12 sm12 md12>
						<v-select
							v-if="isChurchetypeSelected"
							id="churchesubtype"
							label="Select Churche Sub Type"
							v-model="primaryForm.churchesubCategory"
							:items="churchesubCategories"
							@change="listOfChurcheSubSubCategories"
							item-text="SubcatName"
							item-value="ID"
						></v-select>
					</v-flex>
					<v-flex xs12 sm12 md12>
						<v-select
							v-if="isChurchesubtypeSelected"
							label="Select Churche Sub Sub Type"
							v-model="primaryForm.churchesubsubCategory"
							:items="churchesubsubCategories"
							item-text="subsubCatName"
							item-value="ID"
						></v-select>
					</v-flex>
						<!--<v-flex xs12 md10 offset-md1>
                            <v-autocomplete
                              light
                              placeholder="Churche Type"
                              solo
                              v-model=""
							  @change="listofmuncipalty"
                              :items="OrtList"
                              v-validate="'required'"
                              :error-messages="errors.collect('')"
                              label="Ort"
                              item-text="place"
                              item-value="id"
                              data-vv-name="ort"
                            ></v-autocomplete>
						</v-flex>-->

                    <!-- <v-flex xs12 md2>
                          <v-text-field
                            v-model="visiterrange[0]"
                            class="mt-0"
                            label="Visitor Min"
                            hide-details
                            single-line
                          ></v-text-field>
                        </v-flex>
                        <v-flex xs12 md8>
                          <v-range-slider v-model="visiterrange" :max="1000" :min="1" :step="10"></v-range-slider>
                        </v-flex>
                        <v-flex xs12 md2>
                          <v-text-field
                            label="Visitor Max"
                            v-model="visiterrange[1]"
                            class="mt-0"
                            hide-details
                            single-line
                          ></v-text-field>
                        </v-flex>-->
						<v-flex xs12 sm12 md12>
					<v-combobox
						v-model="primaryForm.chipslanguage"
						:items="languageItems"
						chipslanguage
						clearable
						label="Select Language"
						multiple
						solo
						max=5
						item-text="languagename"
						item-value="languageid"
					  >
						<template v-slot:selection="{ attrs, item, select, selected }">
						  <v-chip
							v-bind="attrs"
							:input-value="selected"
							close
							@click="select"
							@click:close="myremove(item)"
						  >
							<strong>{{ item }}</strong>&nbsp;
							<span>(interest)</span>
						  </v-chip>
						</template>
					</v-combobox>
				</v-flex>
                    </v-layout>

                  </v-card-text>
                  <v-card-actions>
                    <v-btn
                      depressed
                      class="grey lighten-2"
                      @click="editPrimaryThings = false"
                    >Abbrechen</v-btn>
                    <v-btn depressed dark @click="editPrimaryThingsBackend" color="orange darken-1">Speichern</v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>

            <!--  <p class="caption mb-0">
              <v-rating
                v-model="rating"
                readonly
                dense
                background-color="grey darken-4"
                color="grey darken-4"
            ></v-rating>-->
            <!-- <v-icon class="black--text">star</v-icon>
              <v-icon class="black--text">star</v-icon>
              <v-icon class="black--text">star</v-icon>
              <v-icon class="black--text">star_half</v-icon>
            <v-icon class="black--text">star_border</v-icon>-->
            <!-- 40 Besucherbewertungen
            </p>-->
          </v-flex>
          <v-flex xs12 md8>
		  <v-dialog light v-model="OpenEditBannerDialog" max-width="440px">
              <v-card class="editDialog pa-3">
                <v-card-title class="subheading pd-1">Titelbild hochladen:</v-card-title>
                <v-card-text class="py-0">
                  <v-flex xs12>
                    <v-image-input
                      v-model="BannerimageData"
                      :image-quality="0.85"
                      clearable
					  minScaling="contain"
                      image-format="jpeg"
                    />
                  </v-flex>
                </v-card-text>
                <v-card-actions>
                  <v-btn
                    depressed
                    class="grey lighten-2"
                    @click="BannerimageData = ''; OpenEditBannerDialog = false"
                  >Abbrechen</v-btn>
                  <v-btn depressed dark color="orange darken-1" @click="saveBannerImage">Speichern</v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
            <v-img style="height: 350px;"
              v-if="bannerLogoHere"
              :lazy-src="'http://dev.woobii.com/admin/'+bannerLogoHere"
              :src="'http://dev.woobii.com/admin/'+bannerLogoHere"
            >
			<v-btn
                fab
                class="right"
                dark
                color="darken-1"
                @click="OpenEditBannerDialog = true"
                style="height: 25px;width: 25px;"
                v-if="userRoleCall != 1 && packageConditions.isUploadHeaderPhoto"
              >
                <v-icon style="font-size:12px">edit</v-icon>
              </v-btn>
            </v-img>
            <v-img
              v-else
              :src="require(`@/assets/woobii-banner.jpg`)"
              :lazy-src="require(`@/assets/woobii-banner.jpg`)"
            >
			<v-btn
                fab
                class="right"
                dark
                color="darken-1"
                @click="OpenEditBannerDialog = true"
                style="height: 25px;width: 25px;"
                v-if="userRoleCall != 1 && packageConditions.isUploadHeaderPhoto"
              >
                <v-icon style="font-size:12px">edit</v-icon>
              </v-btn>
            </v-img>
          </v-flex>
          <v-flex xs12 md8 class>
            <v-tabs slider-color="grey darken-3" v-model="active_tab" hide-slider="true">
              <v-tab grow href="#tab-1">Über uns</v-tab>
              <v-tab href="#tab-2" v-if="isshowAngebote"> Angebote</v-tab>
              <v-tab href="#tab-3">Neuigkeiten</v-tab>
			  <v-tab href="#tab-4">Impulse</v-tab>
              <v-tab href="#tab-5">Social Media Wall</v-tab>
              <v-tab href="#tab-6">Events</v-tab>
              <v-tab href="#tab-7">Jobs</v-tab>
              <!-- <v-tab href="#tab-7">Bewertungen</v-tab> -->
              <v-tab-item id="tab-1">
                <v-card flat>
                  <v-card-text>
                    <v-layout row wrap>
                      <v-flex xs12>
                        <span class="headline">Über uns</span>
                        <v-btn
                          fab
                          small
                          class="right"
                          dark
                          @click="editDialogdesc = true"
                          style="height: 20px; padding: 0px; width: 20px;"
                          v-if="userRoleCall != 1  && packageConditions.IsDescription"
                        >
                          <v-icon style="font-size:12px">edit</v-icon>
                        </v-btn>
                        <v-dialog light v-model="editDialogdesc" max-width="650px">
                          <v-card class="editDialog pa-3">
                            <v-card-title class="subheading pd-1">Über uns:</v-card-title>
                            <v-card-text class="py-0">
                              <v-flex xs12>
                                <wysiwyg name="description" v-model="aboutus" placeholder="Enter text" required />
                              </v-flex>
                            </v-card-text>
                            <v-card-actions>
                              <v-btn
                                depressed
                                class="grey lighten-2"
                                @click="editDialogdesc = false"
                              >Abbrechen</v-btn>
                              <v-btn
                                depressed
                                dark
                                @click="saveAboutUs"
                                color="orange darken-1"
                              >Speichern</v-btn>
                            </v-card-actions>
                          </v-card>
                        </v-dialog>
                        <div
                          class="blue-grey lighten-5 text-xs-center pa-2"
                          v-if="churchesData[0].about_us == ' '"
                        >
                          <span class="subtitle blue-grey--text">
                            <v-icon large class="blue-grey--text">error_outline</v-icon>
                            <br />Keine Informationen vorhanden.
                            <br />
                     <!--     <span class="title">Über uns</span>-->
                          </span>
                        </div>
						<vimeo-player :video-id='360654782'></vimeo-player>
						<vimeo-player ref="player" :video-id="360654782" @ready="true" :autoplay="true" :player-width="640" :player-heigh="320"></vimeo-player>
                        <!-- <div v-html="churchesData[0].about_us"></div>
                        <p class="body-1 font-weight-bold">
                          <v-icon small class="black--text mr-1">expand_more</v-icon>Weiterlesen
                        </p>-->
						<truncate clamp="Zeig mehr" :length="500" less="Show Less" type="html" v-bind:text="churchesData[0].about_us"></truncate>

                        <v-layout row wrap v-if="userRoleCall == 1">
						<p
                          class="headline"
                          style="margin-top: 15px;"
                        >WOOBII.at empfiehlt folgende Gemeinden:</p>
                          <v-flex d-flex xs12 md6 v-for="churche in churchesData.featuredChurch">
                            <v-card flat color="white" class="kFinderWrap">
                              <a :href="'/Kirchenfinder/'+churche.slug" class="caption black--text">
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
                              </a>
                              <v-card-text class="pa-0">
                                <a
                                  :href="'/Kirchenfinder/'+churche.slug"
                                  class="caption black--text"
                                >
                                  <h4 class="body-1 my-2">{{ churche.title }}</h4>
                                </a>
                                <div>
                                  <v-chip
                                    small
                                    label
                                    dark
                                    class="white--text my-1"
                                    v-for="tag in splitTag(churche.tags)"
                                  >Top gemeinde</v-chip>
                                </div>
                                <div class="my-2">
                                  <v-rating
                                    v-model="churche.rating"
                                    readonly
                                    dense
                                    small
                                    background-color="grey darken-4"
                                    color="grey darken-4"
                                  ></v-rating>
                                  <!-- <v-icon small class="black--text">star</v-icon>
                      <v-icon small class="black--text">star</v-icon>
                      <v-icon small class="black--text">star</v-icon>
                      <v-icon small class="black--text">star_half</v-icon>
                                  <v-icon small class="black--text">star_border</v-icon>-->
                                  <span class="body-1 font-weight-bold ml-2">{{churche.rating}}</span>
                                </div>
                                <a
                                  :href="'/Kirchenfinder/'+churche.slug"
                                  class="caption black--text"
                                >
                                  <v-icon small class="black--text">chevron_right</v-icon>Zur Gemeinde
                                </a>
                              </v-card-text>
                            </v-card>
                          </v-flex>
                          <!-- <v-flex d-flex xs12 md6>
                            <v-card flat color="white" class="kFinderWrap">
                              <v-img src="https://cdn.vuetifyjs.com/images/cards/desert.jpg"></v-img>
                              <v-card-text class="pa-0">
                                <h4 class="body-1 my-2">Every Nation Innsbruck</h4>
                                <div>
                                  <v-chip small label dark class="white--text ma-0">Top Gemeinde</v-chip>
                                </div>
                                <div class="my-2">
                                  <v-rating
                                    v-model="rating"
                                    readonly
                                    dense
                                    small
                                    background-color="grey darken-4"
                                    color="grey darken-4"
                                  ></v-rating>
                                  <span class="body-1 font-weight-bold ml-2">4,49</span>
                                </div>
                                <a href class="caption black--text">
                                  <v-icon small class="black--text">chevron_right</v-icon>Zur Gemeinde
                                </a>
                              </v-card-text>
                            </v-card>
                          </v-flex>
                          <v-flex d-flex xs12 md6>
                            <v-card flat color="white" class="kFinderWrap">
                              <v-img src="https://cdn.vuetifyjs.com/images/cards/desert.jpg"></v-img>
                              <v-card-text class="pa-0">
                                <h4 class="body-1 my-2">Every Nation Innsbruck</h4>
                                <div>
                                  <v-chip small label dark class="white--text ma-0">Top Gemeinde</v-chip>
                                </div>
                                <div class="my-2">
                                  <v-rating
                                    v-model="rating"
                                    readonly
                                    dense
                                    small
                                    background-color="grey darken-4"
                                    color="grey darken-4"
                                  ></v-rating>
                                  <span class="body-1 font-weight-bold ml-2">4,49</span>
                                </div>
                                <a href class="caption black--text">
                                  <v-icon small class="black--text">chevron_right</v-icon>Zur Gemeinde
                                </a>
                              </v-card-text>
                            </v-card>
                          </v-flex>-->
                        </v-layout>

						<v-dialog light v-model="uploadimagevideos" max-width="650px">
                          <v-card class="editDialog pa-3">
                            <v-card-title class="subheading pd-1">Bilder hochladen</v-card-title>

							<v-tabs
							  v-model="tab"
							  background-color="transparent"
							  color="basil"
							  grow
							>
							  <v-tab>
								Bild hochladen
							  </v-tab>
							  <v-tab>
								Video hochladen
							  </v-tab>
							</v-tabs>

							<v-tabs-items v-model="tab">
							  <v-tab-item>
								<v-card flat color="basil">
									<v-card-text class="py-0">
									  <v-flex xs12>
										<v-image-input
										  v-model="ImageUrls"
										  :image-quality="0.85"
										  clearable
										  minScaling="contain"
										  image-format="jpeg"
										/>
									  </v-flex>
									</v-card-text>
									<v-card-actions>
									  <v-btn
										depressed
										class="grey lighten-2"
										@click="uploadimagevideos = false"
									  >Abbrechen</v-btn>
									  <v-btn
										depressed
										dark
										@click="saveImagesHere"
										color="orange darken-1"
									  >Speichern</v-btn>
								</v-card-actions>
								</v-card>
							  </v-tab-item>
							  <v-tab-item>
								<v-layout xs12>
									<v-card-text xs12 md6>
										<v-text-field
										  slot="activator"
										  v-model="videoForm.title"
										  label="Enter Video Title"
										></v-text-field>
									</v-card-text>
									<v-card-text xs12 md6>
										<v-text-field
										  slot="activator"
										  v-model="videoForm.youtubeparameter"
										  label="eg:- After ?v= text"
										></v-text-field>
									</v-card-text>
								</v-layout>
								<v-card-text>
									<v-text-field
									  slot="activator"
									  v-model="videoForm.videoUrl"
									  label="Enter Video URL"
									></v-text-field>
								</v-card-text>
								<v-card-actions>
								  <v-btn
									depressed
									class="grey lighten-2"
									@click="uploadimagevideos = false"
								  >Abbrechen</v-btn>
								  <v-btn
									depressed
									dark
									@click="saveVideos"
									color="orange darken-1"
								  >Speichern</v-btn>
								</v-card-actions>
							  </v-tab-item>
							</v-tabs-items>
                          </v-card>
                        </v-dialog>
						<v-layout row wrap v-if="userRoleCall != 1">
							<v-flex md12 xs12>
								<v-btn
									  fab
									  small
									  dark
									  @click="uploadimagevideos = true"
									  style="height: 35px; padding: 0px; width:35px;"
									  v-if="userRoleCall != 1  && packageConditions.IsUploadPhotos"
									>
								<v-icon style="font-size:25px">add</v-icon>
							</v-btn>
							</v-flex>

						  <v-flex d-flex xs12 md4 v-if="imageIndex < images.length" v-for="(n, imageIndex) in ImagecounttoShow" :key="imageIndex">
                            <v-card flat color="white" class="kFinderWrap imagesvideocard hovereffect" style="padding: 2px;">
								<v-img v-if="images[imageIndex].poster"
                                  :lazy-src="images[imageIndex].poster"
                                  :src="images[imageIndex].poster"
                                >
								 <div class="overlay">									
									<v-btn class="mx-2" style="height: 35px; padding: 0px; width:35px; margin-top:28%" fab dark small color="red" @click="deleteImageVideo(images[imageIndex].ID)" title="delete">
									  <v-icon dark>delete</v-icon>
									</v-btn>
									<v-btn class="mx-2" style="height: 35px; padding: 0px; width:35px; margin-top:28%" fab dark small color="blue" 
									@click="index = imageIndex" title="view">
									  <i class="fa fa-eye"></i>
									</v-btn>									
								</div>
								
								</v-img>
								<v-img
								  v-else
								  :src="require(`@/assets/woobii-banner.jpg`)"
								  :lazy-src="require(`@/assets/woobii-banner.jpg`)"
								/>
                            </v-card>
                          </v-flex>
							<button v-if="images.length > 6" @click="ImagecounttoShow = images.length" class=""><v-icon>expand_more</v-icon>show more ({{images.length-ImagecounttoShow}})</button>
						<gallery
						  :images="images"
						  :index="index"
						  :options="{youTubeVideoIdProperty: 'youtube', youTubePlayerVars: undefined, youTubeClickToPlay: true}"
						  @close="index = null"
						  ></gallery>
					</v-layout>
                      </v-flex>
                    </v-layout>
                  </v-card-text>
                </v-card>
              </v-tab-item>
              <v-tab-item id="tab-2">
                <v-card flat>
                  <v-card-text>
                    <v-layout row wrap>
                      <v-flex xs12>
                        <p class="headline">Angebote</p>
                        <div
                          class="blue-grey lighten-5 text-xs-center pa-2"
                          v-if="churchesData.offers == ''"
                        >
                          <span class="subtitle blue-grey--text">
                            <v-icon large class="blue-grey--text">error_outline</v-icon>
                            <br />No data found in
                            <br />
                            <span class="title">Angebote</span>
                          </span>
                        </div>
                       <div class="dealsWrap px-2" v-if="userRoleCall != 1">
                          <div class="wrap" v-for="offer in churchesData.offers">
                            <v-tooltip top>
                               <h4 class="body-1">{{ offer.offer_name }}</h4>
                              <v-img
                                slot="activator"
                                v-if="offer.offer_img"
                                :lazy-src="'http://dev.woobii.com/admin/'+offer.offer_img"
                                :src="'http://dev.woobii.com/admin/'+offer.offer_img"
                                class="imgDeal"
								@click="saveserviceforthis(offer.offer_id)"
                              >
                                <span v-if="offer.colorfillable == 'gray'" class="backgroundColor background" ></span>
                                <span v-if="offer.colorfillable == 'blue'" class="backgroundColor" style="background-color:rgb(11, 50, 79) !important;"></span>
                              </v-img>
                            </v-tooltip>
                          </div>
                        </div>
						<div class="dealsWrap px-2" v-else>
                          <div class="wrap" v-for="offer in churchesData.offersfornormal">
                             <v-tooltip top>
                              <h4 class="body-1">{{ offer.offer_name }}</h4>
                              <v-img
                                slot="activator"
                                v-if="offer.offer_img"
                                :lazy-src="'http://dev.woobii.com/admin/'+offer.offer_img"
                                :src="'http://dev.woobii.com/admin/'+offer.offer_img"
                                class="imgDeal"
                              >
                                 <span v-if="offer.colorfillable == 'gray'" class="backgroundColor background"></span>
                                <span v-if="offer.colorfillable == 'blue'" class="backgroundColor" style="background-color:rgb(11, 50, 79) !important;"></span>
                              </v-img>
                            </v-tooltip>
                          </div>
                        </div>
                      </v-flex>
                    </v-layout>
                  </v-card-text>
                </v-card>
              </v-tab-item>
              <v-tab-item id="tab-3">
                <v-card flat>
                  <v-card-text>
                    <v-layout row wrap>
                      <v-flex xs12>
                        <p class="headline">News
						<v-btn
							  fab
							  small
							  dark
							  class="right"
							  @click="addnewsroomform(); editDialognewsroom = true"
							  style="height: 30px; padding: 0px; width: 30px;"
							  v-if="userRoleCall != 1"
							>
							<v-icon style="font-size:20px">add</v-icon>
                        </v-btn></p>
						
                       <!-- <v-container grid-list-md class="pa-0 tab3rd-edit-panel">
                          <v-layout row wrap>
                            <v-flex xs12>
                              <v-card light color="grey lighten-4">
                                <v-card-text>
                                  <v-textarea
                                    placeholder="Placeholder"
                                    background-color="grey lighten-4"
                                    solo
                                    flat
                                  ></v-textarea>
                                  <v-divider class="my-1"></v-divider>
                                  <v-btn depressed outline round color="indigo" class="heightStyle">
                                    <v-icon>camera_alt</v-icon>
                                  </v-btn>
                                  <v-btn depressed outline round color="indigo" class="heightStyle">
                                    <v-icon class="mr-1">videocam</v-icon>Video
                                  </v-btn>
                                  <v-btn
                                    depressed
                                    dark
                                    color="orange darken-1"
                                    class="heightStyle"
                                  >Veroffentlichen</v-btn>
                                </v-card-text>
                              </v-card>
                            </v-flex>
                            <v-flex xs12></v-flex>
                          </v-layout>
                        </v-container>-->						
                        <div class="blue-grey lighten-5 text-xs-center pa-2 pb-4"
                          v-if="churchesData.newsroom == ''"
                        >
                          <span class="subtitle blue-grey--text">
                            <v-icon large class="blue-grey--text">error_outline</v-icon>
                            <br />Keine News vorhanden.
                            <br />
                        <!--    <span class="title">Neuigkeiten</span>-->
                          </span>
                        </div>
						
						
                        <v-container grid-list-md class="pa-0 tab3rd-edit-panel">
                          <v-layout row wrap>
						  <v-flex xs12 md12>
            <v-layout row wrap>
              <v-flex  d-flex v-for="(newsroom, index) in churchesData.newsroom" v-bind:class="[ index == 0 ? 'md12' : 'md6']">
                <v-card flat color="grey lighten-4">
                 
                    <v-img
					v-if="newsroom.bannerimage"
                      :lazy-src="'http://dev.woobii.com/admin/'+newsroom.bannerimage"
                      :src="'http://dev.woobii.com/admin/'+newsroom.bannerimage"
                      class="newsImage"
					  v-bind:style=" index==0 ? 'height:300px;' : 'height:200px;'"
                    >
                      <v-layout pa-2 column fill-height class="lightbox white--text">
                        <v-flex shrink>
                          <span>
                            <i class="far fa-copyright" style="font-size:9px;margin-left: 10px;margin-top: 10px !important;"></i>
                          </span>
						  <v-btn
                      fab
                      class="editIconBtn"
                      dark
                     color="orange darken-1"
                     @click="newsroomdetail(newsroom.newsid); editDialognewsroom = true"
                     v-if="userRoleCall != 1"
                     >
					<v-icon style="font-size:12px">edit</v-icon>
                     </v-btn>
                        </v-flex>
                      </v-layout>
                    </v-img>
                    <v-img
                      v-else
                      :src="require(`@/assets/woobii-banner.jpg`)"
                      :lazy-src="require(`@/assets/woobii-banner.jpg`)"
					  v-bind:style=" index==0 ? 'height:300px;' : 'height:200px;'"
					 >
					
					 <v-layout pa-2 column fill-height class="lightbox white--text">
                        <v-flex shrink>
                          <span>
                            <i class="far fa-copyright" style="font-size:9px;margin-left: 10px;margin-top: 10px !important;"></i>
                          </span>
						  <v-btn
                      fab
                      class="editIconBtn"
                      dark
                     color="orange darken-1"
                     @click="newsroomdetail(newsroom.newsid); editDialognewsroom = true"
                     v-if="userRoleCall != 1"
                     >
					<v-icon style="font-size:12px">edit</v-icon>
                     </v-btn>
                        </v-flex>
                      </v-layout>
					</v-img>
                  
                  <v-card-title >
                    <h4 class="caption text-uppercase mb-2" style="width:100%;position:relative; bottom:2px;">
                      <span class="font-weight-black teaser-link-info">{{newsroom.category}}</span>
                      <span style="color:#f18825;"> |</span> {{newsroom.subcategory}}
                    </h4>
					<h4 class="caption text-uppercase mb-2 st" style="position: relative;bottom: 10px;">
                     {{newsroom.title}}
                    </h4>
					
                  </v-card-title>
				  
				  

                 
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
                        <v-layout row wrap v-show="churchesData.newsroom">
                        <!--  <v-flex
                            d-flex
                            v-for="(newsroom, index) in churchesData.newsroom"
                            v-bind:class="[ index == 0 ? 'md12' : 'md6']"
                          >
                            <v-card flat color="white" class="grey lighten-4">
                               <v-img
                                v-if="newsroom.bannerimage"
                                :lazy-src="'http://dev.woobii.com/admin/'+newsroom.bannerimage+'?v='+ Math.random()"
                                :src="'http://dev.woobii.com/admin/'+newsroom.bannerimage+'?v='+ Math.random()"
                                gradient="to top right, rgba(0,0,0,0), rgba(0,0,0,.55)"
                              >
                                <v-container fill-height fluid px-2 pt-0 v-if="index == 0">
                                  <v-layout fill-height>
                                    <v-flex xs12 flexbox>
                                      <v-spacer></v-spacer>
                                      <v-layout row wrap shrink>

                                        <v-flex xs6 style="background-color: rgba(255,255,255,.6);">
                                          <h4 class="body-1 my-2 dark--text">
                                            <span class="font-weight-bold">{{ newsroom.category }}</span>
                                            <span class="ml-2" style="color:#fa6e2f;">|</span>
                                            {{ newsroom.subcategory }}
                                          </h4>
                                          <h4 class="body-1 my-2 dark--text">{{ newsroom.title }}</h4>
                                        </v-flex>
                                        <v-flex xs6>
                                          <v-btn
                                            fab
                                            class="right"
                                            dark
                                            color="orange darken-1"
                                            @click="newsroomdetail(newsroom.newsid); editDialog = true"
                                            style="height: 25px;width: 25px;"
                                          >
                                            <v-icon style="font-size:12px">edit</v-icon>
                                          </v-btn>
                                        </v-flex>
                                      </v-layout>
                                    </v-flex>
                                  </v-layout>
                                </v-container>
                              </v-img>-->
                              <!-- <v-card-text v-if="index != 0">
                                <h4 class="body-1 my-2">
                                  <span class="font-weight-bold">{{ newsroom.category }}</span>
                                  <span class="ml-2" style="color:#fa6e2f;">|</span>
                                  {{ newsroom.subcategory }}
                                </h4>
                                <h4 class="body-1 my-2">{{ newsroom.title }}</h4>
                              </v-card-text>
                            </v-card>
                          </v-flex>-->
                          <v-dialog light v-model="editDialognewsroom" max-width="800px">
                            <v-card class="editDialog pa-3">
                              <v-card-title
                                class="subheading"
                              >Bitte Fullen Sie zur Verffentlichung lher News folgende Felder aus:</v-card-title>
                              <v-card-text class="py-0">
                                <v-container grid-list-md class="pa-0">
                                  <v-layout row wrap>
                                    <v-flex xs12>
                                      <v-card
                                        light
                                        color="grey lighten-4"
                                        tile
                                        flat
                                        class="cartBorder"
                                      >
                                        <v-toolbar color="grey lighten-2" light flat height="45px">
                                          <v-toolbar-title>News</v-toolbar-title>
                                          <v-spacer></v-spacer>
                                          <v-btn icon>
                                            <v-icon>expand_more</v-icon>
                                          </v-btn>
                                        </v-toolbar>
                                        <v-card-text>
                                          <v-layout row wrap>
                                            <v-flex xs12 sm3 md3 class="feildLabel">Nachrichtentitel</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-text-field
                                                v-model="formnews.newstitle"
                                                name="newstitle"
                                                required
                                                solo
                                              ></v-text-field>
                                            </v-flex>
                                            <v-flex xs12 sm3 md3 class="feildLabel">Teaser</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-text-field
                                                v-model="formnews.teaser"
                                                name="teaser"
                                                required
                                                solo
                                              ></v-text-field>
                                            </v-flex>
                                            <v-flex xs12 sm3 md3 class="feildLabel">Newsmeldung</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <wysiwyg
                                                v-model="formnews.newsmeldung"
                                                name="newsmeldung"
                                                required
                                              />
                                            </v-flex>
                                          </v-layout>
                                          <v-layout row wrap>
                                            <v-flex xs12 sm3 md3 class="feildLabel">Ressort</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-autocomplete
                                                placeholder="Ressort"
                                                :items="category"
                                                item-text="category"
                                                item-value="id"
                                                v-model="formnews.ressort"
                                                v-on:change="categorySelected"
                                                solo
                                              ></v-autocomplete>
                                              <!-- <v-select :items="scategory" placeholder="Ressort" solo></v-select> -->
                                            </v-flex>
                                            <v-flex xs12 sm3 md3 class="feildLabel">Kategorie</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-autocomplete
                                                placeholder="Kategorie"
                                                :items="subcategory"
                                                item-text="subcategory"
                                                item-value="id"
                                                v-model="formnews.kategorie"
                                                solo
                                              ></v-autocomplete>
                                            </v-flex>
                                            <!--<v-flex xs12 sm3 md3 class="feildLabel">Themen</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-text-field
                                                name="themen"
                                                v-model="formnews.themen"
                                                solo
                                              ></v-text-field>
                                            </v-flex>-->
											<!--<v-flex xs12 sm3 md3 class="feildLabel">Peoples</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-text-field
                                                name="peoples"
                                                v-model="formnews.peoples"
                                                solo
                                              ></v-text-field>
                                            </v-flex>
											<v-flex xs12 sm3 md3 class="feildLabel">Country</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-autocomplete
                                                placeholder="Country"
                                                :items="countryItems"
                                                item-text="name"
                                                item-value="id"
                                                v-model="formnews.country"
                                                v-on:change="listofcity"
                                                solo
                                              ></v-autocomplete>
                                            </v-flex>
											<v-spacer></v-spacer>
											<v-flex xs12 sm3 md3 class="feildLabel">City</v-flex>
                                            <v-flex xs12 sm9 md9>
											<v-autocomplete
                                                placeholder="City"
                                                :items="cityItems"
                                                item-text="place"
                                                item-value="id"
                                                v-model="formnews.city"
                                                v-on:change="listofmuncipalty"
                                                solo
                                              ></v-autocomplete>
											</v-flex>
											<v-flex xs12 sm3 md3 class="feildLabel">Municipalty</v-flex>
                                            <v-flex xs12 sm9 md9>
											 <v-autocomplete
                                                placeholder="Municipalty"
                                                :items="MunicipalityItems"
                                                item-text="type"
                                                item-value="id"
                                                v-model="formnews.municipalty"
                                                solo
                                              ></v-autocomplete>
											</v-flex>-->
											<v-flex xs12 sm3 md3 class="feildLabel">Themen</v-flex>
											<v-flex xs12 sm9 md9>
												<v-combobox
													v-model="formnews.chips"
													:items="themenItems"
													chips
													clearable
													label="Your favorite hobbies"
													multiple
													solo
													max=5
													item-text="Title"
													item-value="ID"
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
											</v-flex>

                                            <v-flex xs12 sm3 md3 class="feildLabel">Veroffentlichung</v-flex>
                                            <v-flex xs12 sm3 md3>
                                              <v-text-field
                                                name="veroffentlichung"
                                                v-model="formnews.veroffentlichung"
                                                solo
                                                readonly
                                                @click="datemodal = true"
                                                append-icon="event"
                                              ></v-text-field>
                                              <v-dialog
                                                ref="dialog"
                                                v-model="datemodal"
                                                :return-value.sync="formnews.veroffentlichung"
                                                persistent
                                                lazy
                                                full-width
                                                width="290px"
                                              >
                                                <v-date-picker
                                                  v-model="formnews.veroffentlichung"
                                                  scrollable
                                                >
                                                  <v-spacer></v-spacer>
                                                  <v-btn
                                                    flat
                                                    color="primary"
                                                    @click="datemodal = false"
                                                  >Cancel</v-btn>
                                                  <v-btn
                                                    flat
                                                    color="primary"
                                                    @click="$refs.dialog.save(formnews.veroffentlichung)"
                                                  >OK</v-btn>
                                                </v-date-picker>
                                              </v-dialog>
                                            </v-flex>
                                            <v-flex
                                              xs12
                                              sm3
                                              md3
                                              class="feildLabel"
                                            >Inaktiv setzen (optional)</v-flex>
                                            <v-flex xs12 sm3 md3>
                                              <v-text-field
                                                name="inaktiv"
                                                v-model="formnews.inaktiv"
                                                solo
                                                readonly
                                                @click="datemodalI = true"
                                                append-icon="event"
                                              ></v-text-field>
                                              <v-dialog
                                                ref="dialogI"
                                                v-model="datemodalI"
                                                :return-value.sync="formnews.inaktiv"
                                                persistent
                                                lazy
                                                full-width
                                                width="290px"
                                              >
                                                <v-date-picker
                                                  v-model="formnews.inaktiv"
                                                  scrollable
                                                >
                                                  <v-spacer></v-spacer>
                                                  <v-btn
                                                    flat
                                                    color="primary"
                                                    @click="datemodalI = false"
                                                  >Cancel</v-btn>
                                                  <v-btn
                                                    flat
                                                    color="primary"
                                                    @click="$refs.dialogI.save(formnews.inaktiv)"
                                                  >OK</v-btn>
                                                </v-date-picker>
                                              </v-dialog>
                                            </v-flex>
                                          </v-layout>
                                        </v-card-text>
                                      </v-card>
                                    </v-flex>
                                    <v-flex d-flex xs12 sm6 md6>
                                      <v-layout row wrap>
                                        <v-flex d-flex xs12>
                                          <v-card
                                            light
                                            color="grey lighten-4"
                                            tile
                                            flat
                                            class="cartBorder"
                                          >
                                            <v-toolbar
                                              color="grey lighten-2"
                                              light
                                              flat
                                              height="45px"
                                            >
                                              <v-toolbar-title>Biddatei hochladen</v-toolbar-title>
                                              <v-spacer></v-spacer>
                                              <v-btn icon>
                                                <v-icon>expand_more</v-icon>
                                              </v-btn>
                                            </v-toolbar>
                                            <v-card-text>
                                              <v-layout row wrap>
                                                <v-flex xs12 sm3 md3 class="feildLabel">Datei</v-flex>
                                                <v-flex xs12 sm9 md9>
                                                  <v-layout row wrap>
                                                    <v-flex xs7 style="padding-right:0px;">
                                                      <v-text-field
                                                        xs8
                                                        sm8
                                                        md8
                                                        v-model="formnews.date"
                                                        solo
                                                      ></v-text-field>
                                                    </v-flex>
                                                    <v-flex xs5 style="padding-left:0px;">
                                                      <v-btn
                                                        small
                                                        block
                                                        depressed
                                                        class="grey lighten-2 btn-detai"
                                                        @click="editBannerDialog = true"
                                                      >Durchsuchen</v-btn>
                                                    </v-flex>
                                                  </v-layout>
                                                  <v-dialog
                                                    light
                                                    v-model="editBannerDialog"
                                                    max-width="810px"
                                                  >
                                                    <v-card class="editDialog pa-3">
                                                      <v-card-title class="subheading pd-1">Banner:</v-card-title>
                                                      <v-card-text class="py-0">
                                                        <v-flex xs12>
                                                          <v-image-input
                                                            v-model="imageBannerData"
                                                            :image-quality="0.85"
                                                            clearable
															minScaling="contain"
                                                            :image-width="600"
                                                            :image-height="278"
                                                            image-format="jpeg"
                                                          />
                                                        </v-flex>
                                                      </v-card-text>
                                                      <v-card-actions>
                                                        <v-btn
                                                          depressed
                                                          class="grey lighten-2"
                                                          @click="imageBannerData = ''; editBannerDialog = false"
                                                        >Abbrechen</v-btn>
                                                        <v-btn
                                                          depressed
                                                          dark
                                                          color="orange darken-1"
                                                          @click="editBannerDialog = false; formnews.bannerimage=imageBannerData"
                                                        >Speichern</v-btn>
                                                      </v-card-actions>
                                                    </v-card>
                                                  </v-dialog>
                                                </v-flex>
                                                <v-flex xs12 sm6 md6></v-flex>
                                              </v-layout>
                                            </v-card-text>
                                          </v-card>
                                        </v-flex>
                                        <v-flex d-flex xs12>
                                          <v-card
                                            light
                                            color="grey lighten-4"
                                            tile
                                            flat
                                            class="cartBorder"
                                          >
                                            <v-toolbar
                                              color="grey lighten-2"
                                              light
                                              flat
                                              height="45px"
                                            >
                                              <v-toolbar-title>Bildinformationen</v-toolbar-title>
                                              <v-spacer></v-spacer>
                                              <v-btn icon>
                                                <v-icon>expand_more</v-icon>
                                              </v-btn>
                                            </v-toolbar>
                                            <v-card-text>
                                              <v-layout row wrap>
                                                <v-flex xs12 sm4 md4 class="feildLabel">Bildtitel</v-flex>
                                                <v-flex xs12 sm8 md8>
                                                  <v-text-field
                                                    name="bildtitel"
                                                    v-model="formnews.bildtitel"
                                                    solo
                                                  ></v-text-field>
                                                </v-flex>
                                                <v-flex xs12 sm4 md4 class="feildLabel">Beschreibung</v-flex>
                                                <v-flex xs12 sm8 md8>
                                                  <v-textarea
                                                    name="beschreibung"
                                                    v-model="formnews.beschreibung"
                                                    solo
                                                  ></v-textarea>
                                                </v-flex>
                                                <v-flex xs12 sm4 md4 class="feildLabel">Bildrechte</v-flex>
                                                <v-flex xs12 sm8 md8>
                                                  <v-text-field
                                                    name="bildrechte"
                                                    v-model="formnews.bildrechte"
                                                    solo
                                                  ></v-text-field>
                                                </v-flex>
												
												 <v-flex xs12 sm4 md4 class="feildLabel">Image Source Label</v-flex>
												  <v-flex xs12 sm8 md8>
												  <v-text-field
                                                    name="bildrechte"
                                                    v-model="formnews.imagesourcelabel"
                                                    solo
                                                  ></v-text-field>
                                                </v-flex>
												 <v-flex xs12 sm4 md4 class="feildLabel">Image Source Link</v-flex>
												 <v-flex xs12 sm8 md8>
												  <v-text-field
                                                    name="bildrechte"
                                                    v-model="formnews.imagesourcelink"
                                                    solo
                                                  ></v-text-field>
                                                </v-flex>
												
												
                                                <v-flex xs12 sm4 md4 class="feildLabel">Lizenz</v-flex>
                                                <v-flex xs12 sm8 md8>
                                                  <v-select
                                                    :items="items"
                                                    v-model="formnews.lizenz"
                                                    solo
                                                  ></v-select>
                                                </v-flex>
                                              </v-layout>
                                            </v-card-text>
                                          </v-card>
                                        </v-flex>
                                      </v-layout>
                                    </v-flex>
                                    <v-flex d-flex xs12 sm6 md6>
                                      <v-layout row wrap>
                                        <v-flex xs12>
                                          <v-card
                                            light
                                            color="grey lighten-4"
                                            tile
                                            flat
                                            class="cartBorder"
                                          >
                                            <v-card-text>
                                              <v-layout row wrap>
                                                <v-flex xs12 sm4 md4>
                                                  <v-img
                                                    src="https://picsum.photos/510/300?random"
                                                    aspect-ratio="1.2"
                                                  ></v-img>
                                                </v-flex>
                                                <v-flex xs12 sm8 md8>
                                                  <ul class="caption">
                                                    <li class="mb-2">Hochgeladen am: 20.08.2014</li>
                                                    <li class="my-2">DateigroBe: 3802,63 KB</li>
                                                    <li class="my-2">MaBe: 2912 x 1353 Pixel</li>
                                                    <li class="mt-2">Dateityp: .jpg</li>
                                                  </ul>
                                                </v-flex>
                                              </v-layout>
                                              <v-divider light class="my-3"></v-divider>
                                              <v-img
                                                v-if="imageBannerData"
                                                :lazy-src="imageBannerData"
                                                :src="imageBannerData"
                                              />
                                              <v-img
                                                v-else-if="formnews.bannerimage"
                                                :lazy-src="'http://dev.woobii.com/admin/'+formnews.bannerimage"
                                                :src="'http://dev.woobii.com/admin/'+formnews.bannerimage"
                                              />
                                              <v-img
                                                v-else
                                                :src="require(`@/assets/woobii-banner.jpg`)"
                                                :lazy-src="require(`@/assets/woobii-banner.jpg`)"
                                              />
                                              <div
                                                class="caption my-3"
                                              >Das Build wird zusatzlich zum Original auch in diesen GroBen zum Download angeboten:</div>
                                              <v-layout
                                                row
                                                wrap
                                                class="caption"
                                                style="border:1px solid #CCC;"
                                              >
                                                <v-flex
                                                  xs6
                                                  style="border:1px solid #CCC;border-top:none;border-left:none;"
                                                >1200 x 557</v-flex>
                                                <v-flex
                                                  xs6
                                                  style="border-bottom:1px solid #CCC;"
                                                >56,5 KB</v-flex>
                                                <v-flex
                                                  xs6
                                                  style="border-right:1px solid #CCC;"
                                                >600 x 278</v-flex>
                                                <v-flex xs6>23.2 KB</v-flex>
                                              </v-layout>
                                            </v-card-text>
                                          </v-card>
                                        </v-flex>
                                      </v-layout>
                                    </v-flex>
									<!--<v-flex d-flex xs12 sm12 md12>
                                      <v-layout row wrap>
                                        <v-flex d-flex xs12>
                                          <v-card
                                            light
                                            color="grey lighten-4"
                                            tile
                                            flat
                                            class="cartBorder"
                                          >
                                            <v-toolbar
                                              color="grey lighten-2"
                                              light
                                              flat
                                              height="45px"
                                            >
                                              <v-toolbar-title>Additional Information</v-toolbar-title>
                                              <v-spacer></v-spacer>
                                              <v-btn icon>
                                                <v-icon>expand_more</v-icon>
                                              </v-btn>
                                            </v-toolbar>
                                              <v-card class="editDialog pa-3">
                            <v-card-title class="subheading pd-1">Upload Images</v-card-title>

							<v-tabs
							  v-model="tab"
							  background-color="transparent"
							  color="basil"
							  grow
							>
							  <v-tab>
								Upload image
							  </v-tab>
							  <v-tab>
								Upload Video
							  </v-tab>
							</v-tabs>

							<v-tabs-items v-model="tab">
							  <v-tab-item>
								<v-card flat color="basil">
									<v-card-text class="py-0">
									  <v-flex xs12>
										<v-image-input
										  v-model="ImageUrls"
										  :image-quality="0.85"
										  clearable
										  image-format="jpeg"
										/>
									  </v-flex>
									</v-card-text>
									<v-card-actions>
									  <v-btn
										depressed
										class="grey lighten-2"
										@click="uploadimagevideos = false"
									  >Cancel</v-btn>
									  <v-btn
										depressed
										dark
										@click="saveImagesHere"
										color="orange darken-1"
									  >Save</v-btn>
								</v-card-actions>
								</v-card>
							  </v-tab-item>
							  <v-tab-item>
								<v-layout xs12>
									<v-card-text xs12 md6>
										<v-text-field
										  slot="activator"
										  v-model="videoForm.title"
										  label="Enter Video Title"
										></v-text-field>
									</v-card-text>
									<v-card-text xs12 md6>
										<v-text-field
										  slot="activator"
										  v-model="videoForm.youtubeparameter"
										  label="eg:- After ?v= text"
										></v-text-field>
									</v-card-text>
								</v-layout>
								<v-card-text>
									<v-text-field
									  slot="activator"
									  v-model="videoForm.videoUrl"
									  label="Enter Video URL"
									></v-text-field>
								</v-card-text>
								<v-card-actions>
								  <v-btn
									depressed
									class="grey lighten-2"
									@click="uploadimagevideos = false"
								  >Cancel</v-btn>
								  <v-btn
									depressed
									dark
									@click="saveVideos"
									color="orange darken-1"
								  >Save</v-btn>
								</v-card-actions>
							  </v-tab-item>
							</v-tabs-items>
                          </v-card>
                                          </v-card>
                                        </v-flex>
                                      </v-layout>
                                    </v-flex>-->
                                  </v-layout>
                                </v-container>
                              </v-card-text>

                              <v-card-actions class="pt-4">
                                <v-container grid-list-md class="pa-0">
                                  <v-layout row wrap>
                                    <v-flex xs12 sm6 md6>
                                      <v-checkbox
                                        v-model="formnews.checkbox"
                                        label="Hauptnews auf GLAUBE.at (kostenpflichtig)"
                                        required
                                      ></v-checkbox>
                                    </v-flex>
                                    <v-flex xs12 sm6 md6>
                                      <v-btn
                                        @click="editDialog = false"
                                        depressed
                                        small
                                        class="grey lighten-2"
                                      >LOSCHEN</v-btn>
                                      <v-btn
                                        @click="saveNewsroom"
                                        depressed
                                        small
                                        class="grey lighten-2"
                                      >SPEICHERN</v-btn>
                                      <v-btn
                                        depressed
                                        small
                                        dark
                                        @click="saveNewsroom"
                                        color="orange darken-1"
                                      >VEROFFENTLICHUN</v-btn>
                                    </v-flex>
                                  </v-layout>
                                </v-container>
                              </v-card-actions>
                            </v-card>
                          </v-dialog>
                          <!-- <v-flex d-flex xs12 md6>
                            <v-card flat color="white" class="grey lighten-4">
                              <v-img
                              v-if="newsroom.bannerimage"
                              :lazy-src="'http://dev.woobii.com/admin/'+newsroom.bannerimage"
                              :src="'http://dev.woobii.com/admin/'+newsroom.bannerimage"
                            />
                              <v-card-text>
                                <h4 class="body-1 my-2">
                                  <span class="font-weight-bold">{{ newsroom.category }}</span>
                                  <span class="ml-2" style="color:#fa6e2f;">|</span> {{ newsroom.subcategory }}
                                </h4>
                                <h4
                                  class="body-1 my-2"
                                >{{ newsroom.title }}</h4>
                              </v-card-text>
                            </v-card>
                          </v-flex>-->
                          <!-- <v-flex d-flex xs12 md6>
                            <v-card flat color="white" class="grey lighten-4">
                              <v-img src="https://cdn.vuetifyjs.com/images/cards/desert.jpg"></v-img>
                              <v-card-text>
                                <h4 class="body-1 my-2">
                                  <span class="font-weight-bold">Ausland</span>
                                  <span class="ml-2" style="color:#fa6e2f;">|</span> Israel
                                </h4>
                                <h4
                                  class="body-1 my-2"
                                >70 Jahre Staat Israel: Evangelischer Oberkichenrat gratuliert</h4>
                              </v-card-text>
                            </v-card>
                          </v-flex>-->
                        </v-layout>
                      </v-flex>
                    </v-layout>
                  </v-card-text>
                </v-card>
              </v-tab-item>
			  
			    <v-tab-item id="tab-4">
                <v-card flat>
                  <v-card-text>
                    <v-layout row wrap>
                      <v-flex xs12>
                        <p class="headline">Impulse
						<v-btn
							  fab
							  small
							  dark
							  class="right"
							  @click="addnewsroomformadvance(); editnewaudiovedio = true"
							  style="height: 30px; padding: 0px; width: 30px;"
							  v-if="userRoleCall != 1"
							>
							<v-icon style="font-size:20px">add</v-icon>
                        </v-btn></p>
						
                       <!-- <v-container grid-list-md class="pa-0 tab3rd-edit-panel">
                          <v-layout row wrap>
                            <v-flex xs12>
                              <v-card light color="grey lighten-4">
                                <v-card-text>
                                  <v-textarea
                                    placeholder="Placeholder"
                                    background-color="grey lighten-4"
                                    solo
                                    flat
                                  ></v-textarea>
                                  <v-divider class="my-1"></v-divider>
                                  <v-btn depressed outline round color="indigo" class="heightStyle">
                                    <v-icon>camera_alt</v-icon>
                                  </v-btn>
                                  <v-btn depressed outline round color="indigo" class="heightStyle">
                                    <v-icon class="mr-1">videocam</v-icon>Video
                                  </v-btn>
                                  <v-btn
                                    depressed
                                    dark
                                    color="orange darken-1"
                                    class="heightStyle"
                                  >Veroffentlichen</v-btn>
                                </v-card-text>
                              </v-card>
                            </v-flex>
                            <v-flex xs12></v-flex>
                          </v-layout>
                        </v-container>-->						
                        <div class="blue-grey lighten-5 text-xs-center pa-2 pb-4"
                          v-if="churchesData.newsroomadvance == ''"
                        >
                          <span class="subtitle blue-grey--text">
                            <v-icon large class="blue-grey--text">error_outline</v-icon>
                            <br />Keine Impulse vorhanden.
                            <br />
                        <!--    <span class="title">Impulse</span>-->
                          </span>
                        </div>
                        <v-container grid-list-md class="pa-0 tab3rd-edit-panel">
                          <v-layout row wrap>
						  <v-flex xs12 md12>
            <v-layout row wrap>
              <v-flex  d-flex v-for="(newsroom, index) in churchesData.newsroomadvance" v-bind:class="[ index == 0 ? 'md12' : 'md6']">
                <v-card flat color="grey lighten-4">
                 
                    <v-img
					v-if="newsroom.bannerimage"
                      :lazy-src="'http://dev.woobii.com/admin/'+newsroom.bannerimage"
                      :src="'http://dev.woobii.com/admin/'+newsroom.bannerimage"
                      class="newsImage"
					  v-bind:style=" index==0 ? 'height:300px;' : 'height:200px;'"
                    >
                      <v-layout pa-2 column fill-height class="lightbox white--text">
                        <v-flex shrink>
                          <span>
                            <i class="far fa-copyright" style="font-size:9px;margin-left: 10px;margin-top: 10px !important;"></i>
                          </span>
						  <v-btn
                      fab
                      class="editIconBtn"
                      dark
                     color="orange darken-1"
                     @click="newsroomdetailadvance(newsroom.newsid); editnewaudiovedio = true"
                     v-if="userRoleCall != 1"
                     >
					<v-icon style="font-size:12px">edit</v-icon>
                     </v-btn>
                        </v-flex>
                      </v-layout>
                    </v-img>
                    <v-img
                      v-else
                      :src="require(`@/assets/woobii-banner.jpg`)"
                      :lazy-src="require(`@/assets/woobii-banner.jpg`)"
					  v-bind:style=" index==0 ? 'height:300px;' : 'height:200px;'"
					 >
					
					 <v-layout pa-2 column fill-height class="lightbox white--text">
                        <v-flex shrink>
                          <span>
                            <i class="far fa-copyright" style="font-size:9px;margin-left: 10px;margin-top: 10px !important;"></i>
                          </span>
						  <v-btn
                      fab
                      class="editIconBtn"
                      dark
                     color="orange darken-1"
                     @click="newsroomdetail(newsroom.newsid); editDialognewsroom = true"
                     v-if="userRoleCall != 1"
                     >
					<v-icon style="font-size:12px">edit</v-icon>
                     </v-btn>
                        </v-flex>
                      </v-layout>
					</v-img>
                  
                  <v-card-title >
                    <h4 class="caption text-uppercase mb-2" style="width:100%;position:relative; bottom:2px;">
                      <span class="font-weight-black teaser-link-info">{{newsroom.category}}</span>
                      <span style="color:#f18825;"> |</span> {{newsroom.subcategory}}
                    </h4>
					<h4 class="caption text-uppercase mb-2 st" style="position: relative;bottom: 10px;">
                     {{newsroom.title}}
                    </h4>
					
                  </v-card-title>
				  
				  

                 
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
						
                        <v-layout row wrap v-show="churchesData.newsroom">
                           <!--  <v-flex
                            d-flex
                            v-for="(newsroom, index) in churchesData.newsroom"
                            v-bind:class="[ index == 0 ? 'md12' : 'md6']"
                          >
                            <v-card flat color="white" class="grey lighten-4">
                            <v-img
                                v-if="newsroom.bannerimage"
                                :lazy-src="'http://dev.woobii.com/admin/'+newsroom.bannerimage+'?v='+ Math.random()"
                                :src="'http://dev.woobii.com/admin/'+newsroom.bannerimage+'?v='+ Math.random()"
                                gradient="to top right, rgba(0,0,0,0), rgba(0,0,0,.55)"
                              >
                                <v-container fill-height fluid px-2 pt-0 v-if="index == 0">
                                  <v-layout fill-height>
                                    <v-flex xs12 flexbox>
                                      <v-spacer></v-spacer>
                                      <v-layout row wrap shrink>

                                        <v-flex xs6 style="background-color: rgba(255,255,255,.6);">
                                          <h4 class="body-1 my-2 dark--text">
                                            <span class="font-weight-bold">{{ newsroom.category }}</span>
                                            <span class="ml-2" style="color:#fa6e2f;">|</span>
                                            {{ newsroom.subcategory }}
                                          </h4>
                                          <h4 class="body-1 my-2 dark--text">{{ newsroom.title }}</h4>
                                        </v-flex>
                                        <v-flex xs6>
                                          <v-btn
                                            fab
                                            class="right"
                                            dark
                                            color="orange darken-1"
                                            @click="newsroomdetail(newsroom.newsid); editDialog = true"
                                            style="height: 25px;width: 25px;"
                                          >
                                            <v-icon style="font-size:12px">edit</v-icon>
                                          </v-btn>
                                        </v-flex>
                                      </v-layout>
                                    </v-flex>
                                  </v-layout>
                                </v-container>
                              </v-img>-->
                              <!-- <v-card-text v-if="index != 0">
                                <h4 class="body-1 my-2">
                                  <span class="font-weight-bold">{{ newsroom.category }}</span>
                                  <span class="ml-2" style="color:#fa6e2f;">|</span>
                                  {{ newsroom.subcategory }}
                                </h4>
                                <h4 class="body-1 my-2">{{ newsroom.title }}</h4>
                              </v-card-text>
                            </v-card>
                          </v-flex>-->
                          <v-dialog light v-model="editnewaudiovedio" max-width="800px">
                            <v-card class="editDialog pa-3">
                              <v-card-title
                                class="subheading"
                              >Bitte Fullen Sie zur Verffentlichung lher News folgende Felder aus:</v-card-title>
							  
							  <v-card-text class="py-0">
                                <v-container grid-list-md class="pa-0">
                                  <v-layout row wrap>
                                    <v-flex xs12>
                                      <v-card
                                        light
                                        color="grey lighten-4"
                                        tile
                                        flat
                                        class="cartBorder"
                                      >
                                        <v-toolbar color="grey lighten-2" light flat height="45px">
                                          <v-toolbar-title>News</v-toolbar-title>
                                          <v-spacer></v-spacer>
                                          <v-btn icon>
                                            <v-icon>expand_more</v-icon>
                                          </v-btn>
                                        </v-toolbar>
                                        <v-card-text>
                                          <v-layout row wrap>
                                            <v-flex xs12 sm3 md3 class="feildLabel">Newstitle</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-text-field
                                                v-model="formnewsadvance.newstitle"
                                                name="newstitle"
                                                required
                                                solo
                                              ></v-text-field>
                                            </v-flex>
                                            <v-flex xs12 sm3 md3 class="feildLabel">Teaser</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-text-field
                                                v-model="formnewsadvance.teaser"
                                                name="teaser"
                                                required
                                                solo
                                              ></v-text-field>
                                            </v-flex>
                                            <v-flex xs12 sm3 md3 class="feildLabel">Newsmeldung</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <wysiwyg
                                                v-model="formnewsadvance.newsmeldung"
                                                name="newsmeldung"
                                                required
                                              />
                                            </v-flex>
                                          </v-layout>
                                          <v-layout row wrap>
                                            <v-flex xs12 sm3 md3 class="feildLabel">Ressort</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-autocomplete
                                                placeholder="Ressort"
                                                :items="category"
                                                item-text="category"
                                                item-value="id"
                                                v-model="formnewsadvance.ressort"
                                                v-on:change="categorySelected"
                                                solo
                                              ></v-autocomplete>
                                              <!-- <v-select :items="scategory" placeholder="Ressort" solo></v-select> -->
                                            </v-flex>
                                            <v-flex xs12 sm3 md3 class="feildLabel">Kategorie</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-autocomplete
                                                placeholder="Kategorie"
                                                :items="subcategory"
                                                item-text="subcategory"
                                                item-value="id"
                                                v-model="formnewsadvance.kategorie"
                                                solo
                                              ></v-autocomplete>
                                            </v-flex>
											<v-flex xs12 sm3 md3 class="feildLabel">Themen</v-flex>
											<v-flex xs12 sm9 md9>
												<v-combobox
													v-model="formnewsadvance.chips"
													:items="themenItems"
													chips
													clearable
													label="Your favorite hobbies"
													multiple
													solo
													max=5
													item-text="Title"
													item-value="ID"
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
											</v-flex>

                                            <v-flex xs12 sm3 md3 class="feildLabel">Veroffentlichung</v-flex>
                                            <v-flex xs12 sm3 md3>
                                              <v-text-field
                                                name="veroffentlichung"
                                                v-model="formnewsadvance.veroffentlichung"
                                                solo
                                                readonly
                                                @click="datemodal = true"
                                                append-icon="event"
                                              ></v-text-field>
                                              <v-dialog
                                                ref="dialog"
                                                v-model="datemodal"
                                                :return-value.sync="formnewsadvance.veroffentlichung"
                                                persistent
                                                lazy
                                                full-width
                                                width="290px"
                                              >
                                                <v-date-picker
                                                  v-model="formnewsadvance.veroffentlichung"
                                                  scrollable
                                                >
                                                  <v-spacer></v-spacer>
                                                  <v-btn
                                                    flat
                                                    color="primary"
                                                    @click="datemodal = false"
                                                  >Cancel</v-btn>
                                                  <v-btn
                                                    flat
                                                    color="primary"
                                                    @click="$refs.dialog.save(formnewsadvance.veroffentlichung)"
                                                  >OK</v-btn>
                                                </v-date-picker>
                                              </v-dialog>
                                            </v-flex>
                                            <v-flex
                                              xs12
                                              sm3
                                              md3
                                              class="feildLabel"
                                            >Inaktiv setzen (optional)</v-flex>
                                            <v-flex xs12 sm3 md3>
                                              <v-text-field
                                                name="inaktiv"
                                                v-model="formnewsadvance.inaktiv"
                                                solo
                                                readonly
                                                @click="datemodalI = true"
                                                append-icon="event"
                                              ></v-text-field>
                                              <v-dialog
                                                ref="dialogI"
                                                v-model="datemodalI"
                                                :return-value.sync="formnewsadvance.inaktiv"
                                                persistent
                                                lazy
                                                full-width
                                                width="290px"
                                              >
                                                <v-date-picker
                                                  v-model="formnewsadvance.inaktiv"
                                                  scrollable
                                                >
                                                  <v-spacer></v-spacer>
                                                  <v-btn
                                                    flat
                                                    color="primary"
                                                    @click="datemodalI = false"
                                                  >Cancel</v-btn>
                                                  <v-btn
                                                    flat
                                                    color="primary"
                                                    @click="$refs.dialogI.save(formnewsadvance.inaktiv)"
                                                  >OK</v-btn>
                                                </v-date-picker>
                                              </v-dialog>
                                            </v-flex>
                                          </v-layout>
                                        </v-card-text>
                                      </v-card>
                                    </v-flex>
                                    <v-flex d-flex xs12 sm6 md6>
                                      <v-layout row wrap>
                                        <v-flex d-flex xs12>
                                          <v-card
                                            light
                                            color="grey lighten-4"
                                            tile
                                            flat
                                            class="cartBorder"
                                          >
                                            <v-toolbar
                                              color="grey lighten-2"
                                              light
                                              flat
                                              height="45px"
                                            >
                                              <v-toolbar-title>Biddatei hochladen</v-toolbar-title>
                                              <v-spacer></v-spacer>
                                              <v-btn icon>
                                                <v-icon>expand_more</v-icon>
                                              </v-btn>
                                            </v-toolbar>
                                            <v-card-text>
                                              <v-layout row wrap>
                                                <v-flex xs12 sm3 md3 class="feildLabel">Datei</v-flex>
                                                <v-flex xs12 sm9 md9>
                                                  <v-layout row wrap>
                                                    <v-flex xs7 style="padding-right:0px;">
                                                      <v-text-field
                                                        xs8
                                                        sm8
                                                        md8
                                                        v-model="formnewsadvance.date"
                                                        solo
                                                      ></v-text-field>
                                                    </v-flex>
                                                    <v-flex xs5 style="padding-left:0px;">
                                                      <v-btn
                                                        small
                                                        block
                                                        depressed
                                                        class="grey lighten-2 btn-detai"
                                                        @click="editBannerDialogadvance = true"
                                                      >Durchsuchen</v-btn>
                                                    </v-flex>
                                                  </v-layout>
                                                  <v-dialog
                                                    light
                                                    v-model="editBannerDialogadvance"
                                                    max-width="810px"
                                                  >
                                                    <v-card class="editDialog pa-3">
                                                      <v-card-title class="subheading pd-1">Banner:</v-card-title>
                                                      <v-card-text class="py-0">
                                                        <v-flex xs12>
                                                          <v-image-input
                                                            v-model="imageBannerDataadvance"
                                                            :image-quality="0.85"
                                                            clearable
															minScaling="contain"
                                                            :image-width="600"
                                                            :image-height="278"
                                                            image-format="jpeg"
                                                          />
                                                        </v-flex>
                                                      </v-card-text>
                                                      <v-card-actions>
                                                        <v-btn
                                                          depressed
                                                          class="grey lighten-2"
                                                          @click="imageBannerDataadvance = ''; editBannerDialogadvance = false"
                                                        >Abbrechen</v-btn>
                                                        <v-btn
                                                          depressed
                                                          dark
                                                          color="orange darken-1"
                                                          @click="editBannerDialogadvance = false; formnewsadvance.bannerimage=imageBannerDataadvance"
                                                        >Speichern</v-btn>
                                                      </v-card-actions>
                                                    </v-card>
                                                  </v-dialog>
                                                </v-flex>
                                                <v-flex xs12 sm6 md6></v-flex>
                                              </v-layout>
                                            </v-card-text>
                                          </v-card>
                                        </v-flex>
                                        <v-flex d-flex xs12>
                                          <v-card
                                            light
                                            color="grey lighten-4"
                                            tile
                                            flat
                                            class="cartBorder"
                                          >
                                            <v-toolbar
                                              color="grey lighten-2"
                                              light
                                              flat
                                              height="45px"
                                            >
                                              <v-toolbar-title>Bildinformationen</v-toolbar-title>
                                              <v-spacer></v-spacer>
                                              <v-btn icon>
                                                <v-icon>expand_more</v-icon>
                                              </v-btn>
                                            </v-toolbar>
                                            <v-card-text>
                                              <v-layout row wrap>
                                                <v-flex xs12 sm4 md4 class="feildLabel">Bildtitel</v-flex>
                                                <v-flex xs12 sm8 md8>
                                                  <v-text-field
                                                    name="bildtitel"
                                                    v-model="formnewsadvance.bildtitel"
                                                    solo
                                                  ></v-text-field>
                                                </v-flex>
                                                <v-flex xs12 sm4 md4 class="feildLabel">Beschreibung</v-flex>
                                                <v-flex xs12 sm8 md8>
                                                  <v-textarea
                                                    name="beschreibung"
                                                    v-model="formnewsadvance.beschreibung"
                                                    solo
                                                  ></v-textarea>
                                                </v-flex>
                                                <v-flex xs12 sm4 md4 class="feildLabel">Bildrechte</v-flex>
                                                <v-flex xs12 sm8 md8>
                                                  <v-text-field
                                                    name="bildrechte"
                                                    v-model="formnewsadvance.bildrechte"
                                                    solo
                                                  ></v-text-field>
                                                </v-flex>
												<v-flex xs12 sm4 md4 class="feildLabel">Image Source Label</v-flex>
												  <v-flex xs12 sm8 md8>
												  <v-text-field
                                                    name="bildrechte"
                                                    v-model="formnewsadvance.imagesourcelabel"
                                                    solo
                                                  ></v-text-field>
                                                </v-flex>
												 <v-flex xs12 sm4 md4 class="feildLabel">Image Source Link</v-flex>
												 <v-flex xs12 sm8 md8>
												  <v-text-field
                                                    name="bildrechte"
                                                    v-model="formnewsadvance.imagesourcelink"
                                                    solo
                                                  ></v-text-field>
                                                </v-flex>
												
                                                <v-flex xs12 sm4 md4 class="feildLabel">Lizenz</v-flex>
                                                <v-flex xs12 sm8 md8>
                                                  <v-select
                                                    :items="items"
                                                    v-model="formnewsadvance.lizenz"
                                                    solo
                                                  ></v-select>
                                                </v-flex>
                                              </v-layout>
                                            </v-card-text>
                                          </v-card>
                                        </v-flex>
                                      </v-layout>
                                    </v-flex>
                                    <v-flex d-flex xs12 sm6 md6>
                                      <v-layout row wrap>
                                        <v-flex xs12>
                                          <v-card
                                            light
                                            color="grey lighten-4"
                                            tile
                                            flat
                                            class="cartBorder"
                                          >
                                            <v-card-text>
                                              <v-layout row wrap>
                                                <v-flex xs12 sm4 md4>
                                                  <v-img
                                                    src="https://picsum.photos/510/300?random"
                                                    aspect-ratio="1.2"
                                                  ></v-img>
                                                </v-flex>
                                                <v-flex xs12 sm8 md8>
                                                  <ul class="caption">
                                                    <li class="mb-2">Hochgeladen am: 20.08.2014</li>
                                                    <li class="my-2">DateigroBe: 3802,63 KB</li>
                                                    <li class="my-2">MaBe: 2912 x 1353 Pixel</li>
                                                    <li class="mt-2">Dateityp: .jpg</li>
                                                  </ul>
                                                </v-flex>
                                              </v-layout>
                                              <v-divider light class="my-3"></v-divider>
                                              <v-img
                                                v-if="imageBannerDataadvance"
                                                :lazy-src="imageBannerDataadvance"
                                                :src="imageBannerDataadvance"
                                              />
                                              <v-img
                                                v-else-if="formnewsadvance.bannerimage"
                                                :lazy-src="'http://dev.woobii.com/admin/'+formnewsadvance.bannerimage"
                                                :src="'http://dev.woobii.com/admin/'+formnewsadvance.bannerimage"
                                              />
                                              <v-img
                                                v-else
                                                :src="require(`@/assets/woobii-banner.jpg`)"
                                                :lazy-src="require(`@/assets/woobii-banner.jpg`)"
                                              />
                                              <div
                                                class="caption my-3"
                                              >Das Build wird zusatzlich zum Original auch in diesen GroBen zum Download angeboten:</div>
                                              <v-layout
                                                row
                                                wrap
                                                class="caption"
                                                style="border:1px solid #CCC;"
                                              >
                                                <v-flex
                                                  xs6
                                                  style="border:1px solid #CCC;border-top:none;border-left:none;"
                                                >1200 x 557</v-flex>
                                                <v-flex
                                                  xs6
                                                  style="border-bottom:1px solid #CCC;"
                                                >56,5 KB</v-flex>
                                                <v-flex
                                                  xs6
                                                  style="border-right:1px solid #CCC;"
                                                >600 x 278</v-flex>
                                                <v-flex xs6>23.2 KB</v-flex>
                                              </v-layout>
                                            </v-card-text>
                                          </v-card>
                                        </v-flex>
                                      </v-layout>
                                    </v-flex>
									 <v-flex d-flex xs12 sm12 md12>
                                      <v-layout row wrap>
                                        <v-flex d-flex xs12>
                                          <v-card
                                            light
                                            color="grey lighten-4"
                                            tile
                                            flat
                                            class="cartBorder"
                                          >
                                            <v-toolbar
                                              color="grey lighten-2"
                                              light
                                              flat
                                              height="45px"
                                            >
                                              <v-toolbar-title>Audio Vedio Section</v-toolbar-title>
                                              <v-spacer></v-spacer>
                                              <v-btn icon>
                                                <v-icon>expand_more</v-icon>
                                              </v-btn>
                                            </v-toolbar>
												<v-card class="editDialog pa-3">
													<v-card-title class="subheading pd-1">Upload Audio/Vedio</v-card-title>
														<v-card flat color="basil">
															<v-card-text class="py-0 audiovideosection">
																<v-flex xs6 style="margin-left: 9px;">
																	  <v-text-field
																		label="Choose File..."
																		@click="pickFile"
																		v-model="formnewsadvance.audiovedioupload"
																		prepend-icon="attach_file"
																	  ></v-text-field>
																	  <input
																		type="file"
																		style="display: none;"
																		ref="image"
																		accept="*"
																		@change="onFilePicked"
																	  >
																	  
																<v-checkbox
																	v-model="formnewsadvance.audiocheckbox"
																	label="Is Audio"
																	true-value="true"
																	false-value="false"
																></v-checkbox>
																
																<v-checkbox
																	v-model="formnewsadvance.vediocheckbox"
																	label="Is Vedio"
																	true-value="true"
																	false-value="false"
																></v-checkbox>
																</v-flex>
															</v-card-text>
														</v-card>
												  </v-card>
                                          </v-card>
                                        </v-flex>
										 
                                      </v-layout>
                                    </v-flex>
                                  </v-layout>
                                </v-container>
                              </v-card-text>
							
                              <v-card-actions class="pt-4">
                                <v-container grid-list-md class="pa-0">
                                  <v-layout row wrap>
                                    <v-flex xs12 sm6 md6>
                                      <v-checkbox
                                        v-model="formnews.checkbox"
                                        label="Hauptnews auf GLAUBE.at (kostenpflichtig)"
                                        required
                                      ></v-checkbox>
                                    </v-flex>
                                    <v-flex xs12 sm6 md6>
                                      <v-btn
                                        @click="editDialog = false"
                                        depressed
                                        small
                                        class="grey lighten-2"
                                      >LOSCHEN</v-btn>
                                      <v-btn
                                        @click="saveNewsroomadvance"
                                        depressed
                                        small
                                        class="grey lighten-2"
                                      >SPEICHERN</v-btn>
                                      <v-btn
                                        depressed
                                        small
                                        dark
                                        @click="saveNewsroomadvance"
                                        color="orange darken-1"
                                      >VEROFFENTLICHUN</v-btn>
                                    </v-flex>
                                  </v-layout>
                                </v-container>
                              </v-card-actions>
                            </v-card>
                          </v-dialog>
                          <!-- <v-flex d-flex xs12 md6>
                            <v-card flat color="white" class="grey lighten-4">
                              <v-img
                              v-if="newsroom.bannerimage"
                              :lazy-src="'http://dev.woobii.com/admin/'+newsroom.bannerimage"
                              :src="'http://dev.woobii.com/admin/'+newsroom.bannerimage"
                            />
                              <v-card-text>
                                <h4 class="body-1 my-2">
                                  <span class="font-weight-bold">{{ newsroom.category }}</span>
                                  <span class="ml-2" style="color:#fa6e2f;">|</span> {{ newsroom.subcategory }}
                                </h4>
                                <h4
                                  class="body-1 my-2"
                                >{{ newsroom.title }}</h4>
                              </v-card-text>
                            </v-card>
                          </v-flex>-->
                          <!-- <v-flex d-flex xs12 md6>
                            <v-card flat color="white" class="grey lighten-4">
                              <v-img src="https://cdn.vuetifyjs.com/images/cards/desert.jpg"></v-img>
                              <v-card-text>
                                <h4 class="body-1 my-2">
                                  <span class="font-weight-bold">Ausland</span>
                                  <span class="ml-2" style="color:#fa6e2f;">|</span> Israel
                                </h4>
                                <h4
                                  class="body-1 my-2"
                                >70 Jahre Staat Israel: Evangelischer Oberkichenrat gratuliert</h4>
                              </v-card-text>
                            </v-card>
                          </v-flex>-->
                        </v-layout>
                      </v-flex>
                    </v-layout>
                  </v-card-text>
                </v-card>
              </v-tab-item>
              <v-tab-item id="tab-5">
                <v-card flat>
                  <v-card-text>	
					<p class="headline"> sozialen Medien 
					<v-btn
							fab
							small
							class="right"
							dark
							@click="socialmediasFormDialog = true;"
							style="height: 30px; padding: 0px; width: 30px;"
							v-if="userRoleCall != 1  && packageConditions.IsintigrationofSocialMedia"
						  >
							<v-icon style="font-size:20px">edit</v-icon>
					  </v-btn></p>
					  <v-dialog light v-model="socialmediasFormDialog" max-width="650px">
						 <v-card class="editDialog pa-3">
                              <v-card-text class="py-0">
                                <v-container grid-list-md class="pa-0">
                                  <v-layout row wrap>
								 
                                    <v-flex xs12>
                                      <v-card
                                        light
                                        color="grey lighten-4"
                                        tile
                                        flat
                                        class="cartBorder"
                                      >
									  <v-card-title>Bitte tragen Sie folgend Ihre Social Media IDs ein, damit Ihre Social-Media-Beiträge auf Ihrer Seite angezeigt werden können</v-card-title>
                                       
                                        <v-card-text>
                                          <v-layout row wrap>
                                            <v-flex xs12 sm3 md3 class="feildLabel">Facebook ID 
											<v-tooltip right>
												<h4 class="body-1">Steps : <br> How to get FacebookID.</h4>
												<v-icon style="font-size:18px" slot="activator" class="imgDeal">info</v-icon>
											</v-tooltip>								
										</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-text-field
                                                v-model="formSocailIds.facebookID"
                                                name="facebookID"
                                                required
                                                solo
                                              ></v-text-field>
                                            </v-flex>
                                            <v-flex xs12 sm3 md3 class="feildLabel">Facebook Token 
											<v-tooltip right>
												<h4 class="body-1">Steps : <br> How to get Facebook Token.</h4>
												<v-icon style="font-size:18px" slot="activator" class="imgDeal">info</v-icon>
											</v-tooltip>								
											</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-text-field
                                                v-model="formSocailIds.facebookToken"
                                                name="facebookToken"
                                                required
                                                solo
                                              ></v-text-field>
                                            </v-flex>
                                            <v-flex xs12 sm3 md3 class="feildLabel">Instagram ID
											<v-tooltip right>
												<h4 class="body-1">Steps : <br> How to get Instagram ID.</h4>
												<v-icon style="font-size:18px" slot="activator" class="imgDeal">info</v-icon>
											</v-tooltip>
											</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-text-field
                                                v-model="formSocailIds.instagramID"
                                                name="instagramID"
                                                required
                                                solo
                                              ></v-text-field>
                                            </v-flex>   
                                            <v-flex xs12 sm3 md3 class="feildLabel">Piterest ID
											<v-tooltip right>
												<h4 class="body-1">Steps : <br> How to get Piterest ID.</h4>
												<v-icon style="font-size:18px" slot="activator" class="imgDeal">info</v-icon>
											</v-tooltip>
											</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-text-field
                                                v-model="formSocailIds.pinterestID"
                                                name="piterestID"
                                                required
                                                solo
                                              ></v-text-field>
                                            </v-flex>      
                                            <v-flex xs12 sm3 md3 class="feildLabel">Youtube ID
											<v-tooltip right>
												<h4 class="body-1">Steps : <br> How to get Youtube ID.</h4>
												<v-icon style="font-size:18px" slot="activator" class="imgDeal">info</v-icon>
											</v-tooltip>
											</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-text-field
                                                v-model="formSocailIds.youtubeID"
                                                name="youtubeID"
                                                required
                                                solo
                                              ></v-text-field>
                                            </v-flex>      
                                            <v-flex xs12 sm3 md3 class="feildLabel">Twitter ID
											<v-tooltip right>
												<h4 class="body-1">Steps : <br> How to get Twitter ID.</h4>
												<v-icon style="font-size:18px" slot="activator" class="imgDeal">info</v-icon>
											</v-tooltip>
											</v-flex>
                                            <v-flex xs12 sm9 md9>
                                              <v-text-field
                                                v-model="formSocailIds.twitterID"
                                                name="twitterID"
                                                required
                                                solo
                                              ></v-text-field>
                                            </v-flex>                                            
                                          </v-layout>                                        
                                        </v-card-text>
                                      </v-card>
                                    </v-flex>                                    
                                  </v-layout>
								   <v-layout row wrap>                                   
                                    <v-flex xs12 sm6 md6>
                                      <v-btn
                                        @click="socialmediasFormDialog = false"
                                        depressed
                                        small
                                        class="grey lighten-2"
                                      >LOSCHEN</v-btn>
                                      <v-btn
                                        @click="UpdateSocialIDs"
                                        depressed
                                        small
                                        class="grey lighten-2"
                                      >SPEICHERN</v-btn>                                     
                                    </v-flex>
                                  </v-layout>
                                </v-container>
                              </v-card-text>
                              
                            </v-card>	
					  </v-dialog>
					  
                    <v-layout row wrap>
					
                      <v-flex xs12 md12>					 
						<v-card>
							<v-tabs centered background-color="white" fixed-tabs>
							 <v-tab>
								<v-icon small class="mr-2" background="gray" color="black">share</v-icon>
							  </v-tab>
							  <v-tab>
								<v-icon small class="mr-2" background="gray" color="black">fab fa-instagram</v-icon>
							  </v-tab>
							  <v-tab>
								<v-icon small class="mr-2">fab fa-twitter</v-icon>
							  </v-tab>
							  <v-tab>
							    <v-icon small class="mr-2">fab fa-facebook-f</v-icon>
							  </v-tab>
							  <v-tab>
							    <v-icon small class="mr-2">fab fa-youtube</v-icon>
							  </v-tab>
							  <v-tab-item>
								<v-card flat>
								 <v-card-text>
										 <v-card-text>
								   <vue-instagram token="18898066541.1677ed0.4420aac276eb437a95740340ad2a6a12" class="layout row wrap" count="100">
										<v-flex md6 xs12 slot="feeds" slot-scope="props">
											<v-img
											  :lazy-src="props.feed.images.low_resolution.url"
											  :src="props.feed.images.low_resolution.url"
											  class="newsImage"
											>
											</v-img>
											<v-layout row wrap>
											<v-flex md2 xs12>
											<v-img
											  :lazy-src="props.feed.user.profile_picture"
											  :src="props.feed.user.profile_picture"
											  style="border-radius:50%;"
											  height="45" width="45"
											>
											</v-img>
											</v-flex>
											<v-flex md8 xs12>
											<p style="margin-bottom:4px;">{{props.feed.user.full_name}}</p>
											<p style="margin-bottom:4px;color:#9dc5d8">{{props.feed.user.username}}</p>
											</v-flex>
											<v-flex md2 xs12>
											<v-icon large class="mr-2" color="red">fab fa-instagram</v-icon>
											</v-flex>
											</v-layout>
											<p style="margin-bottom:6px;"><v-icon small class="mr-2">fa fa-thumbs-up</v-icon> {{props.feed.likes.count}} likes</p>
											<p style="margin-bottom:6px;"><v-icon small class="mr-2">fa fa-comments</v-icon>  {{props.feed.comments.count}} comments</p>
											<p v-if="props.feed.caption">{{ props.feed.caption.text }}</p>
											<p><a :href="props.feed.link" target="new">click here</a></p>
											</v-flex>
										</vue-instagram>
								  </v-card-text>
									<Timeline id="developer1job" source-type="profile" widget-class="mt-3 my-custom-class"/></Timeline>
								  </v-card-text>

								</v-card>
							  </v-tab-item>
							  <v-tab-item>
								<v-card flat>
								  <v-card-text>
								   <vue-instagram token="18898066541.1677ed0.4420aac276eb437a95740340ad2a6a12" class="layout row wrap" count="100">
										<v-flex md6 xs12 slot="feeds" slot-scope="props">
											<v-img
											  :lazy-src="props.feed.images.low_resolution.url"
											  :src="props.feed.images.low_resolution.url"
											  class="newsImage"
											>
											</v-img>
											<v-layout row wrap>
											<v-flex md2 xs12>
											<v-img
											  :lazy-src="props.feed.user.profile_picture"
											  :src="props.feed.user.profile_picture"
											  style="border-radius:50%;"
											  height="45" width="45"
											>
											</v-img>
											</v-flex>
											<v-flex md8 xs12>
											<p style="margin-bottom:4px;">{{props.feed.user.full_name}}</p>
											<p style="margin-bottom:4px;color:#9dc5d8">{{props.feed.user.username}}</p>
											</v-flex>
											<v-flex md2 xs12>
											<v-icon large class="mr-2" color="red">fab fa-instagram</v-icon>
											</v-flex>
											</v-layout>
											<p style="margin-bottom:6px;"><v-icon small class="mr-2">fa fa-thumbs-up</v-icon> {{props.feed.likes.count}} likes</p>
											<p style="margin-bottom:6px;"><v-icon small class="mr-2">fa fa-comments</v-icon>  {{props.feed.comments.count}} comments</p>
											<p v-if="props.feed.caption">{{ props.feed.caption.text }}</p>
											<p><a :href="props.feed.link" target="new">click here</a></p>
											</v-flex>
										</vue-instagram>
								  </v-card-text>
								</v-card>
							  </v-tab-item>
							  <v-tab-item>
								<v-card flat>
								  <v-card-text>
									 <Timeline id="developer1job" source-type="profile" widget-class="mt-3 my-custom-class"/></Timeline>
								  </v-card-text>
								</v-card>
							  </v-tab-item>
							   <v-tab-item>
								<v-card flat>
								  <v-card-text>
									<p>Comming Soon .................
									</p>
								  </v-card-text>
								</v-card>
							  </v-tab-item>
							  <v-tab-item>
								<v-card flat>
								  <v-card-text>
									<youtube :video-id="Y9XZQO1n_7c"></youtube>
								  </v-card-text>
								</v-card>
							  </v-tab-item>
							</v-tabs>
					  </v-card>
                      </v-flex>
                    </v-layout>
                  </v-card-text>
                </v-card>
              </v-tab-item>
              <v-tab-item id="tab-6">
                <v-card flat>
                  <v-card-text>
                    <v-layout row wrap>
                      <v-flex xs12>
					   <v-btn
						fab
						small
						class="right"
						dark
						@click="addeventsform();editEventsDialog = true;"
						style="height: 30px; padding: 0px; width: 30px;"
						v-if="userRoleCall != 1"
					  >
						<v-icon style="font-size:20px">add</v-icon>
					  </v-btn>
                        <p class="headline">Events</p>
                        <div
                          class="blue-grey lighten-5 text-xs-center pa-2"
                          v-if="churchesData.events == ''"
                        >
                          <span class="subtitle blue-grey--text">
                            <v-icon large class="blue-grey--text">error_outline</v-icon>
                            <br />Keine Events eingetragen.
                            <br />
                         <!--   <span class="title">Events</span>-->
                          </span>
                        </div>

<BulmaAccordion :dropdown="false" :icon="'custom'" v-if="churchesData.events"                         v-for="event in churchesData.events">
    <BulmaAccordionItem>
        <p class="title is-4 has-text-weight-normal" slot="title">
				<v-layout row class="mt-2" >
					<v-flex d-flex md3 class="pa-0">
                        <v-card dark tile flat color="dark">
                            <v-card-text>
                                <p class="body-2 text-xs-center">
                                  {{ event.event_date | moment("dddd") }}
                                  <br />
                                  <span class="display-2">{{ event.event_date | moment("DD") }}</span>
                                  <br />
                                  {{ event.event_date | moment("MMM YYYY") }}
                                </p>
                            </v-card-text>
                        </v-card>
                    </v-flex>
					<v-flex d-flex md9 class="pa-0">
						<v-card light tile flat color="grey lighten-4">
							<v-card-text>
								<h3 class="headline font-weight-medium mb-0">{{ event.event_title }}
								 <span class="right body-1">
								 <v-btn
									fab
									small
									class="right"
									dark
									@click="deleteEventhere(event.comm_event_id)"
									style="height: 18px; padding: 0px; width: 18px;"
									v-if="userRoleCall != 1"
								  >
									<v-icon style="font-size:12px">delete</v-icon>
								</v-btn></span>
								</h3>
								<p class="body-1 mb-3">{{ event.timings }}</p>
							   <p class="body-1" v-html="event.address"></p>
							</v-card-text>
						</v-card>
					</v-flex>
				</v-layout>
		</p>
        <div class="high" slot="content">
		<p>
			<v-layout row class="mb-3" >
				<v-flex d-flex md3 class="pa-0">
                            <v-card dark tile flat color="dark">
                              <v-card-text>
                          		<p class="body-2 text-xs-center">
                                 &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp;
                                  <br />
                                  <span class="display-2"> &nbsp; &nbsp; &nbsp; &nbsp;</span>
                                  <br />
                                </p>
                              </v-card-text>
                            </v-card>
                          </v-flex>
                        <v-flex d-flex md9 class="pa-0">
                            <v-card light tile flat color="grey lighten-4">
                              <v-card-text>
                                <p class="body-1" v-html="event.event_description"></p>
								<GmapMap v-if="event.event_lat"
								  :center="{lat: parseFloat(event.event_lat), lng: parseFloat(event.event_long)}"
								  :zoom="12"
								  map-type-id="roadmap"
								  style="width: 80%; height: 180px"
								>
								  <GmapMarker
									:position="{lat: parseFloat(event.event_lat), lng: parseFloat(event.event_long)}"
									:clickable="true"
									:draggable="false"
									@click="center={lat: parseFloat(event.event_lat), lng: parseFloat(event.event_long)}"
								  />
								</GmapMap>
                              </v-card-text>
                            </v-card>
                        </v-flex>
			</v-layout>
			</p>
		</div>
    </BulmaAccordionItem>
</BulmaAccordion>
                       <!-- <v-layout
                          row
                          class="mb-3"
                          v-if="churchesData.events"
                          v-for="event in churchesData.events"
                        >

                          <v-flex d-flex md3 class="pa-0">
                            <v-card dark tile flat color="dark">
                              <v-card-text>
                                <p class="body-2 text-xs-center">
                                  {{ event.event_date | moment("dddd") }}
                                  <br />
                                  <span class="display-2">{{ event.event_date | moment("DD") }}</span>
                                  <br />
                                  {{ event.event_date | moment("MMM YYYY") }}
                                  <br />
                                  {{ event.timings}}
                                </p>
                              </v-card-text>
                            </v-card>
                          </v-flex>
                          <v-flex d-flex md9 class="pa-0">
                            <v-card light tile flat color="grey lighten-4">
                              <v-card-text>
                                <h3 class="headline font-weight-medium mb-1">{{ event.event_title }}</h3>
                                <h3 class="body-3 mb-2">{{ event.timings }}</h3>
                               <p class="body-1" v-html="event.address"></p>
                                <p class="body-1">{{ event.event_description }}</p>
								<GmapMap v-if="event.event_lat"
								  :center="{lat: parseFloat(event.event_lat), lng: parseFloat(event.event_long)}"
								  :zoom="12"
								  map-type-id="roadmap"
								  style="width: 80%; height: 180px"
								>
								  <GmapMarker
									:position="{lat: parseFloat(event.event_lat), lng: parseFloat(event.event_long)}"
									:clickable="true"
									:draggable="false"
									@click="center={lat: parseFloat(event.event_lat), lng: parseFloat(event.event_long)}"
								  />
								</GmapMap>
                              </v-card-text>
                            </v-card>
                          </v-flex>
                        </v-layout>-->

						 <v-dialog light v-model="editEventsDialog" max-width="650px">
							<v-card class="editDialog pa-3">
							  <v-card-title class="subheading pd-1">Event hinzufügen</v-card-title>
							  <v-card-text class="py-0">
							<v-layout row wrap>

							<v-flex xs12 sm6 md6>
							  <v-dialog
								ref="EventDatePicker"
								v-model="EventDatePickerMOdal"
								:return-value.sync="EventForm.EventDatePicker"
								persistent
								full-width
								width="290px"
							  >
								<v-text-field
								  slot="activator"
								  v-model="ComputedEvent"
								  label="Date(From)"
								  prepend-icon="event"
								  readonly
								></v-text-field>
								<v-date-picker v-model="EventForm.EventDatePicker" full-width>
								  <v-spacer></v-spacer>
								  <v-btn text color="primary" @click="EventDatePickerMOdal = false">Cancel</v-btn>
								  <v-btn text color="primary" @click="$refs.EventDatePicker.save(EventForm.EventDatePicker)">OK</v-btn>
								</v-date-picker>
							  </v-dialog>
							</v-flex>
							<v-flex xs12 sm6 md6>
							  <v-dialog
								ref="EventDatePicker"
								v-model="EventDatePickerMOdal"
								:return-value.sync="EventForm.EventDatePickerto"
								persistent
								full-width
								width="290px"
							  >
								<v-text-field
								  slot="activator"
								  v-model="ComputedEvent"
								  label="Date(to)"
								  prepend-icon="event"
								  readonly
								></v-text-field>
								<v-date-picker v-model="EventForm.EventDatePickerto" full-width>
								  <v-spacer></v-spacer>
								  <v-btn text color="primary" @click="EventDatePickerMOdal = false">Cancel</v-btn>
								  <v-btn text color="primary" @click="$refs.EventDatePicker.save(EventForm.EventDatePickerto)">OK</v-btn>
								</v-date-picker>
							  </v-dialog>
							</v-flex>
							<!--<v-layout justify-center>
								<v-date-picker v-model="EventForm.EventDatePicker"></v-date-picker>
							  </v-layout>	-->
								 <v-flex d-flex xs12 md6>
								   <v-dialog
									ref="EventTimeStatrTime"
									v-model="EventTimeStatrModal"
									:return-value.sync="EventForm.EventTimeStatrTime"
									persistent
									lazy
									full-width
									width="290px"
								  >
									<v-text-field
									  slot="activator"
									  v-model="EventForm.EventTimeStatrTime"
									  label="Start Time"
									  prepend-icon="access_time"
									  readonly
									></v-text-field>
									<v-time-picker v-if="EventTimeStatrModal" v-model="EventForm.EventTimeStatrTime" full-width>
									  <v-spacer></v-spacer>
									  <v-btn flat color="primary" @click="EventTimeStatrModal = false">Cancel</v-btn>
									  <v-btn flat color="primary" @click="$refs.EventTimeStatrTime.save(EventForm.EventTimeStatrTime)">OK</v-btn>
									</v-time-picker>
								  </v-dialog>
								  </v-flex>
								  <v-flex d-flex xs12 md6>
								   <v-dialog
									ref="EventCloseTime"
									v-model="EventTimeCloseModal"
									:return-value.sync="EventForm.EventCloseTime"
									persistent
									lazy
									full-width
									width="290px"
								  >
									<v-text-field
									  slot="activator"
									  v-model="EventForm.EventCloseTime"
									  label="Closing Time"
									  prepend-icon="access_time"
									  readonly
									></v-text-field>
									<v-time-picker v-if="EventTimeCloseModal" v-model="EventForm.EventCloseTime" full-width>
									  <v-spacer></v-spacer>
									  <v-btn flat color="primary" @click="EventTimeCloseModal = false">Cancel</v-btn>
									  <v-btn flat color="primary" @click="$refs.EventCloseTime.save(EventForm.EventCloseTime)">OK</v-btn>
									</v-time-picker>
								  </v-dialog>
								  </v-flex>
							</v-layout>
							<v-flex xs12>
								  <v-text-field
									label="Event Title"
									v-model="EventForm.EventTitle"
									colClasses="xl3 lg3 md3 sm3 xs3"
									required
								  ></v-text-field>
							</v-flex>
							<v-flex xs12>
								  <v-text-field
									label="Address"
									v-model="EventForm.EventAddress"
									colClasses="xl3 lg3 md3 sm3 xs3"
									required
								  ></v-text-field>
							</v-flex>
							<v-flex xs12>
								  <v-text-field
									label="Pincode"
									v-model="EventForm.EventPincode"
									colClasses="xl3 lg3 md3 sm3 xs3"
									required
								  ></v-text-field>
							</v-flex>
							<v-layout row>
								<v-flex xs12 md5>
								  <v-autocomplete
									label="Select Country"
									@change="listofStates"
									v-model="EventForm.country"
									search-input="name"
									:items="countryItems"
									item-text="name"
									item-value="id"
									:rules="[v => !!v || 'Country is required']"
									required
								  ></v-autocomplete>
								</v-flex>
								<v-spacer></v-spacer>
								<v-flex xs12 md6>
								 <v-autocomplete
									label="State:"
									:items="StateItems"
									item-text="StateName"
									item-value="ID"								
									v-model="EventForm.StateID"
									@change="listofcity"
								  ></v-autocomplete>
								</v-flex>
							</v-layout>
							<v-layout row>
								<v-flex xs12 md5>
								  <v-autocomplete
									label="Select City"
									v-model="EventForm.city"
									search-input="place"
									:items="cityItems"
									item-text="place"
									item-value="id"
									@change="listofmuncipalty"
									:rules="[v => !!v || 'City is required']"
									required
								  ></v-autocomplete>
								</v-flex>
								<v-spacer></v-spacer>
								<v-flex xs12 md6>
									<v-autocomplete
									  v-model="EventForm.municipaltyID"
									  :items="MunicipalityItems"
									  label="Municipality"
									  item-text="type"
									  item-value="id"
									></v-autocomplete>
								</v-flex>
							</v-layout>
							<v-flex xs12 mt-2>
								<wysiwyg name="EventAddress" placeholder="Event-Beschreibung" v-model="EventForm.EventDescription" required />
							</v-flex>
							<v-layout row wrap>
								<v-flex d-flex xs12 md5>
								  <v-text-field
									label="Lattitude"
									v-model="EventForm.Eventlattitude"
									colClasses="xl3 lg3 md3 sm3 xs3"
								  ></v-text-field>
								</v-flex>
								<v-spacer></v-spacer>
								<v-flex d-flex xs12 md6>								
								  <v-text-field
									label="Longitude"
									v-model="EventForm.Eventlongitude"
									colClasses="xl3 lg3 md3 sm3 xs3"
								  ></v-text-field>
								</v-flex>
							</v-layout>
							</v-card-text>
							<v-card-actions>
								<v-btn
								  depressed
								  class="grey lighten-2"
								  @click="editEventsDialog = false"
								>Abbrechen</v-btn>
								<v-btn depressed dark @click="saveEventshere" color="orange darken-1">Speichern</v-btn>
							</v-card-actions>
						</v-card>
					  </v-dialog>

                      </v-flex>
                    </v-layout>
                  </v-card-text>
                </v-card>
              </v-tab-item>
              <v-tab-item id="tab-7">
                <v-card flat>
                  <v-card-text>
                    <v-layout row wrap>
                      <v-flex xs12>
					  <v-btn
						fab
						small
						class="right"
						dark
						@click="addjobsform(); addJobsDialogbox = true;"
						style="height: 30px; padding: 0px; width: 30px;"
						v-if="userRoleCall != 1"
					  >
						<v-icon style="font-size:20px">add</v-icon>
					  </v-btn>
                        <p class="headline">Jobs</p>

                        <div
                          class="blue-grey lighten-5 text-xs-center pa-2"
                          v-if="churchesData.jobs == ''"
                        >
                          <span class="subtitle blue-grey--text">
                            <v-icon large class="blue-grey--text">error_outline</v-icon>
                            <br />Keine Jobs vakant.
                            <br />
                            <!--<span class="title">Jobs</span>-->
                          </span>
                        </div>
				<BulmaAccordion :dropdown="false" :icon="'custom'" v-if="churchesData.jobs" v-for="job in churchesData.jobs">
					<BulmaAccordionItem>
						<p class="title is-4 has-text-weight-normal" slot="title">
								 <v-layout row class="mt-2">
										  <v-flex d-flex md3 class="pa-0">
											<v-card dark tile flat color="dark">
											  <v-card-text>													
													 <v-img
														  v-if="!job.f_image"
														  v-bind:src="'http://dev.woobii.com/admin/'+churcheslogo"
														  max-width="175"
														  class="mb-3"
														  srcset
														  lazy-src
														>
														</v-img>													
													 <v-img
														  v-else-if="job.f_image"
														  v-bind:src="'http://dev.woobii.com/admin/'+job.f_image"
														  max-width="175"
														  class="mb-3"
														  srcset
														  lazy-src
														>
														</v-img>
														<v-img
														  v-else
														  :src="require(`@/assets/woobii-banner.jpg`)"
														  :lazy-src="require(`@/assets/woobii-banner.jpg`)"
														>
														</v-img>
													
													
											  </v-card-text>
											</v-card>
										  </v-flex>
										  <v-flex d-flex md9 class="pa-0">
											<v-card light tile flat color="grey lighten-4">
											  <v-card-text>
												<h3 class="headline font-weight-medium mb-0">
												  {{job.job_title}}
												  <span
													class="right body-1"
												  >{{job.date | moment("DD.MM.YYYY")}}
														<v-btn
															fab
															class="editIconBtn"
															dark
															small
															@click="getjobdetail(job.comm_jobId); addJobsDialogbox = true"
															v-if="userRoleCall != 1"
															style="height: 18px; padding: 0px; width: 18px;"
														  >
															<v-icon style="font-size:12px;">edit</v-icon>
														  </v-btn>

													<v-btn
														fab
														small
														class="right"
														dark
														@click="deleteJobsHere(job.comm_jobId)"
														style="height: 18px; padding: 0px; width: 18px;"
														v-if="userRoleCall != 1"
													  >
													<v-icon style="font-size:12px;">delete</v-icon>
													</v-btn> </span>
												</h3>
												<p class="body-1 mb-1">{{job.Lesson}}</p>
												<p class="body-1">kirche im kino, Innsbruck (A)</p>
											  </v-card-text>
											</v-card>
										  </v-flex>
										</v-layout>
						</p>
						<div class="high" slot="content">
						<p>
							 <v-layout row class="mb-3" >
										  <v-flex d-flex md3 class="pa-0">
											<v-card dark tile flat color="dark">
											  <v-card-text>

											  </v-card-text>
											</v-card>
										  </v-flex>
										  <v-flex d-flex md9 class="pa-0">
											<v-card light tile flat color="grey lighten-4">
											  <v-card-text>
												<p class="body-1 " v-html="job.job_description"></p>
												<GmapMap v-if="job.jobLatt"
												  :center="{lat: parseFloat(job.jobLatt), lng: parseFloat(job.JobLng)}"
												  :zoom="12"
												  map-type-id="roadmap"
												  style="width: 80%; height: 180px"
												>
												  <GmapMarker
													:position="{lat: parseFloat(job.jobLatt), lng: parseFloat(job.JobLng)}"
													:clickable="true"
													:draggable="false"
													@click="center={lat: parseFloat(job.jobLatt), lng: parseFloat(job.JobLng)}"
												  />
												</GmapMap>
											  </v-card-text>
											</v-card>

										  </v-flex>
										</v-layout>
							</p>
						</div>
					</BulmaAccordionItem>
				</BulmaAccordion>

                        <!--<v-layout
                          row
                          class="mb-3"
                          v-if="churchesData.jobs"
                          v-for="job in churchesData.jobs"
                        >
                          <v-flex d-flex md3 class="pa-0">
                            <v-card dark tile flat color="dark">
                              <v-card-text>
								 <v-img style=""
									  v-if="job.job_banner"
									  :lazy-src="'http://dev.woobii.com/admin/'+job.job_banner"
									  :src="'http://dev.woobii.com/admin/'+job.job_banner"
									>
									</v-img>
									<v-img
									  v-else
									  :src="require(`@/assets/woobii-banner.jpg`)"
									  :lazy-src="require(`@/assets/woobii-banner.jpg`)"
									>
									</v-img>
                              </v-card-text>
                            </v-card>
                          </v-flex>
                          <v-flex d-flex md9 class="pa-0">
                            <v-card light tile flat color="grey lighten-4">
                              <v-card-text>
                                <h3 class="headline font-weight-medium mb-2">
                                  {{job.job_title}}
                                  <span
                                    class="right body-1"
                                  >{{job.date | moment("DD.MM.YYYY")}}</span>
                                </h3>
								<p class="body-1 mb-1">{{job.Lesson}}</p>
								<p class="body-1">kirche im kino, Innsbruck (A)</p>
                                <p class="body-1 " v-html="job.job_description"></p>
								<GmapMap v-if="job.jobLatt"
								  :center="{lat: parseFloat(job.jobLatt), lng: parseFloat(job.JobLng)}"
								  :zoom="12"
								  map-type-id="roadmap"
								  style="width: 80%; height: 180px"
								>
								  <GmapMarker
									:position="{lat: parseFloat(job.jobLatt), lng: parseFloat(job.JobLng)}"
									:clickable="true"
									:draggable="false"
									@click="center={lat: parseFloat(job.jobLatt), lng: parseFloat(job.JobLng)}"
								  />
								</GmapMap>
                              </v-card-text>
                            </v-card>

                          </v-flex>
                        </v-layout>-->

                       <v-dialog light v-model="addJobsDialogbox" max-width="650px">
							<v-card class="editDialog pa-3">
							  <v-card-title class="subheading pd-1">Jobs hinzufügen</v-card-title>
							  <v-card-text class="py-0">
							<v-layout row wrap>
						<!--<v-flex xs6 class="pl-3">
							  <v-text-field
								label="Upload Logo"
								@click="pickFile"
								v-model="JobsForm.imageName"
								prepend-icon="attach_file"
							  ></v-text-field>
							  <input
								type="file"
								style="display: none"
								ref="image"
								accept="image/*"
								@change="onFilePicked"
							  >
                        </v-flex>-->
							<v-image-input
							  v-model="imageFIle"
							  :image-quality="0.85"
							  clearable

							   minScaling="contain"
							  image-format="jpeg"
							/>
							<v-flex xs12 sm6 md6>
							  <v-dialog
								ref="JobOffertDate"
								v-model="JobDatePickerModal"
								:return-value.sync="JobsForm.JobOffertDate"
								persistent
								full-width
								width="290px"
							  >
								<v-text-field
								  slot="activator"
								  v-model="computedDateFormatted"
								  label="Date"
									persistent-hint
								  prepend-icon="event"
								  readonly
								></v-text-field>
								<v-date-picker v-model="JobsForm.JobOffertDate" full-width>
								  <v-spacer></v-spacer>
								  <v-btn text color="primary" @click="JobDatePickerModal = false">Cancel</v-btn>
								  <v-btn text color="primary" @click="$refs.JobOffertDate.save(JobsForm.JobOffertDate)">OK</v-btn>
								</v-date-picker>
							  </v-dialog>
							</v-flex>
							<v-flex xs12 md6>
							  <v-select
								label="Stundenausmaß"
								v-model="JobsForm.ofLessons"
								search-input="place"
								:items="ofLessonsItems"
								item-text="place"
								item-value="id"
								required
							  ></v-select>
							</v-flex>
							</v-layout>
							<v-layout row>
							<v-flex xs12 md5>
								  <v-text-field
									label="Job Position"
									v-model="JobsForm.JobPosition"
									colClasses="xl3 lg3 md3 sm3 xs3"
									required
								  ></v-text-field>
							</v-flex>
							<v-spacer></v-spacer>
							<v-flex xs12 md6>
								  <v-text-field
									label="Email"
									v-model="JobsForm.JobEmail"
									colClasses="xl3 lg3 md3 sm3 xs3"
									required
								  ></v-text-field>
							</v-flex>
							</v-layout>
							<v-flex xs12>
								  <v-text-field
									label="Address"
									v-model="JobsForm.JobAddress"
									colClasses="xl3 lg3 md3 sm3 xs3"
									required
								  ></v-text-field>
							</v-flex>
							<v-flex xs12>
								  <v-text-field
									label="Pincode"
									v-model="JobsForm.pincode"
									colClasses="xl3 lg3 md3 sm3 xs3"
									required
								  ></v-text-field>
							</v-flex>
							<v-layout row>
								<v-flex xs12 md5>
								  <v-autocomplete
									label="Select Country"
									@change="listofStates"
									v-model="JobsForm.country"
									search-input="name"
									:items="countryItems"
									item-text="name"
									item-value="id"
									:rules="[v => !!v || 'Country is required']"
									required
								  ></v-autocomplete>
								</v-flex>
								<v-spacer></v-spacer>
								<v-flex xs12 md6>
								 <v-autocomplete
									label="State:"
									:items="StateItems"
									item-text="StateName"
									item-value="ID"								
									v-model="JobsForm.StateID"
									@change="listofcity"
								  ></v-autocomplete>
								</v-flex>
							</v-layout>
							<v-layout row>
								<v-flex xs12 md5>
								  <v-autocomplete
									label="Select City"
									v-model="JobsForm.city"
									search-input="place"
									:items="cityItems"
									item-text="place"
									item-value="id"
									@change="listofmuncipalty"
									:rules="[v => !!v || 'City is required']"
									required
								  ></v-autocomplete>
								</v-flex>
								<v-spacer></v-spacer>
								<v-flex xs12 md6>
									<v-autocomplete
									  v-model="JobsForm.municipaltyID"
									  :items="MunicipalityItems"
									  label="Municipality"
									  item-text="type"
									  item-value="id"
									></v-autocomplete>
								</v-flex>					
							</v-layout>
							<v-flex xs12>
								<wysiwyg name="JobsForm" v-model="JobsForm.JobDescription" required placeholder="Jobs Beschreibung"/>
							</v-flex>
							<v-layout row wrap>
								<v-flex d-flex xs12 md5>
								  <v-text-field
									label="Lattitude"
									v-model="JobsForm.JobsLattitude"
									colClasses="xl3 lg3 md3 sm3 xs3"
								  ></v-text-field>
								</v-flex>
								<v-spacer></v-spacer>
								<v-flex d-flex xs12 md6>
								  <v-text-field
									label="Longitude"
									v-model="JobsForm.JobsLongitude"
									colClasses="xl3 lg3 md3 sm3 xs3"
								  ></v-text-field>
								</v-flex>
							</v-layout>
							</v-card-text>
							<v-card-actions>
								<v-btn
								  depressed
								  class="grey lighten-2"
								  @click="addJobsDialogbox = false"
								>Abbrechen</v-btn>
								<v-btn depressed dark @click="saveJobsHere" color="orange darken-1">Speichern</v-btn>
							</v-card-actions>
						</v-card>
					  </v-dialog>
                      </v-flex>
                    </v-layout>
                  </v-card-text>
                </v-card>
              </v-tab-item>
              <v-tab-item id="tab-7">
                <v-card flat>
                  <v-card-text>
                    <v-layout row wrap>
                      <v-flex xs12>
                        <p class="headline">Bewertungen</p>
                        <!-- <div class="blue-grey lighten-5 text-xs-center pa-2">
                          <span class="subtitle blue-grey--text">
                            <v-icon large class="blue-grey--text">error_outline</v-icon>
                            <br>No data found in
                            <br>
                            <span class="title">Bewertungen</span>
                          </span>
                        </div>-->
                        <p
                          class="body-1 font-weight-bold"
                        >Deine Meinung zahlt. Teile deine Erfahrung damit andere fie passende Gemeinde finden.</p>
                        <v-btn flat dark class="blue darken-4 ma-0 mb-4 py-2">
                          <v-icon class="mr-3">fab fa-facebook</v-icon>Log in with Facebook
                        </v-btn>
						
						  
                        <p class="title font-weight-regular">Gottesdienste</p>
                        <p class="body-1 pl-3">
                          Offenheit fur neue Besucher
                          <span class="right">
                            <v-rating
                              v-model="rating"
                              readonly
                              dense
                              background-color="grey darken-4"
                              color="grey darken-4"
                            ></v-rating>
                          </span>
                        </p>
                        <p class="body-1 pl-3">
                          Christuszentrierte Predigten
                          <span class="right">
                            <v-rating
                              v-model="rating"
                              readonly
                              dense
                              background-color="grey darken-4"
                              color="grey darken-4"
                            ></v-rating>
                          </span>
                        </p>
                        <p class="title font-weight-regular mt-4">Gemeinschaft</p>
                        <p class="body-1 pl-3">
                          Kleingruppen & Hauskreise
                          <span class="right">
                            <v-rating
                              v-model="rating"
                              readonly
                              dense
                              background-color="grey darken-4"
                              color="grey darken-4"
                            ></v-rating>
                          </span>
                        </p>
                        <p class="body-1 pl-3">
                          Genertfruhstuck & Gebetsabende
                          <span class="right">
                            <v-rating
                              v-model="rating"
                              readonly
                              dense
                              background-color="grey darken-4"
                              color="grey darken-4"
                            ></v-rating>
                          </span>
                        </p>
                        <p class="body-1 pl-3">
                          Dienstbereitschaft
                          <span class="right">
                            <v-rating
                              v-model="rating"
                              readonly
                              dense
                              background-color="grey darken-4"
                              color="grey darken-4"
                            ></v-rating>
                          </span>
                        </p>
                        <p class="title font-weight-regular mt-4">Nachfolge</p>
                        <p class="body-1 pl-3">
                          Sehnsucht Gott ahnlicher zu werden
                          <span class="right">
                            <v-rating
                              v-model="rating"
                              readonly
                              dense
                              background-color="grey darken-4"
                              color="grey darken-4"
                            ></v-rating>
                          </span>
                        </p>
                        <p class="body-1 pl-3">
                          Genertfruhstuck & Gebetsabende
                          <span class="right">
                            <v-rating
                              v-model="rating"
                              readonly
                              dense
                              background-color="grey darken-4"
                              color="grey darken-4"
                            ></v-rating>
                          </span>
                        </p>
                        <p class="body-1 pl-3">
                          Dienstbereitschaft
                          <span class="right">
                            <v-rating
                              v-model="rating"
                              readonly
                              dense
                              background-color="grey darken-4"
                              color="grey darken-4"
                            ></v-rating>
                          </span>
                        </p>
                      </v-flex>
                    </v-layout>
                  </v-card-text>
                </v-card>
              </v-tab-item>
            </v-tabs>
          </v-flex>
          <v-flex xs12 md4 class="tabRight">
            <v-btn flat block class="colorGreen ma-0 mb-3 py-2" @click="sharedialog = true">
              <v-icon class="mr-2">share</v-icon>Kirchengemeinde empfehlen
            </v-btn>
            <v-dialog v-model="sharedialog" max-width="290" dark>
              <v-card>
                <v-card-title class="mr-2">
                  <v-icon class="mr-2">share</v-icon>Kirchengemeinde empfehlen
                </v-card-title>
                <v-card-text class="mt-0">
                  <social-sharing
                    :url="socialUrl"
                    :title="socialTitle"
                    :description="socialDescription"
                    :quote="socialQuote"
                    hashtags="Woobii"
                    twitter-user="woobii"
                    inline-template
                  >
                    <div class="socialBox">
                      <network network="email">
                        <v-icon class>email</v-icon>Email
                      </network>
                      <network network="facebook">
                        <v-icon class>fab fa-facebook</v-icon>Facebook
                      </network>
                      <network network="twitter">
                        <v-icon class>fab fa-twitter-square</v-icon>Twitter
                      </network>
                      <!-- <network network="googleplus">
                        <v-icon class>fab fa-google-plus-square</v-icon>Google +
                      </network>-->
                      <network network="linkedin">
                        <v-icon class>fab fa-linkedin</v-icon>LinkedIn
                      </network>
                      <network network="pinterest">
                        <v-icon class>fab fa-pinterest-square</v-icon>Pinterest
                      </network>
                      <network network="skype">
                        <v-icon class>fab fa-skype</v-icon>Skype
                      </network>
                    </div>
                  </social-sharing>
                </v-card-text>
              </v-card>
            </v-dialog>
            <!-- <v-btn flat block class="grey lighten-4 ma-0 mb-4 py-2">
              <v-rating
                v-model="rating"
                readonly
                dense
                small
                background-color="grey darken-4"
                color="grey darken-4"
              ></v-rating>Kirchengemeinde bewerten
            </v-btn>-->
            <p class="headline">
              Gemeindeprofil
             
			  <v-btn
                fab
                small
                class="right"
                dark
                @click="addtimingmultiple= true;addtimingform();"
                style="height: 20px; padding: 0px; width: 20px;"
                v-if="userRoleCall != 1"
              >
                <v-icon style="font-size:12px">add</v-icon>
              </v-btn>
			   <v-dialog light v-model="addtimingmultiple" max-width="650px">
							<v-card class="editDialog pa-3">
							  <v-card-title class="subheading pd-1">Add Timing</v-card-title>
							  <v-card-text class="py-0">
							<v-layout row wrap>
							<v-flex xs5>
                      <v-text-field
                        label="Celebration Location"
                        v-model="timingform.celbrationLocation"
                        colClasses="xl3 lg3 md3 sm3 xs3"
                        required
                      ></v-text-field>
                    </v-flex>
					<v-spacer></v-spacer>
							<v-flex xs6>
				   <v-select
					    v-model="timingform.weekdays"
					  :items="weekdays"
					  menu-props="auto"
					  label="Weekdays"
					
					></v-select>
					</v-flex>
					
					
				 
					<v-flex xs12 sm6 md5>
							  <v-dialog
								ref="timingOffertDate"
								v-model="GemeindeDatepickerModal"
								:return-value.sync="timingform.timingOffertDate"
								persistent
								full-width
								width="290px"
							  >
								<v-text-field
								  slot="activator"
								  v-model="computeddatetime"
								  label="Date"
									persistent-hint
								  prepend-icon="event"
								  readonly
								></v-text-field>
								<v-date-picker v-model="timingform.timingOffertDate" full-width>
								  <v-spacer></v-spacer>
								  <v-btn text color="primary" @click="GemeindeDatepickerModal = false">Cancel</v-btn>
								  <v-btn text color="primary" @click="$refs.timingOffertDate.save(timingform.timingOffertDate)">OK</v-btn>
								</v-date-picker>
							  </v-dialog>
							</v-flex>
							
								<v-spacer></v-spacer>
					<v-flex d-flex xs12 md6>
					
						<v-dialog
                            ref="fromtime"
                            v-model="modaltime2"
                            :return-value.sync="timingform.fromtime"
                            persistent
                            lazy
                            full-width
                            width="290px"
                          >
                            <v-text-field
                              slot="activator"
                              v-model="timingform.fromtime"
                              label="From Time"
                              prepend-icon="access_time"
                              readonly
							   
                            ></v-text-field>
                            <v-time-picker v-if="modaltime2" v-model="timingform.fromtime" full-width>
                              <v-spacer></v-spacer>
                              <v-btn flat color="primary" @click="modaltime2 = false">Cancel</v-btn>
                              <v-btn flat color="primary" @click="$refs.fromtime.save(timingform.fromtime)">OK</v-btn>
                            </v-time-picker>
                          </v-dialog>
                          </v-flex>
						  
						  <v-flex d-flex xs12 md5>
                           <v-dialog
                            ref="totime"
                            v-model="modaltime3"
                            :return-value.sync="timingform.totime"
                            persistent
                            lazy
                            full-width
                            width="290px"
                          >
                            <v-text-field
                              slot="activator"
                              v-model="timingform.totime"
                              label="To Time"
                              prepend-icon="access_time"
                              readonly
							   
                            ></v-text-field>
                            <v-time-picker v-if="modaltime3" v-model="timingform.totime" full-width>
                              <v-spacer></v-spacer>
                              <v-btn flat color="primary" @click="modaltime3 = false">Cancel</v-btn>
                              <v-btn flat color="primary" @click="$refs.totime.save(timingform.totime)">OK</v-btn>
                            </v-time-picker>
                          </v-dialog>
                          </v-flex>
						  <v-spacer></v-spacer>
						  
						   <v-flex xs6>
				   <v-select
					  v-model="timingform.eventtype"
					  :items="eventtype"
					  menu-props="auto"
					  label="Event Type"
					
					></v-select>
					</v-flex>
					
					 <v-flex xs12>
				   <v-select
					  v-model="timingform.Frequency"
					  :items="Frequency"
					  menu-props="auto"
					  label="Frequency"
					 
					></v-select>
					</v-flex>
					
						<v-flex xs12>
                      <v-text-field
                        label="Event Description"
                        v-model="timingform.eventdescription"
                       
                        required
					
                      ></v-text-field>
                    </v-flex>
					<v-spacer></v-spacer>
					<v-flex xs12>
                      <v-text-field
                        label="Address"
                        v-model="timingform.address"
                  
                        required
						
                      ></v-text-field>
                    </v-flex>
							</v-layout>
							</v-card-text>
							<v-card-actions>
								<v-btn
								  depressed
								  class="grey lighten-2"
								  @click="addtimingmultiple = false"
								>Abbrechen</v-btn>
								<v-btn depressed dark @click="saveMultipletiminghere" color="orange darken-1">Speichern</v-btn>
							</v-card-actions>
						</v-card>
					  </v-dialog>

			  
              <v-dialog light v-model="editDialogaddress" max-width="650px">
                <v-card class="editDialog pa-3">
                  <v-card-title class="subheading pd-1">Gemeindeprofil</v-card-title>
                  <v-card-text class="py-0">
				  <v-layout row wrap>
					<v-flex d-flex xs12 md6>
						<v-dialog
                            ref="starttimeof"
                            v-model="modal2"
                            :return-value.sync="form2.starttimeof"
                            persistent
                            lazy
                            full-width
                            width="290px"
                          >
                            <v-text-field
                              slot="activator"
                              v-model="form2.starttimeof"
                              label="Start Time"
                              prepend-icon="access_time"
                              readonly
                            ></v-text-field>
                            <v-time-picker v-if="modal2" v-model="form2.starttimeof" full-width>
                              <v-spacer></v-spacer>
                              <v-btn flat color="primary" @click="modal2 = false">Cancel</v-btn>
                              <v-btn flat color="primary" @click="$refs.starttimeof.save(form2.starttimeof)">OK</v-btn>
                            </v-time-picker>
                          </v-dialog>
                          </v-flex>
						  <v-flex d-flex xs12 md6>
                           <v-dialog
                            ref="closetime"
                            v-model="modal3"
                            :return-value.sync="form2.closetime"
                            persistent
                            lazy
                            full-width
                            width="290px"
                          >
                            <v-text-field
                              slot="activator"
                              v-model="form2.closetime"
                              label="Closing Time"
                              prepend-icon="access_time"
                              readonly
                            ></v-text-field>
                            <v-time-picker v-if="modal3" v-model="form2.closetime" full-width>
                              <v-spacer></v-spacer>
                              <v-btn flat color="primary" @click="modal3 = false">Cancel</v-btn>
                              <v-btn flat color="primary" @click="$refs.closetime.save(form2.closetime)">OK</v-btn>
                            </v-time-picker>
                          </v-dialog>
                          </v-flex>
                        </v-layout>
					<v-flex xs12>
                      <v-text-field
                        label="Celebration Location"
                        v-model="form2.celbrationLocation"
                        colClasses="xl3 lg3 md3 sm3 xs3"
                        required
                      ></v-text-field>
                    </v-flex>
					<v-layout row>
					<v-flex xs12 md5>
                      <v-text-field
                        label="Email"
                        v-model="form2.emailid"
                        colClasses="xl3 lg3 md3 sm3 xs3"
                        required
                      ></v-text-field>
                    </v-flex>
					<v-spacer></v-spacer>
					<v-flex xs12 md6>
                      <v-text-field
                        label="Contact No"
                        v-model="form2.contactno"
                        :rules="form2.mobileRules"
                        colClasses="xl3 lg3 md3 sm3 xs3"
                        required
					></v-text-field>
                    </v-flex>
					</v-layout>
					<v-flex xs12>
                      <v-text-field
                        label="Address"
                        v-model="form2.address1"
                        colClasses="xl3 lg3 md3 sm3 xs3"
                        required
                      ></v-text-field>
                    </v-flex>
					<v-flex xs12>
                      <v-text-field
                        label="Pincode"
                        v-model="form2.pincode"
                        colClasses="xl3 lg3 md3 sm3 xs3"
                        required
                      ></v-text-field>
                    </v-flex>

					<v-layout row wrap>
                    <v-flex xs12 md5>
						  <v-autocomplete
							label="Select Country"
							@change="listofStates"
							v-model="form2.country"
							search-input="name"
							:items="countryItems"
							item-text="name"
							item-value="id"
							:rules="[v => !!v || 'Country is required']"
							required
						  ></v-autocomplete>
                    </v-flex>
					<v-spacer></v-spacer>
                    <v-flex xs12 md6>
						<v-autocomplete
							label="State:"
							v-model="form2.StateID"
							:items="StateItems"
							item-text="StateName"
							item-value="ID"	
							@change="listofcity"
						></v-autocomplete>
                    </v-flex>
					</v-layout>
					<v-layout row>
						<v-flex xs12 md5>
						<v-autocomplete
							label="Select City"
							v-model="form2.city"
							search-input="place"
							:items="cityItems"
							item-text="place"
							item-value="id"
							@change="listofmuncipalty"
							:rules="[v => !!v || 'City is required']"
							required
						  ></v-autocomplete>
						</v-flex>
						<v-spacer></v-spacer>
						<v-flex xs12 md6>
							<v-autocomplete
							  v-model="form2.municipalityID"
							  :items="MunicipalityItems"
							  label="Municipality"
							  item-text="type"
							  item-value="id"
							></v-autocomplete>
						</v-flex>					
					</v-layout>					
					<!--<v-layout row wrap>
                    <v-flex d-flex xs12 md5>
                      <v-text-field
                        label="Lattitude"
                        v-model="form2.lattitude"
                        colClasses="xl3 lg3 md3 sm3 xs3"
                      ></v-text-field>
                    </v-flex>
					<v-spacer></v-spacer>
                    <v-flex d-flex xs12 md6>					
                      <v-text-field
                        label="Longitude"
                        v-model="form2.longitude"
                        colClasses="xl3 lg3 md3 sm3 xs3"
                      ></v-text-field>
                    </v-flex>
					</v-layout>-->
					 
					<span class="font-weight-bold black--text">Gottesdienst</span>
					<v-divider></v-divider>
					<v-checkbox
					  v-model="sameaddressforoffice"
					  :label="`Office Adresse anders als Kirchenadresse: ${checkbox.toString()}`"
					  @click="handlechange"
					  value="accepted"
					unchecked-value="not_accepted"
					></v-checkbox>
					</v-card-text>
                  	<v-card-text class="py-0" v-show="isNight">                   
					<v-flex xs12>
                      <v-text-field
                        label="Office Address"
                        v-model="form2.head_office_address"
                        colClasses="xl3 lg3 md3 sm3 xs3"
                        required
                      ></v-text-field>
                    </v-flex>
					<v-flex xs12>
                      <v-text-field
                        label="Office Pincode"
                        v-model="form2.headoffice_pincode"
                        colClasses="xl3 lg3 md3 sm3 xs3"
                        required
                      ></v-text-field>
                    </v-flex>
					<v-layout row wrap>
                    <v-flex xs12 md5>
						  <v-autocomplete
							label="Select Country"
							@change="listofofficeStates"
							v-model="form2.headofficeCountry"
							search-input="name"
							:items="countryItems"
							item-text="name"
							item-value="id"
							:rules="[v => !!v || 'Country is required']"
							required
						  ></v-autocomplete>
                    </v-flex>
					<v-spacer></v-spacer>
                    <v-flex xs12 md6>
						<v-autocomplete
							label="State:"
							v-model="form2.head_office_StateID"
							:items="StateofficeItems"
							item-text="StateName"
							item-value="ID"	
							@change="listofofficecity"
						></v-autocomplete>
                    </v-flex>
					</v-layout>
					<v-layout row>
						<v-flex xs12 md5>
						<v-autocomplete
							label="Select City"
							v-model="form2.headoffice_city"
							search-input="place"
							:items="cityofficeItems"
							item-text="place"
							item-value="id"
							return-object
							@change="listofofficemuncipalty"
							:rules="[v => !!v || 'City is required']"
							required
						  ></v-autocomplete>
						</v-flex>
						<v-spacer></v-spacer>
						<v-flex xs12 md6>
							<v-autocomplete
							  v-model="form2.headoffice_municiplity"
							  :items="MunicipalityofficeItems"
							  label="Municipality"
							  item-text="type"
							  item-value="id"
							></v-autocomplete>
						</v-flex>					
					</v-layout>	
                    <v-flex xs12>
                      <v-text-field
                        label="Head Office No"
                        v-model="form2.headoffice"
                        colClasses="xl3 lg3 md3 sm3 xs3"
                      ></v-text-field>
                    </v-flex>
					<v-flex xs12>
                      <v-text-field
                        label="Email"
                        v-model="form2.head_office_email"
                        :rules="form2.emailRules"
                        colClasses="xl3 lg3 md3 sm3 xs3"
                      ></v-text-field>
                    </v-flex>
                    <v-flex xs12>
                      <v-text-field
                        label="Website Address"
                        v-model="form2.website"
                        colClasses="xl3 lg3 md3 sm3 xs3"
                      ></v-text-field>
                    </v-flex>
					 <!-- <v-layout row wrap>
                    <v-flex d-flex xs12 md6>
                      <v-text-field
                        label="Head Office Lattitude"
                        v-model="form2.head_office_lat"
                        colClasses="xl3 lg3 md3 sm3 xs3"
                      ></v-text-field>
                    </v-flex>
                    <v-flex d-flex xs12 md6>
                      <v-text-field
                        label="Head Office Longitude"
                        v-model="form2.head_office_lng"
                        colClasses="xl3 lg3 md3 sm3 xs3"
                      ></v-text-field>
                    </v-flex>
					</v-layout>-->
                  </v-card-text>
                  <v-card-actions>
                    <v-btn
                      depressed
                      class="grey lighten-2"
                      @click="editDialogaddress = false"
                    >Abbrechen</v-btn>
                    <v-btn depressed dark @click="saveAddress" color="orange darken-1">Speichern</v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </p>
            <p class="body-1" v-for="timing in churchesData.timing">
              <span class="font-weight-bold black--text">Gottesdienste</span>
			  <v-btn
			fab
			small
			class="right"
			dark
			@click="deletetiming(timing.timing_id)"
			style="height: 18px; padding: 0px; width: 18px;"
			v-if="userRoleCall != 1"
			>
			<v-icon style="font-size:12px;">delete</v-icon>
			</v-btn> 
              <br />
			  {{timing.weekdays}} <br />
              {{timing.date}}<br />
			  {{timing.fromtime}}<br />
			  {{timing.totime}}<br />
			  {{timing.eventtype}}<br />
			  {{timing.frequency}}<br />
			  {{timing.eventdescription}}<br />
			  {{timing.address}}<br />
			  {{timing.celebrationlocation}}<br />
			   
			
            </p>
			 <v-btn
                fab
                small
                class="right"
                dark
                @click="editDialogaddress = true; addressService();"
                style="height: 20px; padding: 0px; width: 20px;"
                v-if="userRoleCall != 1"
              >
                <v-icon style="font-size:12px">edit</v-icon>
              </v-btn>
            <span v-html="churchesData[0].address"></span><br/>
			 <span v-html="churchesData[0].Pincode"></span>
            <span v-html="churchesData[0].city"  style="margin-left:2px;"></span><br/>
            <span v-html="churchesData[0].country"></span><br/>
           
			
			
			
            <!-- <p class="body-1">
                <span class="font-weight-bold black--text">Head Office</span>
                <br>Gutenbergstr. 6
                <br>6020 Innsbruck
                <br>Osterreich
            </p>-->
			<span class="font-weight-bold black--text" v-if="churchesData[0].head_office_address && churchesData[0].head_office || churchesData[0].head_office_address || churchesData[0].head_office">Head Office</span>
			 <p class="body-1" v-html="churchesData[0].head_office_address"></p>
            <p class="body-1" v-if="churchesData[0].head_office">
              Tel: {{churchesData[0].head_office}}
              <br />
              <a
				style="color:#00bfff" 
                v-if="churchesData[0].email_id"
                :href="'mailto:'+churchesData[0].email_id"
                target="new"
              >{{churchesData[0].email_id}}</a>
            </p>
            <p class="body-1" v-if="churchesData[0].website">
             <a style="color:#00bfff" v-bind:href=churchesData[0].website>{{ churchesData[0].website }}</a>
            </p>
            <GmapMap v-if="churchesData[0].lat"
              :center="{lat: parseFloat(latstreet), lng: parseFloat(lngstreet)}"
              :zoom="16"
              map-type-id="roadmap"
              style="width: 100%; height: 180px"
            >
              <GmapMarker
                :position="{lat: parseFloat(latstreet), lng: parseFloat(lngstreet)}"
                :clickable="true"
                :draggable="false"
                @click="center={lat: parseFloat(latstreet), lng: parseFloat(lngstreet)}"
              />
            </GmapMap>
			<GmapMap v-else
              :center="{lat: parseFloat(churchesData[0].head_office_lat), lng: parseFloat(churchesData[0].head_office_lng)}"
              :zoom="16"
              map-type-id="roadmap"
              style="width: 100%; height: 180px"
            >
              <GmapMarker
                :position="{lat: parseFloat(churchesData[0].head_office_lat), lng: parseFloat(churchesData[0].head_office_lng)}"
                :clickable="true"
                :draggable="false"
                @click="center={lat: parseFloat(churchesData[0].head_office_lat), lng: parseFloat(churchesData[0].head_office_lng)}"
              />
            </GmapMap>
          </v-flex>
        </v-layout>
        <!--<v-dialog v-model="dialogLoader" hide-overlay persistent width="300">
          <v-card color="#FA6E2F" dark>
            <v-card-text>
              Please stand by
              <v-progress-linear indeterminate color="white" class="mb-0"></v-progress-linear>
            </v-card-text>
          </v-card>
        </v-dialog>-->
		<loading :active.sync="isLoading"
        :can-cancel="false"
		:color="orange"
		:height="150"
		:width="150"
        :is-full-page="fullPage"></loading>
      </v-container>
    </section>
	<!--<div class="pretty p-default">
        <input type="checkbox" />
        <div class="state">
            <label>Check</label>
        </div>
    </div>-->
  </v-content>
  </v-flex>
    </v-layout>
</template>

<script>
import axios from 'axios'
import $ from 'jquery'
import VueGallery from 'vue-gallery'
import truncate from 'vue-truncate-collapsed'
import Loading from 'vue-loading-overlay'
import { BulmaAccordion, BulmaAccordionItem } from 'vue-bulma-accordion'
import 'vue-loading-overlay/dist/vue-loading.css'
import VueInstagram from 'vue-instagram'
import { Tweet, Moment, Timeline } from 'vue-tweet-embed'
import VueYouTubeEmbed from 'vue-youtube-embed'
import { vueVimeoPlayer } from 'vue-vimeo-player'
export default {
  name: 'KitrchenFinderLocation',
  data () {
    return {
		 Frequency:['unique event (einmalig)','weekly (Wöchentlich)','every 2 weeks (14tägig) monthly (Monatlich)',' 4 times a year (quartalsweise)','2 times a year (halbjährlich)','once a year (jährlich)'],
		   
		  weekdays:['Monday','Tuesday','Wednesday','Thursday','Friday','Saturday','Sunday'],
		  eventtype:['Gebet','Gottesdienst','Lobpreis','Versammlung','Hauskreis'],
		imageName:'',
		myaudiovediofile:'',
		instagramTokenvalue:'',
		subID:'',
	  isChurchetypeSelected:false,
	  isChurchesubtypeSelected:false,
	  churchesubsubCategories:'',
      ifPackagesvaluetrue: false,
      isshowAngebote: true,
      isNight: false,
      chips: [],
      chipslanguage: [],
      themenItems: [],
      languageItems: [],
      ChurcheCategories: [],
      churchesubCategories: [],
      ofLessonsItems: ['Vollzeit', 'Teilzeit', 'Geringfügig', 'Ehrenamtlich'],
      languages: [
        'Französisch',
        'Deutsch',
        'Chinesisch',
        'Ungarisch',
        'Persisch',
        'Englisch',
        'Arabisch',
        'Französisch',
        'Portugiesisch',
        'Rumänisch',
        'Koreanisch'
      ],
      visitorRangItems: [],
      ImagecounttoShow: 6,
      isLoading: false,
      fullPage: true,
      modal2: false,
      modal3: false,
	  modaltime2: false,
      modaltime3: false,
      EventTimeStatrModal: false,
      EventTimeCloseModal: false,
      EventDatePickerMOdal: false,
      JobDatePickerModal: false,
      imageUrl: '',
      logochurche: '',
      imageFile: '',
      tab: null,
      images: [],
      index: null,
      panel: [0, 1],
      sameaddressforoffice: true,
      userRole: localStorage.getItem('userRole')
        ? localStorage.getItem('userRole')
        : 1,
      churcheslogo: '',
	  churchesubsubCategory2:'',
      bannerLogoHere: '',
	  editEventsDialog: null,
	  editDialognewsroom: null,
	  editnewaudiovedio:null,
	  addJobsDialogbox: null,
      rating: 0,
	  name:'',
	  lat:'',
	  lng:'',
	  latstreet:'',
	  lngstreet:'',
	  visiterrange: [110, 440],
      // baseUrl: process.env.BASE_URL
      isMobile: false,
      drawer: null,
	  fchurche:'',
	  schurche:'',
	  sschurche:'',
      churchesData: [],
      active_tab: 'tab-1',
      searchLoading: false,
      churcheTitle: [],
      countryItems: [],
      cityItems: [],
	  cityofficeItems: [],
      StateItems: [],
	  StateofficeItems: [],
      MunicipalityItems: [],
	  MunicipalityofficeItems: [],
      search: null,
      sharedialog: false,
      imageData: '',
      imageFIle: '',
      ImageUrls: '',
      BannerimageData: '',
	  uploadimagevideos: '',
      socialUrl:
        'http://dev.woobii.com/admin/Kirchenfinder/' + this.$route.params.slug,
      socialTitle: 'Woobii',
      socialDescription: 'Woobii',
      socialQuote: 'Woobii',
      editDialog: false,
      editDialogdesc: false,
      editDialoglogo: false,
      OpenEditBannerDialog: false,
      editPrimaryThings: false,
      dialogLoader: false,
      datemodal: false,
      datemodalI: false,
      editBannerDialog: false,
      editBannerDialogadvance: false,
      editDialogaddress: false,
	  addtimingmultiple:false,
      socialmediasFormDialog: false,
      imageBannerData: '',
      imageBannerDataadvance: '',
	  GemeindeDatepickerModal:false,
      date: '',
      items: ['Test', 'Test', 'Test'],
      checkbox: '',
      newsmeldung: '',
      subcategory: '',
      category: '',
      churcheid: '',
      aboutus: '',
      formnews: {
        id: '',
        newstitle: '',
        teaser: '',
        newsmeldung: '',
        ressort: '',
        kategorie: '',
        themen: '',
        veroffentlichung: '',
        inaktiv: '',
        date: '',
        bildtitel:'',
        beschreibung:'',
        bildrechte:'',
        lizenz: '',
        bannerimage: '',
        peoples: '',
        country: '',
        city: '',
        municipalty: '',
        chips: '',
		audiovedioupload:'',
		vediocheckbox:false,
		audiocheckbox:false,
		imagesourcelabel:'',
		imagesourcelink:'',
      },
	   formnewsadvance: {
        id: '',
        newstitle: '',
        teaser: '',
        newsmeldung: '',
        ressort: '',
        kategorie: '',
        themen: '',
        veroffentlichung: '',
        inaktiv: '',
        date: '',
        bildtitel:'',
        beschreibung:'',
        bildrechte:'',
        lizenz: '',
        bannerimage: '',
        peoples: '',
        country: '',
        city: '',
        municipalty: '',
        chips: '',
		audiovedioupload:'',
		audiovediouploadhidden:'',
		vediocheckbox:false,
		audiocheckbox:false,
		vediocheckbox:false,
		audiocheckbox:false,
      },
      primaryForm: {
        id: '',
        comm_name: '',
        country: '',
        visitorranges: '',
        chipslanguage: '',
        city: '',
        languages: '',
        churchecategory: '',
        churchesubCategory: '',
		range:''
      },
	  videoForm: {
        id: '',
        videoUrl: '',
        title: '',
        poster: '',
        youtubeparameter: ''
      },
	  timingform:{
		  eventdescription:'',
		 timingOffertDate: new Date().toISOString().substr(0, 10),
		  celbrationLocation:'',
        address: '',
		fromtime: null,
        totime:null
	  },
      form2: {
        address: '',
        addressRules: [v => !!v || 'Address is required'],
        country: '',
        city: '',
		StateID:'',
		municipalityID:'',
        email: '',
        emailRules: [
          v => !!v || 'E-mail is required',
          v =>
            /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) ||
            'E-mail must be valid'
        ],
        contactno: '',
        mobileRules: [v => !!v || 'Contact No is required'],
        headoffice: '',
		lati:'',
		longi:'',
        website: '',
        lattitude: '',
        longitude: '',
        celbrationLocation: '',
        address1: '',
        pincode: '',
        officeaddress: '',
        officepincode: '',
        emailid: '',
        head_office_address: '',
        head_office_email: '',
        head_office_lat: '',
        head_office_lng: '',
		headoffice_city:'',
		headoffice_municiplity:'',
		headofficeCountry:'',
		head_office_StateID:'',
		headoffice_pincode:'',
        starttimeof: null,
        closetime: null
      },
	  EventForm: {
		  EventAddress: '',
		  EventCloseTime: null,
		  EventTimeStatrTime: null,
		  EventTitle: '',
		  EventAddress: '',
		  EventPincode: '',
		  Eventlattitude: '',
		  Eventlongitude: '',
		  EventDescription: '',
		  StateID: '',
		  municipaltyID: '',
		  EventDatePicker: new Date().toISOString().substr(0, 10),
		  EventDatePickerto: new Date().toISOString().substr(0, 10),
		  country:'',
		  city:''
	  },
	  JobsForm: {
		  JobId: '',
		  JobsLattitude: '',
		  JobsLongitude: null,
		  JobDescription: null,
		  JobPosition: null,
		  JobAddress: '',
		  JobEmail: '',
		  ofLessons: '',
		  JobOffertDate: new Date().toISOString().substr(0, 10),
		  city:'',
		  country:'',
		  StateID:'',
		  municipaltyID:'',
		  pincode:'',
	  },
	  formSocailIds: {
		  facebookID: '',
		  facebookToken: '',
		  instagramID: '',
		  twitterID: '',
		  pinterestID: '',
		  youtubeID: '',
	  },
	  packageConditions: {
		  IsBasicProfile:false,
		  isAdressPlzCity:false,
		  IsEmailWebaddress:false,
		  IsLinktoHome:false,
		  IsContact:false,
		  IsPhoneNumber:false,
		  IsCommunityDeals:false,
		  IsDescription:false,
		  IsUploadLogo:false,
		  isUploadHeaderPhoto:false,
		  IsintigrationOfChurchefinder:false,
		  IsNewsWithPicture:false,
		  IsUploadPhotos:false,
		  IsintigrationofSocialMedia:false,
		  IsNewsOnHomePage:false,
		  IsJobOffers:false,
		  IsEventCalander:false,
		  IsAddFreeProfile:false,
		  IsReasonalProfileApplication:false,
		  IsJobMarket:false,
		  IsEventExchange:false,
		  IsTopNews:false,
      },
    }
  },
  watch: {
    search (val) {
      if (val.length >= 3) {
        val && val !== this.select && this.querySelections(val)
      } else {
        this.churcheTitle = []
      }
    }
  },
  mounted () {
	  // alert(this.churchesData[0].churchetypeID);
	  this.listofgemeindegrobe();
    this.churchdata(this.$route.params.slug);
    this.listofcategory();
    this.listofsubcategory();
    this.listofuploadedFiles(this.$store.state.communityid);
    this.listofcountry();
    // this.listofStates();
    // this.listofcity();
    this.listofthemens();
    this.listoflanguages();
    // this.listofChurcheCategories();
    this.getIfpackagesDataTrue(this.$store.state.packageID, 8);
	// this.churchelogo();
	  // this.listOfChurcheSubCategories(this.primaryForm.churchecategory);
    // this.listofmuncipalty();
    // this.listofuploadedFiles(1509);
    // simulate AJAX
  },
  computed: {
	  
    userRoleCall () {
      if (this.$route.params.slug === this.$store.state.logincommunityslug) {
        return 2
      } else {
        return 1
      }
      // return this.$store.state.userRole;
    },
	 computeddatetime() {
      return this.formatDate(this.timingform.timingOffertDate)
    },
    computedDateFormatted () {
      return this.formatDate(this.JobsForm.JobOffertDate)
    },
	  ComputedEvent () {
		  return this.formatDate(this.EventForm.EventDatePicker)
	  }
  },
  beforeDestroy () {},
  methods: {
	  
	   deletetiming: function (timingid) {
      let e = this
      // this.isLoading = true;
      if (confirm('are you sure?')) {
        axios
          .delete('/churcheview/deletetiming/?id=' + timingid)
          .then(function(response) {        
            if (response.status == 200) {
			 e.churchdata(e.$route.params.slug)
			 e.$toasted.success('Successfully Deleted', {
                position: 'top-center',
                duration: 1000
              })
            } else {
              alert('Something went worng at this time')
            }
          })
      }
    },
	  saveMultipletiminghere: function(){
		 this.isLoading = true
      let e = this
      axios
        .post('/churcheview/saveMultipletiminghere', {
          id: this.churcheid,
		 // JobId: this.JobsForm.JobId,
          weekdays: this.timingform.weekdays,
          eventtype: this.timingform.eventtype,
          Frequency: this.timingform.Frequency,
          eventdescription: this.timingform.eventdescription,
		  timingOffertDate:this.timingform.timingOffertDate,
		  celbrationLocation: this.timingform.celbrationLocation,
		  address: this.timingform.address,
        fromtime: this.timingform.fromtime,
		totime: this.timingform.totime,
         
        })
        .then(function (response) {
          e.isLoading = false
		 // alert(response.data.status);
           e.addtimingmultiple = false
          if (response.data.status == true) {
            e.churchdata(e.$route.params.slug)
            e.$toasted.success('Erfolgreich aktualisiert', {
              position: 'top-center',
              duration: 2000
            })
          } else {
            e.$toasted.error('Please try after sometime', {
              position: 'top-center',
              duration: 2000
            })
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
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            console.log(error.request)
          } else {
            // Something happened in setting up the request that triggered an Error
            console.log('Error', error.message)
          }
        })  
	  },
	  
	  
	   
	    addtimingform: function () {
      var self = this
	   self.timingform.eventdescription = ''
      self.timingform.Frequency = ''
      self.timingform.GemeindeDatePicker = ''
	  self.timingform.weekdays = ''
      self.timingform.eventtype = ''
      self.timingform.celbrationLocation =''
      self.timingform.address = ''
	  
 },
	   onFilePicked(e) {
		 console.log(e.target.value);
      const files = e.target.files;
      if (files[0] !== undefined) {
        this.formnewsadvance.audiovedioupload = files[0].name;
        if (this.formnewsadvance.audiovedioupload.lastIndexOf(".") <= 0) {
          return;
        }
        const fr = new FileReader();
        fr.readAsDataURL(files[0]);
        fr.addEventListener("load", () => {
          this.imageUrl = fr.result;
          this.formnewsadvance.myaudiovediofile = files[0]; // this is an image file that can be sent to server...
		  console.log(this.formnewsadvance.myaudiovediofile);
        });
      } else {
        this.audiovedioupload.imageName = "";
       
        this.imageUrl = "";
      }
    },
	pickFile() {
      this.$refs.image.click();
    },
	  saveLogo(e) {
      const file = this.$refs.file.files[0];
      
      if (!file) {
        e.preventDefault();
        alert('No file chosen');
        return;
      }
      
      if (file.size > 1024 * 1024) {
        e.preventDefault();
        alert('File too big (> 1MB)');
        return;
      }
      
      alert('File OK');
    },
    imageBannerDataChange: function () {},
    addressService: function () {
      this.listofcountry();
      this.listofcity(this.form2.StateID);
      this.listofmuncipalty(this.form2.city);
	  this.listofofficecity(this.form2.head_office_StateID)
	  this.listofofficeStates(this.form2.headofficeCountry)
	  this.listofofficemuncipalty(this.form2.headoffice_city)
    },
    saveNewsroom: function () {
      // this.dialogLoader = true;
	  // this.isLoading = true;
	  // console.log(this.formnews); return false;
      let e = this
      axios
        .post('/churcheview/newsroomupdate', {
          id: this.formnews.id,
          comm_id: this.churcheid,
          newstitle: this.formnews.newstitle,
          teaser: this.formnews.teaser,
          newsmeldung: this.formnews.newsmeldung,
          ressort: this.formnews.ressort,
          kategorie: this.formnews.kategorie,
          // themen: this.formnews.themen,
          veroffentlichung: this.formnews.veroffentlichung,
          inaktiv: this.formnews.inaktiv,
          date: this.formnews.date,
          bildtitel: this.formnews.bildtitel,
          beschreibung: this.formnews.beschreibung,
          bildrechte: this.formnews.bildrechte,
          lizenz: this.formnews.lizenz,
          bannerimage: this.formnews.bannerimage,
		  
		  
          // peoples: this.formnews.peoples,
          // country: this.formnews.country,
          // city: this.formnews.city,
          // municipalty: this.formnews.municipalty,
          chips: this.formnews.chips,
		  imagesourcelabel:this.formnews.imagesourcelabel,
		  imagesourcelink:this.formnews.imagesourcelink,
        })
        .then(function (response) {
          // this.isLoading = false;
          e.editDialog = false
          if (response.data.status == true) {
            e.churchdata(e.$route.params.slug)
            e.$toasted.success('Erfolgreich aktualisiert', {
              position: 'top-center',
              duration: 2000
            })
          } else {
            e.$toasted.error('Please try after sometime', {
              position: 'top-center',
              duration: 2000
            })
          }
        })
        .catch(function (error) {
          if (error.response) {
            // The request was made and the server responded with a status code
            // that falls out of the range of 2xx
            // console.log(error.response.data)
            // console.log(error.response.status)
            // console.log(error.response.headers)
          } else if (error.request) {
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            // console.log(error.request)
          } else {
            // Something happened in setting up the request that triggered an Error
            // console.log('Error', error.message)
          }
        })
    },
	saveNewsroomadvance: function () {
		var e = this;
	 	//console.log(this.formnews.chips);
		// e.preventDefault();
		let approvers = [];
		if(this.formnewsadvance.chips){
		this.formnewsadvance.chips.forEach((user) => {
			 approvers.push(user.ID);
		});
		this.approvers = approvers;
		}
	  var bodyFormData = new FormData();
        bodyFormData.set('id', this.formnewsadvance.id);
        bodyFormData.set('comm_id', this.churcheid);
        bodyFormData.set('newstitle', this.formnewsadvance.newstitle);
		 bodyFormData.set('teaser', this.formnewsadvance.teaser);
		  bodyFormData.set('newsmeldung',this.formnewsadvance.newsmeldung);
		bodyFormData.set('ressort',this.formnewsadvance.ressort);
		bodyFormData.set('kategorie',this.formnewsadvance.kategorie);
		bodyFormData.set('veroffentlichung',this.formnewsadvance.veroffentlichung);
		bodyFormData.set('inaktiv', this.formnewsadvance.inaktiv);
		bodyFormData.set('date', this.formnewsadvance.date);
	
		bodyFormData.set('bildtitel', this.formnewsadvance.bildtitel);
		bodyFormData.set('beschreibung', this.formnewsadvance.beschreibung);
		bodyFormData.set('bildrechte', this.formnewsadvance.bildrechte);
		bodyFormData.set('lizenz', this.formnewsadvance.lizenz);
		bodyFormData.set('bannerimage', this.formnewsadvance.bannerimage);
		bodyFormData.set('chips',this.approvers);
		bodyFormData.set('imagesourcelabel', this.formnewsadvance.imagesourcelabel);
		bodyFormData.set('imagesourcelink', this.formnewsadvance.imagesourcelink);
		bodyFormData.set('myaudiovediofile',this.formnewsadvance.myaudiovediofile);
		bodyFormData.set('audiocheckbox',this.formnewsadvance.audiocheckbox);
		
		bodyFormData.set('vediocheckbox',this.formnewsadvance.vediocheckbox);
		bodyFormData.set('audiovediouploadhidden',this.formnewsadvance.audiovediouploadhidden);
		//console.log(bodyFormData); return false
		axios({
                method: 'post',
                url: "/churcheview/newsroomupdateadvance",
                data: bodyFormData,
                config: {
                    headers: {
                        'Content-Type': 'multipart/form-data'
                    }
                }
		})
        .then(function (response) {
          // this.isLoading = false;
          e.editDialog = false
          if (response.data.status == true) {
            e.churchdata(e.$route.params.slug)
            e.$toasted.success('Erfolgreich aktualisiert', {
              position: 'top-center',
              duration: 2000
            })
          } else {
            e.$toasted.error('Please try after sometime', {
              position: 'top-center',
              duration: 2000
            })
          }
        })
    },
	listofgemeindegrobe:function()
	{
		var e = this
      axios
        .get('/adminglobal/getallgemeindegroberanges')
        .then(function (response) {
          if (response.data.status == true) {
            e.visitorRangItems = response.data.gemeindegrobes.gemeindegrobe;
			console.log(e.visitorRangItems);
          }
        })	
		
	},
	
	
    saveLogo: function () {
      this.isLoading = true
      let e = this
      axios
        .post('/churcheview/logoupdate', {
          id: this.churcheid,
          logo: this.imageData
        })
        .then(function (response) {
          e.isLoading = false
          e.editDialoglogo = false
          if (response.data.status == true) {
            // e.churcheslogo = ''
            e.churchdata(e.$route.params.slug)
            e.$toasted.success('Erfolgreich aktualisiert', {
              position: 'top-center',
              duration: 2000
            })
          } else {
            e.$toasted.error('Please try after sometime', {
              position: 'top-center',
              duration: 2000
            })
          }
        })
        .catch(function (error) {
          if (error.response) {
            // The request was made and the server responded with a status code
            // that falls out of the range of 2xx
            // console.log(error.response.data)
            // console.log(error.response.status)
            // console.log(error.response.headers)
          } else if (error.request) {
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            // console.log(error.request)
          } else {
            // Something happened in setting up the request that triggered an Error
            // console.log('Error', error.message)
          }
        })
    },
    saveAboutUs: function () {
      this.isLoading = true
      let e = this
      axios
        .post('/churcheview/aboutusupdate', {
          id: this.churcheid,
          aboutus: this.aboutus
        })
        .then(function (response) {
          e.isLoading = false
          e.editDialogdesc = false
          if (response.data.status == true) {
            e.churchdata(e.$route.params.slug)
            e.$toasted.success('Erfolgreich aktualisiert', {
              position: 'top-center',
              duration: 2000
            })
          } else {
            e.$toasted.error('Please try after sometime', {
              position: 'top-center',
              duration: 2000
            })
          }
        })
        .catch(function (error) {
          if (error.response) {
            // The request was made and the server responded with a status code
            // that falls out of the range of 2xx
            // console.log(error.response.data)
            // console.log(error.response.status)
            // console.log(error.response.headers)
          } else if (error.request) {
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            // console.log(error.request)
          } else {
            // Something happened in setting up the request that triggered an Error
            // console.log('Error', error.message)
          }
        })
    },
    newsroomdetail: function (id) {
      var self = this
      this.searchLoading = true
      // Simulated ajax query
      axios
        .get('/churcheview/newsroombyid?r=' + id)
        .then(function (response) {
          if (response.data.status == true) {
			  // console.log(response.data.themen)
            self.formnews.id = response.data.newsroom[0].newsroom_id
            self.formnews.newstitle = response.data.newsroom[0].nr_title
            self.formnews.teaser = response.data.newsroom[0].nr_short_desc
            self.formnews.newsmeldung = response.data.newsroom[0].nr_description
            self.formnews.ressort = response.data.newsroom[0].nr_category
            self.listofsubcategory(self.formnews.ressort)
            self.formnews.kategorie = response.data.newsroom[0].nr_subcategory
            self.formnews.themen = response.data.newsroom[0].subjects
            self.formnews.veroffentlichung = response.data.newsroom[0].publication
            self.formnews.inaktiv = response.data.newsroom[0].inactive
            self.formnews.bannerimage = response.data.newsroom[0].nr_banner
            self.formnews.date = response.data.newsroom[0].nr_banner
              .split('/')
              .pop()
            self.formnews.bildtitel = response.data.newsroom[0].image_title
            self.formnews.beschreibung =
              response.data.newsroom[0].image_description
            self.formnews.bildrechte = response.data.newsroom[0].image_rights
            self.formnews.lizenz = response.data.newsroom[0].license
            self.formnews.country = response.data.newsroom[0].nr_country_id
            self.formnews.city = response.data.newsroom[0].nr_city_id
            self.formnews.municipalty = response.data.newsroom[0].nr_municipalty_id
            self.formnews.peoples = response.data.newsroom[0].peoples
			self.formnews.imagesourcelabel = response.data.newsroom[0].imagesourcelabel
			self.formnews.imagesourcelink = response.data.newsroom[0].imagesourcelink
            // self.formnews.chips = response.data.newsroom[0].peoples;
            self.formnews.chips = response.data.themen
          }
        })
    },
	newsroomdetailadvance: function (id) {
      var self = this
      this.searchLoading = true
      // Simulated ajax query
	  console.log(self.formnews.audiocheckbox);
      axios
        .get('/churcheview/newsroombyidadvance?r=' + id)
        .then(function (response) {
          if (response.data.status == true) {
			   console.log(response.data.themen)
            self.formnewsadvance.id = response.data.newsroom[0].newsroom_id
            self.formnewsadvance.newstitle = response.data.newsroom[0].nr_title
            self.formnewsadvance.teaser = response.data.newsroom[0].nr_short_desc
            self.formnewsadvance.newsmeldung = response.data.newsroom[0].nr_description
            self.formnewsadvance.ressort = response.data.newsroom[0].nr_category
            self.listofsubcategory(self.formnews.ressort)
            self.formnewsadvance.kategorie = response.data.newsroom[0].nr_subcategory
            self.formnewsadvance.themen = response.data.newsroom[0].subjects
            self.formnewsadvance.veroffentlichung = response.data.newsroom[0].publication
            self.formnewsadvance.inaktiv = response.data.newsroom[0].inactive
            self.formnewsadvance.bannerimage = response.data.newsroom[0].nr_banner
            self.formnewsadvance.date = response.data.newsroom[0].nr_banner
              .split('/')
              .pop()
            self.formnewsadvance.bildtitel = response.data.newsroom[0].image_title
            self.formnewsadvance.beschreibung = response.data.newsroom[0].image_description
            self.formnewsadvance.bildrechte = response.data.newsroom[0].image_rights
            self.formnewsadvance.lizenz = response.data.newsroom[0].license
            self.formnewsadvance.country = response.data.newsroom[0].nr_country_id
            self.formnewsadvance.city = response.data.newsroom[0].nr_city_id
            self.formnewsadvance.municipalty = response.data.newsroom[0].nr_municipalty_id
            self.formnewsadvance.peoples = response.data.newsroom[0].peoples
			self.formnewsadvance.audiocheckbox= response.data.newsroom[0].IsAudio	 	  
			self.formnewsadvance.vediocheckbox = response.data.newsroom[0].IsVideo		  
			self.formnewsadvance.audiovedioupload = response.data.newsroom[0].AudioVideoUrl
			self.formnewsadvance.audiovediouploadhidden = response.data.newsroom[0].AudioVideoUrl
			self.formnewsadvance.imagesourcelabel = response.data.newsroom[0].imagesourcelabel
			self.formnewsadvance.imagesourcelink = response.data.newsroom[0].imagesourcelink
            // self.formnews.chips = response.data.newsroom[0].peoples;
            self.formnewsadvance.chips = response.data.themen
          }
        })
    },
    addjobsform: function () {
      var self = this
	   self.JobsForm.JobId = ''
      self.JobsForm.JobDescription = ''
      self.JobsForm.JobsLattitude = ''
	  self.JobsForm.JobsLongitude = ''
      self.JobsForm.JobPosition = ''
      self.JobsForm.JobAddress = this.churchesData[0].Address1
      self.JobsForm.JobEmail = ''
      self.JobsForm.JobOffertDate = ''
      self.JobsForm.ofLessons = ''
	  self.JobsForm.pincode = this.churchesData[0].Pincode
	  self.JobsForm.city = this.churchesData[0].city_id
	 self.JobsForm.country = this.churchesData[0].country_id
		self.JobsForm.StateID = this.churchesData[0].StateID
	 self.JobsForm.municipaltyID = this.churchesData[0].municipalty_id
	 
          

   },
   addeventsform:function(){   
	  
	   var self = this
	   self.EventForm.EventAddress = ''
     //self.EventForm.EventCloseTime = ''
     // self.EventForm.EventTimeStatrTime = ''
	  self.EventForm.EventTitle = ''
      self.EventForm.EventAddress = ''
	  
	  self.EventForm.EventPincode = ''
      self.EventForm.Eventlattitude = ''
	  
	  self.EventForm.Eventlongitude = ''
      self.EventForm.EventDescription = ''
	  self.EventForm.StateID = ''
      self.EventForm.municipaltyID = ''
	  self.EventForm.city = ''
	  self.EventForm.country = ''
	  
	  
   },
    addnewsroomform: function () {
      var self = this
	  self.formnews.id = ''
	  self.formnews.newstitle = ''
	  self.formnews.teaser = ''
	  self.formnews.newsmeldung = ''
	  self.formnews.ressort = ''
	  self.formnews.kategorie = ''
      self.formnews.veroffentlichung = ''
	  self.formnews.inaktiv = ''
	  self.formnews.date = ''
	  self.formnews.bildtitel = ''
	  self.formnews.lizenz = ''
	  self.formnews.bannerimage = ''
	  self.formnews.peoples = ''
	  self.formnews.country = ''
	  self.formnews.city = ''
	  self.formnews.municipalty = ''
	  self.formnews.chips = ''
	  self.formnews.beschreibung=''
	  self.formnews.imageBannerData=''
	  self.formnews.bildrechte=''
	  self.formnews.imagesourcelabel=''
	  self.formnews.imagesourcelink=''
	  
    },
	addnewsroomformadvance: function () {
      var self = this
	  self.formnewsadvance.id = ''
	  self.formnewsadvance.newstitle = ''
	  self.formnewsadvance.teaser = ''
	  self.formnewsadvance.newsmeldung = ''
	  self.formnewsadvance.ressort = ''
	  self.formnewsadvance.kategorie = ''
      self.formnewsadvance.veroffentlichung = ''
	  self.formnewsadvance.inaktiv = ''
	  self.formnewsadvance.date = ''
	  self.formnewsadvance.bildtitel = ''
	  self.formnewsadvance.lizenz = ''
	  self.formnewsadvance.bannerimage = ''
	  self.formnewsadvance.peoples = ''
	  self.formnewsadvance.country = ''
	  self.formnewsadvance.city = ''
	  self.formnewsadvance.municipalty = ''
	  self.formnewsadvance.chips = ''
	  self.formnewsadvance.beschreibung=''
	  self.formnewsadvance.bildrechte=''
	  self.formnewsadvance.audiovedioupload=''
	  self.formnewsadvance.audiocheckbox=''
	  self.formnews.imageBannerDataadvance=''
	  self.formnewsadvance.vediocheckbox=''
	  self.formnewsadvance.imagesourcelabel=''
	  self.formnewsadvance.imagesourcelink=''
	  
    },
    getjobdetail: function (id) {
	 var self = this
      this.searchLoading = true
      // Simulated ajax query
      axios
        .get('/churcheview/myjobbyid?r=' + id)
        .then(function (response) {
          if (response.data.status == true) {
			  // console.log(response.data.jobs[0])
            self.JobsForm.JobId = response.data.jobs[0].id

            self.JobsForm.JobDescription = response.data.jobs[0].description
            self.JobsForm.JobsLattitude = response.data.jobs[0].jobLatt
			 self.JobsForm.JobsLongitude = response.data.jobs[0].JobLng
            self.JobsForm.JobPosition = response.data.jobs[0].comm_title
            self.JobsForm.JobAddress = response.data.jobs[0].Address
            self.JobsForm.JobEmail = response.data.jobs[0].Email
            self.JobsForm.JobOffertDate = response.data.jobs[0].date
            self.JobsForm.ofLessons = response.data.jobs[0].Lesson
            self.JobsForm.country = response.data.jobs[0].countryID
            self.JobsForm.city = response.data.jobs[0].City
            self.JobsForm.municipaltyID = response.data.jobs[0].municipalityID
            self.JobsForm.StateID = response.data.jobs[0].StateID
            self.JobsForm.pincode = response.data.jobs[0].Pincode
          }
        })
    },
    categorySelected: function (event) {
      this.listofsubcategory(event)
    },
    pageRedirect: function (event) {
      this.$router.push('/Kirchenfinder/' + event)
      this.churchdata(this.$route.params.slug)
    },
    splitTag: function (value) {
      if (!value) return ''
      value = value.split(',')
      return value
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
            self.socialTitle = churchesData[0].title
            self.socialDescription = churchesData[0].title
            self.socialQuote = churchesData[0].title
            self.searchLoading = false
          }
        })
    },
    churchdata: function (slug) {
				var e = this
				axios
				.get('/churcheview/churche?s=' + slug)
				.then(function (response) {
				if (response.data.status == true) {

				e.churchesData = response.data.churche
				e.getgeolocationstreet(e.churchesData[0].address+' '+e.churchesData[0].city);
				console.log(e.churchesData[0])
				// alert('country ID = '+ e.churchesData[0].country_id);
				// alert('State ID = '+ e.churchesData[0].StateID);
				// alert('City ID = '+ e.churchesData[0].city_id);
				// alert('Municipality ID = '+e.churchesData[0].municipalty_id);
				e.listofStates(e.churchesData[0].country_id);
				e.listofChurcheCategories(e.churchesData[0].country_id);
				// e.listofcity(e.churchesData[0].StateID);
				e.churcheid = e.churchesData[0].id
				e.churcheslogo = e.churchesData[0].logo
				e.instagramTokenvalue = e.churchesData[0].social_instagram
				e.bannerLogoHere = e.churchesData[0].bannerimage
				e.aboutus = e.churchesData[0].about_us
				e.rating = e.churchesData[0].rating
				e.form2.country = e.churchesData[0].country_id
				e.form2.city = e.churchesData[0].city_id
				e.form2.StateID = e.churchesData[0].StateID
				e.form2.municipalityID = e.churchesData[0].municipalty_id
				e.form2.contactno = e.churchesData[0].contact_no
				e.form2.website = e.churchesData[0].website
				e.form2.lattitude = e.churchesData[0].lat
				e.form2.longitude = e.churchesData[0].lng
				e.form2.starttimeof = e.churchesData[0].timing.split(' - ')[0]
				e.form2.closetime = e.churchesData[0].timing.split(' - ')[1]
				e.form2.celbrationLocation = e.churchesData[0].celbrationLocation
				e.form2.address1 = e.churchesData[0].Address1
				e.form2.pincode = e.churchesData[0].Pincode
				e.form2.emailid = e.churchesData[0].email_id
				e.form2.head_office_email = e.churchesData[0].head_office_email
				e.form2.head_office_address = e.churchesData[0].head_office_address
				e.form2.headoffice_pincode = e.churchesData[0].office_pincode
				e.form2.head_office_StateID = e.churchesData[0].head_office_StateID
				e.form2.headofficeCountry = e.churchesData[0].office_country_id
				e.form2.headoffice_municiplity = e.churchesData[0].headoffice_municiplity
				e.form2.headoffice_city = e.churchesData[0].office_city_id
				e.form2.head_office_lat = e.churchesData[0].head_office_lat
				e.form2.head_office_lng = e.churchesData[0].head_office_lng
				e.form2.headoffice = e.churchesData[0].head_office
				e.primaryForm.comm_name = e.churchesData[0].title
				e.primaryForm.visitorranges = e.churchesData[0].visitor_range
				e.primaryForm.ranges = e.churchesData[0].Ranges
				e.primaryForm.languages = e.churchesData[0].langauages
				e.primaryForm.chipslanguage = e.churchesData.languagesfrom
				e.primaryForm.churchecategory = e.churchesData[0].churchetypeID
				if(!e.churchesData[0].churchetypeID){
				e.primaryForm.churchecategorytwo = 0;
				}else{
				e.primaryForm.churchecategorytwo = e.churchesData[0].churchetypeID
				}
				e.primaryForm.logochurche = e.churchesData[0].churchelogo
				e.primaryForm.logosubchurche = e.churchesData[0].churchesublogo
				e.primaryForm.logosubsubchurche = e.churchesData[0].churchesubsublogo
				e.primaryForm.churchesubCategory = e.churchesData[0].churchesubcatID
				if(!e.churchesData[0].churchesubcatID){
				e.primaryForm.churchesubCategorytwo = 0;
				}else{
				e.primaryForm.churchesubCategorytwo = e.churchesData[0].churchesubcatID
				}
				e.listOfChurcheSubSubCategories(e.primaryForm.churchesubCategory)
				e.primaryForm.churchesubsubCategory = e.churchesData[0].churchesubsubcatID
				if(!e.churchesData[0].churchesubsubcatID){
				e.primaryForm.churchesubsubCategorytwo = 0;
				}else{
				e.primaryForm.churchesubsubCategorytwo = e.churchesData[0].churchesubsubcatID
				}
				e.churchesubsubCategory2 = e.churchesData[0].churchesubsubcatID
				e.formSocailIds.instagramID = e.churchesData[0].social_instagram
				e.formSocailIds.facebookID = e.churchesData[0].social_fb
				e.formSocailIds.twitterID = e.churchesData[0].social_twitter
				e.formSocailIds.youtubeID = e.churchesData[0].social_youtube
				e.formSocailIds.pinterestID = e.churchesData[0].social_pinterest
				e.formSocailIds.facebookToken = e.churchesData[0].facebookToken


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
    listofcountry: function () {
      var e = this
      axios
        .get('/adminglobal/getallcountry')
        .then(function (response) {
          if (response.data.status == true) {
            e.countryItems = response.data.countries.country
          }
        })
    },
    listofStates: function (countryid) {
      var e = this
      axios
        .get('/adminglobal/getAllStates/?id=' + countryid)
        .then(function (response) {
          if (response.data.status == true) {
            e.StateItems = response.data.states.state
            // e.dialogLoader = false;
          }
        });
    },
	 listofofficeStates: function (countryid) {
      var e = this
      axios
        .get('/adminglobal/getAllStates/?id=' + countryid)
        .then(function (response) {
          if (response.data.status == true) {
            e.StateofficeItems = response.data.states.state
           e.dialogLoader = false;
          }
        });
    },
    listofcity: function (stateid) {
      var e = this
      axios
        .get('/adminglobal/getallcity/?id=' + stateid)
        .then(function (response) {
          if (response.data.status == true) {
            e.cityItems = response.data.cities.city
            // e.dialogLoader = false;
          }
        })
    },
	listofofficecity: function (stateid) {
      var e = this
      axios
        .get('/adminglobal/getallcity/?id=' + stateid)
        .then(function (response) {
          if (response.data.status == true) {
            e.cityofficeItems = response.data.cities.city;
           e.dialogLoader = false;
          }
        })
    },
    listofmuncipalty: function (id) {
      var e = this
      axios
        .get('/adminglobal/getallmuncipalitytype?cityid=' + id)
        .then(function (response) {
          e.dialogLoader = false
          if (response.data.status == true) {
            e.MunicipalityItems = response.data.allmuncipalitytype
            // console.log(e.MunicipalityItems);
          }
        })
    },
	listofofficemuncipalty: function (id) {
		// console.log(id.place);
		this.name = id.place;
		this.getgeolocation(this.name);
      var e = this
      axios
        .get('/adminglobal/getallmuncipalitytype?cityid=' + id)
        .then(function (response) {
          e.dialogLoader = false
          if (response.data.status == true) {
            e.MunicipalityofficeItems = response.data.allmuncipalitytype
           // console.log(e.MunicipalityItems);
          }
        })
    },
    saveBannerImage: function () {
      this.isLoading = true
      let e = this
      axios
        .post('/churcheview/changebannerimage', {
          id: this.churcheid,
          f_image: this.BannerimageData
        })
        .then(function (response) {
          // console.log(response.data)
          e.isLoading = false
          e.OpenEditBannerDialog = false
          // if (response.data.status == true) {
          e.bannerLogoHere = ''
          e.churchdata(e.$route.params.slug)
          e.$toasted.success('Erfolgreich aktualisiert', {
            position: 'top-center',
            duration: 2000
          })
        })
        .catch(function (error) {
          if (error.response) {
            // The request was made and the server responded with a status code
            // that falls out of the range of 2xx
            // console.log(error.response.data)
            // console.log(error.response.status)
            // console.log(error.response.headers)
          } else if (error.request) {
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            // console.log(error.request)
          } else {
            // Something happened in setting up the request that triggered an Error
            // console.log('Error', error.message)
          }
        })
    },
	
	getgeolocationstreet: function (location) {
	//alert(location)
		var e = this
      axios
        .get('https://maps.googleapis.com/maps/api/geocode/json?address='+location+'&key=AIzaSyD-rdRygIIUU40ksaF8ZGZ1n8rfsMdM0xc')
        .then(function (response) {		
			e.latstreet = response.data.results[0].geometry.location.lat;
			e.lngstreet = response.data.results[0].geometry.location.lng;
			 //alert(e.latstreet);
			
        })	
},
	
	getgeolocation: function (location) {
		//alert(location)
		var e = this
      axios
        .get('https://maps.googleapis.com/maps/api/geocode/json?address='+location+'&key=AIzaSyD-rdRygIIUU40ksaF8ZGZ1n8rfsMdM0xc')
        .then(function (response) {		
			// e.center = response.data.results[0].geometry.location;
			// console.log(response.data.results);
			e.lat = response.data.results[0].geometry.location.lat;
			e.lng = response.data.results[0].geometry.location.lng;
			console.log(e.lat);
				// var bounds = new google.maps.LatLngBounds();
			// console.log(response.data.results[0].geometry.bounds.northeast);
			// console.log(response.data.results[0].geometry.bounds.southwest);
			// var bounds = new google.maps.LatLngBounds(response.data.results[0].geometry.bounds.southwest,response.data.results[0].geometry.bounds.northeast);
			// console.log(bounds);
			// bounds.extend(new google.maps.LatLng(response.data.results[0].geometry.location));
			// var zoomlevel = map.fitBounds(bounds);
          // if (response.data.status == true) {
            // e.communitySize = response.data.gemeindegrobes.gemeindegrobe
			// e.communitySize.unshift({visitor_range:"Alle Gemeindegröße", id:'000'});
          // }
        })	
		
		// // var e = this
		// // const geocoder = new google.maps.Geocoder();
		// var geocoder = require('geocoder');
		// // geocoder.geocode("Atlanta, GA", function ( err, data ) {
			// // console.log(data);
		  // // // do something with data
		// // });
		// var result = [];
		// geocoder.geocode( { 'address': location}, function(results, status) {
		// if (status == google.maps.GeocoderStatus.OK) {
		 // console.log(results);
         // result['lat'] = results[0].geometry.location.lat();
         // result['lng'] = results[0].geometry.location.lng();
		 // } else {
         // result = "Unable to find address: " + status;
     // }
	  // // e.center = result;
	 // console.log(result);
    // });
},
	
    saveAddress () {
      this.isLoading = true
      let e = this
      axios
        .post('/churcheview/addressupdate', {
          id: this.churcheid,
		   timing:
            this.form2.starttimeof != null
              ? this.form2.starttimeof + ' - ' + this.form2.closetime
              : '',
			  
          celbrationLocation: this.form2.celbrationLocation,		  
          email: this.form2.email,
          contactno: this.form2.contactno,
		  address1: this.form2.address1,
		  pincode: this.form2.pincode,
          country: this.form2.country,
          city: this.form2.city,
          StateID: this.form2.StateID,
          municipalityID: this.form2.municipalityID,
          comm_lat: this.form2.lattitude,
          comm_long: this.form2.longitude,
          website: this.form2.website,
		  emailid: this.form2.emailid,
          head_office_address: this.form2.head_office_address,		  
		  headoffice_pincode: this.form2.headoffice_pincode,
          head_office_email: this.form2.head_office_email,
          head_office_lat: this.form2.head_office_lat,
          head_office_lng: this.form2.head_office_lng,		  
		  headofficeCountry: this.form2.headofficeCountry,
		  headoffice_city: this.form2.headoffice_city.id,
		  head_office_StateID: this.form2.head_office_StateID,
		  headoffice_municiplity: this.form2.headoffice_municiplity,		  
          headoffice: this.form2.headoffice,		 
          lati: this.lat,		 
          longi: this.lng,		 
        })
        .then(function (response) {
          e.isLoading = false
          e.editDialogaddress = false
          if (response.data.status == true) {
            e.churchdata(e.$route.params.slug)
            e.$toasted.success('Erfolgreich aktualisiert', {
              position: 'top-center',
              duration: 2000
            })
          } else {
            e.$toasted.error('Please try after sometime', {
              position: 'top-center',
              duration: 2000
            })
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
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            console.log(error.request)
          } else {
            // Something happened in setting up the request that triggered an Error
            console.log('Error', error.message)
          }
        })
    },
    editPrimaryThingsBackend () {
      this.isLoading = true
      let e = this
	  // console.log(e)
      axios
        .post('/churcheview/primarythingsupdate', {
          id: this.churcheid,
          comm_name: this.primaryForm.comm_name,
          // country: this.primaryForm.country,
          // city: this.primaryForm.city,
          // visitor_range: this.visiterrange[0] + " - " + this.visiterrange[1],
          visitor_range: this.primaryForm.visitorranges,
          languages: this.primaryForm.languages,
		  newlanguage: this.primaryForm.chipslanguage,
		  churchecategory: this.primaryForm.churchecategory,
		  churchesubCategory: this.primaryForm.churchesubCategory,
		  churchesubsubCategories: this.primaryForm.churchesubsubCategory,
        })
        .then(function (response) {
          // console.log(response);
          e.isLoading = false
          e.editPrimaryThings = false
          if (response.data.status == true) {
            e.churchdata(e.$route.params.slug)
            e.$toasted.success('Erfolgreich aktualisiert', {
              position: 'top-center',
              duration: 2000
            })
          } else {
            e.$toasted.error('Please try after sometime', {
              position: 'top-center',
              duration: 2000
            })
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
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            console.log(error.request)
          } else {
            // Something happened in setting up the request that triggered an Error
            console.log('Error', error.message)
          }
        })
    },
    saveEventshere () {
      this.isLoading = true
      let e = this
      axios
        .post('/churcheview/saveEventsHere', {
          id: this.churcheid,
          address: this.EventForm.EventAddress,
          EventTitle: this.EventForm.EventTitle,
		  EventAddress: this.EventForm.EventAddress,
		  EventPincode: this.EventForm.EventPincode,
		  country: this.EventForm.country,
		  city: this.EventForm.city,
		  state: this.EventForm.StateID,
		  municipality: this.EventForm.municipaltyID,
          Eventlattitude: this.EventForm.Eventlattitude,
          Eventlongitude: this.EventForm.Eventlongitude,
          EventDescription: this.EventForm.EventDescription,
          EventDatePicker: this.EventForm.EventDatePicker,
          EventDatePickerto: this.EventForm.EventDatePickerto,
		  timing:
            this.EventForm.EventTimeStatrTime != null
              ? this.EventForm.EventTimeStatrTime + ' - ' + this.EventForm.EventCloseTime
              : ''
        })
        .then(function (response) {
          e.isLoading = false
          e.editEventsDialog = false
          if (response.data.status == true) {
            e.churchdata(e.$route.params.slug)
            e.$toasted.success('Erfolgreich aktualisiert', {
              position: 'top-center',
              duration: 2000
            })
          } else {
            e.$toasted.error('Please try after sometime', {
              position: 'top-center',
              duration: 2000
            })
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
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            console.log(error.request)
          } else {
            // Something happened in setting up the request that triggered an Error
            console.log('Error', error.message)
          }
        })
    },
    saveJobsHere () {
      this.isLoading = true
      let e = this
      axios
        .post('/churcheview/SaveJobsHere', {
          id: this.churcheid,
		  JobId: this.JobsForm.JobId,
          JobDescription: this.JobsForm.JobDescription,
          JobsLattitude: this.JobsForm.JobsLattitude,
          JobsLongitude: this.JobsForm.JobsLongitude,
          JobPosition: this.JobsForm.JobPosition,
		  pincode:this.JobsForm.pincode,
		  JobAddress: this.JobsForm.JobAddress,
		  JobEmail: this.JobsForm.JobEmail,
          JobOffertDate: this.JobsForm.JobOffertDate,
          ofLessons: this.JobsForm.ofLessons,
          imageFile: this.imageFIle,
          cityid: this.JobsForm.city,
          countryid: this.JobsForm.country,
          stateid: this.JobsForm.StateID,
          municipalityid: this.JobsForm.municipaltyID
        })
        .then(function (response) {
          e.isLoading = false
		  alert(response.data.status);
          // e.addJobsDialogbox = false
          if (response.data.status == true) {
            e.churchdata(e.$route.params.slug)
            e.$toasted.success('Erfolgreich aktualisiert', {
              position: 'top-center',
              duration: 2000
            })
          } else {
            e.$toasted.error('Please try after sometime', {
              position: 'top-center',
              duration: 2000
            })
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
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            console.log(error.request)
          } else {
            // Something happened in setting up the request that triggered an Error
            console.log('Error', error.message)
          }
        })
    },
    saveVideos () {
      this.isLoading = true
      let e = this
	  // console.log(e)
      axios
        .post('/churcheview/uploadvideoshere', {
          id: this.churcheid,
          videoUrl: this.videoForm.videoUrl,
          title: this.videoForm.title,
          youtubeparameter: this.videoForm.youtubeparameter
        })
        .then(function (response) {
          e.isLoading = false
          e.uploadimagevideos = false
          if (response.data.status == true) {
            e.listofuploadedFiles(e.$store.state.communityid)
            e.$toasted.success('Erfolgreich aktualisiert', {
              position: 'top-center',
              duration: 2000
            })
          } else {
            e.$toasted.error('Please try after sometime', {
              position: 'top-center',
              duration: 2000
            })
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
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            console.log(error.request)
          } else {
            // Something happened in setting up the request that triggered an Error
            console.log('Error', error.message)
          }
        })
    },
    saveImagesHere: function () {
      this.isLoading = true
      let e = this
      axios
        .post('/churcheview/saveImagesMultiple', {
          id: this.churcheid,
          imageurl: this.ImageUrls
        })
        .then(function (response) {
          e.isLoading = false
          e.uploadimagevideos = false
          if (response.data.status == true) {
			  e.listofuploadedFiles(e.$store.state.communityid)
            // e.churchdata(e.$route.params.slug);
            e.$toasted.success('Erfolgreich aktualisiert', {
              position: 'top-center',
              duration: 2000
            })
          } else {
            e.$toasted.error('Please try after sometime', {
              position: 'top-center',
              duration: 2000
            })
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
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            console.log(error.request)
          } else {
            // Something happened in setting up the request that triggered an Error
            console.log('Error', error.message)
          }
        })
    },
    listofuploadedFiles: function (churchid) {
      var e = this
      var imagess = []
      axios
        .get('/churcheview/listofuploadedFiles/?id=' + churchid)
        .then(function (response) {
          if (response.data.status == true) {
            // console.log(response.data.files); return false;
			  for (var i = 0; i < response.data.files.length; i++) {
              imagess.push({
                href: response.data.files[i].href,
                type: response.data.files[i].type,
                title: response.data.files[i].title,
                poster: response.data.files[i].poster,
                youtube: response.data.files[i].youtube,
                ID: response.data.files[i].ID
              })
            }
            e.images = imagess
          }
        })
    },
    handlechange: function () {
      this.isLoading = true
      this.isNight = true
      if (this.sameaddressforoffice == 'accepted') {
        // this.form2.head_office_address = '' + this.form2.address + ''
        // this.form2.headoffice = '' + this.form2.contactno + ''
        // this.form2.head_office_email = '' + this.form2.email + ''
        // this.form2.head_office_lat = '' + this.form2.lattitude + ''
        // this.form2.head_office_lng = '' + this.form2.longitude + ''
        // this.form2.officeaddress = '' + this.form2.officeaddress + ''
        this.isLoading = false
      } else {
        this.isNight = false
        // this.form2.head_office_address = ''
        // this.form2.headoffice = ''
        // this.form2.head_office_email = ''
        // this.form2.head_office_lat = ''
        // this.form2.head_office_lng = ''
        // this.form2.officeaddress = ''
        this.isLoading = false
      }
    },
    saveserviceforthis: function (offerid) {
	   this.isLoading = true
      let e = this
      axios
        .post('/churcheview/addmoreoffersforchurche', {
          id: this.churcheid,
          offerid: offerid
        })
        .then(function (response) {
          // console.log(response)
          e.isLoading = false
          e.editDialoglogo = false
          if (response.data.status == true) {
            e.churcheslogo = ''
            e.churchdata(e.$route.params.slug)
            e.$toasted.success('Erfolgreich aktualisiert', {
              position: 'top-center',
              duration: 2000
            })
          } else {
            e.$toasted.error('Already Addedd', {
              position: 'top-center',
              duration: 2000
            })
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
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            console.log(error.request)
          } else {
            // Something happened in setting up the request that triggered an Error
            console.log('Error', error.message)
          }
        })
    },
    deleteEventhere: function (eventid) {
      let e = this
      // this.isLoading = true;
      if (confirm('are you sure?')) {
        axios
          .delete('/churcheview/deleteEvent/?id=' + eventid)
          .then(function (response) {
          // this.isLoading = false;
            // console.log(response.status)
            if (response.status == 200) {
			 e.churchdata(e.$route.params.slug)
			 e.$toasted.success('Successfully Deleted', {
                position: 'top-center',
                duration: 1000
              })
            } else {
              alert('Something went worng at this time')
            }
          })
      }
    },
    deleteJobsHere: function (jobid) {
      let e = this
      // this.isLoading = true;
      if (confirm('are you sure?')) {
        axios
          .delete('/churcheview/deleteJobshere/?id=' + jobid)
          .then(function(response) {        
            if (response.status == 200) {
			 e.churchdata(e.$route.params.slug)
			 e.$toasted.success('Successfully Deleted', {
                position: 'top-center',
                duration: 1000
              })
            } else {
              alert('Something went worng at this time')
            }
          })
      }
    },
    deleteImageVideo: function (imageid) {
      let e = this      
      if (confirm('are you sure?')) {
        axios
          .delete('/churcheview/deleteimageVideows/?id=' + imageid)
          .then(function (response) {
				console.log(response);
            if (response.status == 200) {
			e.listofuploadedFiles(e.$store.state.communityid)
			 e.$toasted.success('Successfully Deleted', {
                position: 'top-center',
                duration: 1000
              })
            } else {
              alert('Something went worng at this time')
            }
          })
      }
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
    listoflanguages: function () {
      var e = this
      axios
        .get('/churcheview/listOflanguage')
        .then(function (response) {
          if (response.data.status == true) {
            e.languageItems = response.data.language
          }
        })
    },
    listofChurcheCategories: function (countryid) {
      var e = this
      axios
        .get('/churcheview/listofChurcheCategories?cid=' + countryid)
        .then(function (response) {
          if (response.data.status == true) {
            e.ChurcheCategories = response.data.categories;
			e.subID = e.ChurcheCategories[0].ID;
			e.listOfChurcheSubCategories(e.primaryForm.churchecategory);
			// e.logochurche = e.ChurcheCategories[0].Logo;
			// alert(e.$route.params.slug);
			e.churchelogo(e.primaryForm.churchecategory);
			if( !$("#churchesubtype").val() ) { 
			alert("adkasdg");
				e.isChurchesubtypeSelected = false;
			}
          }
        })
    },
    listOfChurcheSubCategories: function (ID) {
      var e = this
      axios
        .get('/churcheview/listOfChurcheSubCategories?id=' + ID)
        .then(function (response) {
          if (response.data.status == true) {
            e.churchesubCategories = response.data.categories;
				e.isChurchetypeSelected = true;
		  }else if (response.data.status == false) {
			   e.primaryForm.churchesubCategory =0;
			   e.primaryForm.churchesubsubCategory=0;
			   e.isChurchetypeSelected = false;
			   e.isChurchesubtypeSelected = false;
			   
		  }
        })
    },
	
	listOfChurcheSubSubCategories: function (ID) {
		
      var e = this
      axios
        .get('/churcheview/listOfChurcheSubSubCategories?id=' + ID)
        .then(function (response) {
          if (response.data.status == true) {
			  console.log(response.data);
            e.churchesubsubCategories = response.data.categories;
			e.isChurchesubtypeSelected = true;
          }else if(response.data.NEWStatus == false){
			  e.churchesubsubCategories = response.data.categories;
			  e.primaryForm.churchesubsubCategory=0;
			  e.isChurchesubtypeSelected = false;
		  }
        })
    },
	
    getIfpackagesDataTrue: function (packageid, pointid) {
      var e = this
	  var loginuserid = e.$store.state.loginUserId
      axios
        .get('/churcheview/getIfpackagesDataTrue?packageid=' + packageid + '&userId=' + loginuserid)
        .then(function (response) {
          if (response.data.status == true) {
			  // return true;
			  // e.isshowUberuns = true;
			  // console.log(response.data.packegesdata[0].point_id);
			  for (var i = 0; i <= response.data.packegesdata.length; i++) { 
			  if (response.data.packegesdata[i].point_id == 8) {
                e.packageConditions.IsDescription = true;
              } 
              if (response.data.packegesdata[i].point_id == 9) {
                e.packageConditions.IsUploadLogo = true;
              }
			  if (response.data.packegesdata[i].point_id == 10) {
                e.packageConditions.isUploadHeaderPhoto = true;
              } 
			  if (response.data.packegesdata[i].point_id == 13) {
                e.packageConditions.IsUploadPhotos = true;
              }
			  if (response.data.packegesdata[i].point_id == 14) {
                e.packageConditions.IsintigrationofSocialMedia = true;
              }
            }
          }
        })
    },
	UpdateSocialIDs () {
      this.isLoading = true
      let e = this
      axios
        .post('/churcheview/editSocialmediaids', {
          churcheid: this.churcheid,
          facebookID: this.formSocailIds.facebookID,
          facebookToken: this.formSocailIds.facebookToken,
          InstagramID: this.formSocailIds.instagramID,
		  youtubeID: this.formSocailIds.youtubeID,
		  twitterID: this.formSocailIds.twitterID,
		  pinterestID: this.formSocailIds.pinterestID,
        })
        .then(function (response) {
          e.isLoading = false
          e.socialmediasFormDialog = false
          if (response.data.status == true) {
            e.churchdata(e.$route.params.slug)
            e.$toasted.success('Erfolgreich aktualisiert', {
              position: 'top-center',
              duration: 2000
            })
          } else {
            e.$toasted.error('Please try after sometime', {
              position: 'top-center',
              duration: 2000
            })
          }
        })
    },

    remove (item) {
      this.chips.splice(this.chips.indexOf(item), 1)
      this.chips = [...this.chips]
    },
	  myremove (item) {
      this.chipslanguage.splice(this.chipslanguage.indexOf(item), 1)
      this.chipslanguage = [...this.chipslanguage]
    },
    formatDate (date) {
      if (!date) return null

      const [year, month, day] = date.split('-')
      return `${day}.${month}.${year}`
    },
	  parseDate (date) {
      if (!date) return null

      const [month, day, year] = date.split('/')
      return `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`
    }
  },
  components: {
    'gallery': VueGallery,
    'truncate': truncate,
    Loading,
    BulmaAccordion,
    BulmaAccordionItem,
    VueInstagram,
    Tweet,
    Timeline,
    VueYouTubeEmbed,
    vueVimeoPlayer

    // 'datetime': datetime
  }
}
</script>

<style>
.socialBox {
  text-align: center;
  margin: auto;
}
.socialBox i {
  font-size: 20px;
  line-height: 16px;
  margin-right: 5px;
}
.socialBox span {
  margin-right: 20px;
  margin-bottom: 10px;
  display: inline-block;
  cursor: pointer;
}
.editr--content {
  background: #ffffff;
}
.image {
    float: left;
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center center;
    border: 1px solid #ebebeb;
    margin: 10px;
}
.imgDeal:hover{
  background-color:#f58c00;
}
.accordion-body{
	margin-top: -17px !important;
    margin-left: -12px !important;
    width: 709px !important;
}
.imagesvideocard{
	box-shadow: 0px 0px 0px 0px rgba(0,0,0,.25) !important;
    border-top: 0px solid #0b324f !important;
    border-color: #ffffff !important !important;
    border-radius: 0 !important;
}
 button.grey:hover {
    background-color: black !important;
    color: white;
}
button.orange:hover{
	background-color: black !important;
    color: white;
}
.audiovideosection .v-input__slot {
   border-style: none !important;
}
.background{
	background-color:#8080802e !important;
}
</style>
