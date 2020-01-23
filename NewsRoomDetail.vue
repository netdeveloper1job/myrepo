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
     <!-- <v-navigation-drawer v-model="drawer" fixed temporary>
        <v-card flat color="white" class="mobileFilterWarp">
          <v-card-text>
            <v-form>
              <v-text-field placeholder="Placeholder" append-icon="search" class="searchInput"></v-text-field>
              <v-select
                label="Land:"
                placeholder="Land..."
                :items="LandList"
                item-text="name"
                v-model="land"
                item-value="id"
              ></v-select>
              <v-select
                :items="GemeindetypeList"
                item-text="type"
                item-value="id"
                label="Gemeindetyp:"
                placeholder="Bitte auswählen"
              ></v-select>
              <v-select label="Ressort:" placeholder="Bitte auswählen"></v-select>
              <v-select label="Subressort:" placeholder="Stichwort einfügen"></v-select>
              <v-select label="Themen:" placeholder="Themen..."></v-select>
              <v-select label="People:" placeholder="People..."></v-select>

            </v-form>
          </v-card-text>
        </v-card>
      </v-navigation-drawer>-->
      <v-container class="white py-0">
        <v-layout>
          <v-flex xs6>
            <v-btn flat class="white black--text ma-0 px-0 btnTab">Kirchen</v-btn>
          </v-flex>
          <v-flex xs6>
            <v-menu
              close-on-content-click="false"
              close-on-content="false"
              transition="slide-y-transition"
              class="right"
              offset-y
              left
              nudge-top="-15"
              :nudge-width="180"
              text-xs-center
              attach="#fooAnchor"
            >
              <template>
                <v-btn flat class="grey lighten-2 ma-0 black--text" slot="activator" id="fooAnchor">
                  <v-icon class="mr-2">folder</v-icon>
                  {{whishlistcount}} Lightbox
                </v-btn>
              </template>
              <v-card>
                <v-container fluid grid-list-lg class="dropContainer">
                  <v-layout row wrap>
                    <v-flex md12>
                      <v-card class="dropCard" v-if="whishlistdata.length == 0">
                        <v-card-title class="p-0">
                          <div>
                            <div>0 Dateien in der Lightbox</div>
                          </div>
                        </v-card-title>
                      </v-card>
                      <v-card
                        v-if="whishlistdata.length != 0"
                        v-for="(data, index) in whishlistdata"
                        class="dark--text mb-3 dropCard"
                      >
                        <v-layout>
                          <v-flex xs4>
                            <v-img
                              v-if="data.attachment_type == 'image'"
                              :src="'http://dev.woobii.com/admin/'+data.attachment"
                              height="80px"
                              contain
                            ></v-img>
                            <v-icon
                              v-if="data.attachment_type != 'image'"
                              light
                              size="80"
                            >description</v-icon>
                          </v-flex>
                          <v-flex xs8>
                            <v-card-title primary-title class="p-0">
                              <div>
                                <v-btn small icon class="dropClose" @click="deletewhishlist(index)">
                                  <v-icon>close</v-icon>
                                </v-btn>
                                <div>
                                  .jpg
                                  <br>2126 x 1462
                                  <br>385.63 KB
                                </div>
                              </div>
                            </v-card-title>
                          </v-flex>
                        </v-layout>
                        <v-divider light></v-divider>
                      </v-card>
                      <v-btn
                        v-if="whishlistdata.length != 0"
                        outline
                        @click="whishlistdata = []; whishlistcount = 0;"
                        color="#fa6e2f"
                      >Empty</v-btn>
                      <v-btn
                        v-if="whishlistdata.length != 0"
                        @click="whishlistzipdownload"
                        class="theme--dark"
                        color="#fa6e2f"
                      >Download</v-btn>
                    </v-flex>
                  </v-layout>
                </v-container>
              </v-card>
            </v-menu>
          </v-flex>
        </v-layout>
      </v-container>
    </section>
    <section class="newsRoomSection">
      <v-container fluid grid-list-xl>
        <v-layout row wrap justify-center>
          <!--<v-flex xs12 md3>
            <v-card flat color="white" class="newsRoomFilter">
              <v-card-text>
                <v-form>
                  <v-text-field placeholder="Placeholder" append-icon="search"></v-text-field>
                   <v-select
                    label="Land:"
                    placeholder="Land..."
                    :items="LandList"
                    item-text="name"
                    v-model="land"
                    item-value="id"
                  ></v-select>
                  <v-select
                    :items="GemeindetypeList"
                    item-text="type"
                    item-value="id"
                    label="Gemeindetyp:"
                    placeholder="Bitte auswählen"
                  ></v-select>
                  <v-select label="Ressort:" placeholder="Bitte auswählen"></v-select>
                  <v-select label="Subressort:" placeholder="Stichwort einfügen"></v-select>
                  <v-select label="Themen:" placeholder="Themen..."></v-select>
                  <v-select label="People:" placeholder="People..."></v-select>
                </v-form>
              </v-card-text>
            </v-card>
          </v-flex>-->
          <v-flex xs12 md9 class="newsDetail">
            <v-card flat color="white">
              <v-tabs v-model="active" slider-color="deep-orange accent-3">
                <v-tab key="1">Text</v-tab>
                <v-tab key="2" v-if="newsroomData.attachment.image.length > 0">Bilder</v-tab>
                <v-tab key="3" v-if="newsroomData.attachment.document.length > 0">Dokument</v-tab>
                <v-tab-item key="1">
                  <v-card flat>
                    <v-card-text class="px-0">
                      <p class="caption mb-1">{{ newsroomData.text[0].date | moment("DD.MM.YYYY")}}</p>
                      <p class="caption mb-1" v-html="newsroomData.text[0].short_desc"></p>
                      <div class="grey lighten-3 my-3 pa-2">
					  <p
                          class="caption font-weight-bold mb-1 orangeText"
                        >{{newsroomData.text[0].image_title}}</p>
                        <v-img
                          v-if="newsroomData.text[0].bannerimage"
                          :lazy-src="'http://dev.woobii.com/admin/'+newsroomData.text[0].bannerimage"
                          :src="'http://dev.woobii.com/admin/'+newsroomData.text[0].bannerimage"
                          class="mb-2"
                        />
                        <v-img
                          v-else
                          :src="require(`@/assets/woobii-banner.jpg`)"
                          :lazy-src="require(`@/assets/woobii-banner.jpg`)"
                          class="mb-2"
                        />
                        <p
                          class="caption font-weight-bold text-xs-center mb-1"
                        >{{newsroomData.text[0].image_description}}</p>
                      </div>
                      <div
                        class="grey lighten-3 my-3 pa-2"
                        v-if="newsroomData.attachment.image.length != 0 || newsroomData.attachment.document.length != 0 "
                      >
                        <span
                          class="caption font-weight-bold"
                        >Zu dieser Presseinformation bieten wir:</span>
                        <v-btn
                          small
                          outline
                          color="black"
                          class="my-0"
                          @click="next(1)"
                          v-if="newsroomData.attachment.image.length > 0"
                        >
                          <v-icon small dark class="mr-1">camera_alt</v-icon>
                          {{newsroomData.attachment.image.length}} Bider
                        </v-btn>
                        <v-btn
                          small
                          outline
                          color="black"
                          class="my-0"
                          @click="next(2)"
                          v-if="newsroomData.attachment.document.length > 0"
                        >
                          <v-icon small dark class="mr-1">description</v-icon>
                          {{newsroomData.attachment.document.length}} Dokument
                        </v-btn>
                      </div>
                      <div class="grey lighten-3 my-3 pa-2">
                        <span class="body-2 font-weight-bold mb-0 mr-2">Presseinformation</span>
                        <span class="caption font-weight-bold mb-0">({{ contentLength() }} Zeichen)</span>
                      </div>
                      <p class="caption mb-1" v-html="newsroomData.text[0].description"></p>
                      <div class="topBottonBorder mt-3 py-3">
                        <v-btn small outline color="black" class="ma-0">
                          <v-icon small dark class="mr-1">print</v-icon>Seite drucken
                        </v-btn>
                        <v-btn small outline color="black" class="right ma-0">
                          <v-icon small dark class="mr-1">email</v-icon>Link Mailen
                        </v-btn>
                      </div>
                    </v-card-text>
                  </v-card>
                </v-tab-item>
                <v-tab-item key="2">
                  <v-layout row wrap class="mt-2"  v-if="newsroomData.attachment.image.length > 0">
                    <v-flex xs12>
                      <h3 class="body-1">{{ newsroomData.attachment.image[0].description }}</h3>
                    </v-flex>
                    <v-flex xs12 class="py-0">
                      <v-img
                        v-if="newsroomData.attachment.image[imageIndex].attachment"
                        :lazy-src="'http://dev.woobii.com/admin/'+newsroomData.attachment.image[imageIndex].attachment"
                        :src="'http://dev.woobii.com/admin/'+newsroomData.attachment.image[imageIndex].attachment"
                        class="mb-2"
                      />
                    </v-flex>
                    <v-flex xs12>
                      <div
                        class="body-1"
                      >Neuzugang in der Geschäftsführung von ATP-Berlin: Niklas Veelken.</div>
                      <div
                        class="caption font-italic font-weight-light grey--text my-3"
                      >Credits: ATP/Rauschmeir</div>
                      <v-layout row wrap>
                        <v-flex xs1></v-flex>
                        <v-flex xs5></v-flex>
                        <v-flex xs3>Maße</v-flex>
                        <v-flex xs3>Größe</v-flex>
                      </v-layout>
                      <v-divider></v-divider>
                      <v-radio-group :mandatory="false" v-model="radioGroup">
                        <v-layout row wrap>
                          <v-flex xs1>
                            <v-radio key="original" color="blue darken-1" value="original"></v-radio>
                          </v-flex>
                          <v-flex xs5>Original</v-flex>
                          <v-flex xs3 class="grey--text">2480 x 3100</v-flex>
                          <v-flex xs3 class="grey--text">703.96 KB</v-flex>
                        </v-layout>
                        <v-divider></v-divider>
                        <v-layout row wrap>
                          <v-flex xs1>
                            <v-radio color="blue darken-1" key="medium" value="medium"></v-radio>
                          </v-flex>
                          <v-flex xs5>Medium</v-flex>
                          <v-flex xs3 class="grey--text">1200 x 1500</v-flex>
                          <v-flex xs3 class="grey--text">227.84 KB</v-flex>
                        </v-layout>
                        <v-divider></v-divider>
                        <v-layout row wrap>
                          <v-flex xs1>
                            <v-radio color="blue darken-1" key="klein" value="klein"></v-radio>
                          </v-flex>
                          <v-flex xs5>Klein</v-flex>
                          <v-flex xs3 class="grey--text">600 x 750</v-flex>
                          <v-flex xs3 class="grey--text">69.6 KB</v-flex>
                        </v-layout>
                        <v-divider></v-divider>
                        <v-layout row wrap class="tabsTextFeilds">
                          <v-flex xs1>
                            <v-radio color="blue darken-1" value="radio-4"></v-radio>
                          </v-flex>
                          <v-flex xs5>Benutzerdefiniert</v-flex>
                          <v-flex xs6>
                            <v-text-field
                              v-model="custwidth"
                              label
                              single-line
                              outline
                              class="mr-2"
                            ></v-text-field>X
                            <v-text-field
                              v-model="custheight"
                              label
                              single-line
                              outline
                              class="ml-1"
                            ></v-text-field>
                          </v-flex>
                        </v-layout>
                      </v-radio-group>
                      <v-divider></v-divider>
                      <v-layout row wrap>
                        <v-flex xs6>
                          <v-btn
                            large
                            @click="currentImageDownload"
                            depressed
                            outline
                            block
                            class="mb-2 textLeft hoverCustom"
                          >
                            <v-icon small dark class="mr-2">file_download</v-icon>Sofort downloaden
                          </v-btn>
                        </v-flex>
                        <v-flex xs6>
                          <v-btn
                            @click="currentImageWhishlist"
                            large
                            depressed
                            outline
                            block
                            class="mb-2 textLeft hoverCustom"
                          >
                            <v-icon small dark class="mr-2">folder</v-icon>In die Lightbox legen
                          </v-btn>
                        </v-flex>
                      </v-layout>
                      <v-layout row wrap>
                        <v-flex xs4 v-for="image in newsroomData.attachment.image">
                          <v-img
                            :lazy-src="'http://dev.woobii.com/admin/'+image.attachment"
                            :src="'http://dev.woobii.com/admin/'+image.attachment"
                          />
                          <h2
                            class="bilder-imgCaption grey lighten-2 body-1 font-weight-bold px-2 py-1 mb-3"
                          >
                            1160 X 1120
                            <v-icon small class="right red--text mr-2">camera_alt</v-icon>
                          </h2>
                        </v-flex>
                      </v-layout>
                    </v-flex>
                  </v-layout>
                </v-tab-item>
                <v-tab-item key="3" v-if="newsroomData.attachment.document">
                  <v-layout row wrap class="mt-2 tabsDocument">
                    <v-flex xs12 v-if="newsroomData.attachment.image.length > 0">
                      <h3 class="body-1 mb-3">{{ newsroomData.attachment.image[0].description }}</h3>
                    </v-flex>
                    <v-flex d-flex xs12 md3>
                      <v-card light tile flat style="border:1px solid #CCC;">
                        <v-card-text>
                          <v-icon light size="60">description</v-icon>
                        </v-card-text>
                      </v-card>
                    </v-flex>
                    <v-flex xs12 md9>
                      <v-layout row wrap>
                        <v-flex xs6>.pdf</v-flex>
                        <v-flex xs6>137.01 KB</v-flex>
                      </v-layout>
                      <v-divider></v-divider>
                      <v-layout row wrap>
                        <v-flex xs12 md6>
                          <v-btn
                            @click="downloadFile('document', '0')"
                            outline
                            block
                            class="mb-2 textLeft hoverCustom"
                          >
                            <v-icon small dark>file_download</v-icon>Sofort downloaden
                          </v-btn>
                        </v-flex>
                        <v-flex xs12 md6>
                          <v-btn
                            @click="whishlistFile('document', '0')"
                            depressed
                            outline
                            block
                            class="mb-2 textLeft hoverCustom"
                          >
                            <v-icon small dark class="mr-2">folder</v-icon>In die Lightbox legen
                          </v-btn>
                        </v-flex>
                      </v-layout>
                    </v-flex>
                  </v-layout>
                </v-tab-item>
              </v-tabs>
            </v-card>
          </v-flex>
          <v-flex xs12 md3 class="newsRight">
            <v-card flat color="white">
             
<li class="has-sub open"><a href="/de" class="">
     
        <p class="presscss"><i class="fa fa-edit"></i> &nbsp; Presseinformation</p></a>
                    <ul class="sub">
                        
        <li class="sub-has-sub open">
            <a class="active" href="/de/filter/94">
              
                <p class="arrow">News</p>
            </a>
            <ul class="sub">
                        <li class="sub-has-sub ">
            <a class="" href="/de/filter/2">
               
                 <p class="subarrow">Politik</p>
            </a>
        </li>
            </ul>
    <ul class="sub">
                        <li class="sub-has-sub ">
            <a class="" href="/de/filter/82">
                
                <p class="subarrow">Kirche</p>
            </a>
        </li>
            </ul>
    <ul class="sub">
                        <li class="sub-has-sub ">
            <a class="" href="/de/filter/9">
       <p class="subarrow">Wirtschaft</p>
            </a>
        </li>
            </ul>
    <ul class="sub">
                        <li class="sub-has-sub ">
            <a class="" href="/de/filter/97">
         <p class="subarrow">Sport</p>
            </a>
        </li>
            </ul>
    <ul class="sub">
                        <li class="sub-has-sub ">
            <a class="" href="/de/filter/100">
                              <p class="subarrow">Gesellschaft</p>
            </a>
        </li>
            </ul>
			
   <ul class="sub">
                        <li class="sub-has-sub ">
            <a class="" href="/de/filter/100">
                              <p class="subarrow">Medien</p>
            </a>
        </li>
            </ul>
			
	   <ul class="sub">
                        <li class="sub-has-sub ">
            <a class="" href="/de/filter/100">
                              <p class="subarrow">Kultur</p>
            </a>
        </li>
            </ul>
			
			
        </li>
            </ul>
    <ul class="sub">
                        
        <li class="sub-has-sub ">
            <a class="" href="/de/filter/10">
               
                <p class="arrow">Impulse</p>
            </a>
            <ul class="sub">
                        <li class="sub-has-sub ">
            <a class="" href="/de/filter/37">
               
              <p class="subarrow">People</p>
            </a>
        </li>
            </ul>
    <ul class="sub">
                        <li class="sub-has-sub ">
            <a class="" href="/de/filter/31">
                  <p class="subarrow">Im Blick</p>
            </a>
        </li>
            </ul>
    <ul class="sub">
                        <li class="sub-has-sub ">
            <a class="" href="/de/filter/34">
                               <p class="subarrow">Glaube</p>
            </a>
        </li>
            </ul>
    
        </li>
            </ul>
    <ul class="sub">
                        
        <li class="sub-has-sub ">
            <a class="" href="/de/filter/1">
             <p class="arrow">Ratgeber</p>
            </a>
            <ul class="sub">
                        <li class="sub-has-sub ">
            <a class="" href="/de/filter/7">
                <p class="subarrow">Beziehung</p>
            </a>
        </li>
            </ul>
    <ul class="sub">
                        <li class="sub-has-sub ">
            <a class="" href="/de/filter/8">
                <p class="subarrow">Gesundheit</p>
            </a>
        </li>
            </ul>
    <ul class="sub">
                        <li class="sub-has-sub ">
            <a class="" href="/de/filter/103">
                 <p class="subarrow">Karriere</p>
            </a>
        </li>
            </ul>
			<ul class="sub">
                        <li class="sub-has-sub ">
            <a class="" href="/de/filter/103">
                 <p class="subarrow">Seelsorge</p>
            </a>
        </li>
            </ul>
        </li>
            </ul>
            </li>
              <v-btn depressed outline block class="mb-2 textLeft">
                <v-icon small dark class="mr-2">edit</v-icon>Downloads
              </v-btn>
              <v-btn depressed outline block class="mb-2 textLeft">
                <v-icon small dark class="mr-2">edit</v-icon>Pressekontakt
              </v-btn>
              <v-divider class="my-3"></v-divider>
              <p class="caption mb-2">Alie Inhalte dieser Meldung als .zip:</p>
              <v-btn
                depressed
                dark
                block
                @click="downloadzip"
                color="grey darken-4 mb-2 hoverCustom"
              >
                <v-icon small dark class="mr-2">file_download</v-icon>Sofort downloaden
              </v-btn>
              <v-btn depressed dark block @click="whishlistzip" color="grey darken-4 hoverCustom">
                <v-icon small dark class="mr-2">folder</v-icon>In die Lightbox legen
              </v-btn>
              <v-divider class="my-3"></v-divider>
              <div v-if="newsroomData.attachment.image.length > 0 && active == 0">
                <a @click="next(1)">
                  <h2 class="subheading font-weight-bold mb-3">
                    <v-icon small class="black--text mr-2">camera_alt</v-icon>Bilder
                    <v-icon class="right black--text ml-2">keyboard_arrow_right</v-icon>
                  </h2>
                </a>
                <silentbox-group>
                  <div v-for="imageValue in newsroomData.attachment.image">
                    <silentbox-item
                      :src="'http://dev.woobii.com/admin/'+imageValue.attachment"
                      :description="imageValue.description"
                    >
                      <v-img
                        :lazy-src="'http://dev.woobii.com/admin/'+imageValue.attachment"
                        :src="'http://dev.woobii.com/admin/'+imageValue.attachment"
                      />
                    </silentbox-item>
                    <a @click="next(1)">
                      <h2
                        class="bilder-imgCaption grey lighten-2 body-1 font-weight-bold px-2 py-1 mb-3"
                      >
                        1160 X 1120
                        <v-icon small class="right red--text mr-2">camera_alt</v-icon>
                      </h2>
                    </a>
                  </div>
                </silentbox-group>
                <v-divider class="my-3"></v-divider>
              </div>
              <div v-if="newsroomData.attachment.document  && active == 0">
                <a @click="next(2)">
                  <h2 class="subheading font-weight-bold mb-3">
                    <v-icon small class="black--text mr-2">edit</v-icon>Dokument
                    <v-icon class="right black--text ml-2">keyboard_arrow_right</v-icon>
                  </h2>
                </a>
                <a
                  @click="next(2)"
                  class="caption mb-2"
                  v-for="document in newsroomData.attachment.document"
                >
                  <p class="caption mb-2">
                    {{ document.attachment | documentName }} | 1.25 MB
                    <v-icon small class="right black--text ml-2">edit</v-icon>
                  </p>
                </a>
              </div>
              <div v-if="active == 0">
                <v-divider class="my-3"></v-divider>
                <h2 class="subheading font-weight-bold mb-2">Pressekontakt</h2>
                <v-img
                  v-if="newsroomData.publisher[0].image"
                  :lazy-src="'http://dev.woobii.com/admin/'+newsroomData.publisher[0].image"
                  :src="'http://dev.woobii.com/admin/'+newsroomData.publisher[0].image"
                  class="mb-2"
                />
            <p class="caption mb-1">
                  <strong>{{newsroomData.publisher[0].name}}</strong>
                  <br>
                  {{newsroomData.publisher[0].designation}}
                </p>
                <p class="caption mb-1" v-html="newsroomData.publisher[0].address"></p>
                <p class="caption mb-1">Telefonnummer: {{newsroomData.publisher[0].contact}}</p>
                <p class="caption mb-1">{{newsroomData.publisher[0].email}}</p>
              </div>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
    </section>
  </v-content>
  </v-flex>
    </v-layout>
</template>

<script>
import axios from 'axios'
import JSZipUtils from 'jszip-utils'
var JSZip = require('jszip')

export default {
  name: 'App',
  data () {
    return {
      // baseUrl: process.env.BASE_URL
      isMobile: false,
      drawer: null,
      newsroomData: {},
      GemeindetypeList: [],
      LandList: [],
      land: '',
      active: 0,
      imageGallery: [],
      custwidth: '',
      custheight: '',
      imageIndex: 0,
      whishlistdata: [],
      whishlistcount: 0,
      radioGroup: 'original'
    }
  },
  filters: {
    documentName: function (val) {
      let doc = val.split('/')
      return doc.pop()
    }
  },
  mounted () {
	// alert(this.$route.params.slug);
    this.newsroomdata(this.$route.params.slug)
    this.listofmuncipalty()
    this.listofcountry()
  },
  beforeDestroy () {},
  methods: {
    whishlistzipdownload: function () {
      let url = axios.defaults.baseURL
      var zip = new JSZip()
      var img = zip.folder('whishlist')
      if (this.whishlistdata.length >= 1) {
        for (var imageindex in this.whishlistdata) {
          const filename = this.whishlistdata[imageindex].attachment.split('/')
          let fpath =
            url + this.newsroomData.attachment.image[imageindex].attachment
          img.file(filename.pop(), fpath)
        }
      }
      zip.generateAsync({ type: 'blob' }).then(function (content) {
        // see FileSaver.js
        saveAs(content, 'woobii-download.zip')
      })
    },
    deletewhishlist: function (index) {
      this.whishlistdata.splice(index, 1)
      this.whishlistcount = this.whishlistdata.length
    },
    whishlistzip: function () {
      if (this.newsroomData.attachment.image.length != 0) {
        if (this.whishlistdata.length == 0) {
          this.newsroomData.attachment.image.forEach(element => {
            this.whishlistdata.push(element)
            this.whishlistcount = this.whishlistdata.length
          })
        } else {
          this.newsroomData.attachment.image.forEach(element => {
            if (
              this.whishlistdata.find(e =>
                e.attachment_type == 'image'
                  ? e.attachment === element.image.attachment
                  : -2
              ) == ''
            ) {
              this.whishlistdata.push(element)
              this.whishlistcount = this.whishlistdata.length
            }
          })
        }
      }
      if (this.newsroomData.attachment.document.length != 0) {
        if (this.whishlistdata.length == 0) {
          this.newsroomData.attachment.document.forEach(element => {
            this.whishlistdata.push(element)
            this.whishlistcount = this.whishlistdata.length
          })
        } else {
          this.newsroomData.attachment.document.forEach(element => {
            if (
              this.whishlistdata.find(e =>
                e.attachment_type == 'document'
                  ? e.attachment === element.document.attachment
                  : '2'
              ) == ''
            ) {
              this.whishlistdata.push(element)
              this.whishlistcount = this.whishlistdata.length
            }
          })
        }
      }
    },
    whishlistFile: function (fileType, fileNo) {
      if (this.whishlistdata.length == 0) {
        this.whishlistdata.push(this.newsroomData.attachment[fileType][fileNo])
        this.whishlistcount = this.whishlistdata.length
      } else {
        if (
          this.whishlistdata.find(e =>
            e.attachment_type == fileType
              ? e.attachment ===
                this.newsroomData.attachment[fileType][fileNo].attachment
              : ''
          ) == ''
        ) {
          this.whishlistdata.push(
            this.newsroomData.attachment[fileType][fileNo]
          )
          this.whishlistcount = this.whishlistdata.length
        }
      }
    },
    currentImageWhishlist: function () {
      if (this.whishlistdata.length == 0) {
        this.whishlistdata.push(this.newsroomData.attachment.image[0])
        this.whishlistcount = this.whishlistdata.length
      } else {
        if (
          this.whishlistdata.find(e =>
            e.attachment_type == 'image'
              ? e.attachment ===
                this.newsroomData.attachment.image[0].attachment
              : ''
          ) == ''
        ) {
          this.whishlistdata.push(this.newsroomData.attachment.image[0])
          this.whishlistcount = this.whishlistdata.length
        }
      }
    },
    downloadzip: function () {
      let url = axios.defaults.baseURL
      var e = this
      var zip = new JSZip()
      var img = zip.folder('images')
      var doc = zip.folder('document')
      if (this.newsroomData.attachment.image.length >= 1) {
        for (var imageindex in this.newsroomData.attachment.image) {
          const filename = this.newsroomData.attachment.image[
            imageindex
          ].attachment.split('/')
          let fpath =
            url + this.newsroomData.attachment.image[imageindex].attachment
          img.file(filename.pop(), fpath)
          // JSZipUtils.getBinaryContent(fpath, function(err, data) {
          //   console.log(err);
          //   img.file(filename.pop(), data);
          // });
        }
      }
      if (this.newsroomData.attachment.document.length >= 1) {
        for (var documentindex in this.newsroomData.attachment.document) {
          const filename = this.newsroomData.attachment.document[
            documentindex
          ].attachment.split('/')
          let fpath =
            url +
            this.newsroomData.attachment.document[documentindex].attachment
          doc.file(filename.pop(), fpath)
          // JSZipUtils.getBinaryContent(fpath, function(err, data) {
          //   console.log(err);
          //   doc.file(filename.pop(), data, { binary: true });
          // });
        }
      }

      zip.generateAsync({ type: 'blob' }).then(function (content) {
        // see FileSaver.js
        saveAs(content, 'woobii-download.zip')
      })
    },
    currentImageDownload: function () {
      this.imageIndex
      this.radioGroup
      this.downloadFile('image', this.imageIndex)
    },
    forceFileDownload (response) {
      const filename = response.split('/')
      const link = document.createElement('a')
      // link.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(text));
      link.href = response
      link.style.display = 'none'
      link.setAttribute('download', filename.pop()) // or any other extension
      document.body.appendChild(link)
      link.click()
      document.body.removeChild(link)
    },
    downloadFile: function (fileType, fileNo) {
      let url = axios.defaults.baseURL
      this.forceFileDownload(
        url + this.newsroomData.attachment[fileType][fileNo].attachment
      )
    },
    next: function (e) {
      const active = e
      this.active = active <= 2 ? active : 0
    },
    contentLength () {
      var content = this.newsroomData.text[0].description
      content = content.toString()
      return content.replace(/<[^>]*>/g, '').length
    },
    discardSelected () {
      var dataImage = this.newsroomData.attachment.image
      dataImage.splice(this.imageIndex, 1)
      return dataImage
    },
    newsroomdata: function (slug) {
      var e = this
      axios
        .get('/churcheview/newsroom?s=' + slug)
        .then(function (response) {
          if (response.data.status == true) {
            e.newsroomData = response.data.newsroom
			//console.log(response.data.newsroom);
            if (e.newsroomData.attachment.image) {
              e.imageGallery = e.newsroomData.attachment.image
            }
          }
          // console.log(response.data);
          // console.log(response.status);
          // console.log(response.statusText);
          // console.log(response.headers);
          // console.log(response.config);
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
          console.log(error.config)
        })
    },
    listofmuncipalty: function () {
      var e = this
      axios
        .get('/adminglobal/getallmuncipalitytype')
        .then(function (response) {
          if (response.data.status == true) {
            e.GemeindetypeList = response.data.allmuncipalitytype
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
    listofcountry: function () {
      var e = this
      axios
        .get('/adminglobal/getallcountry')
        .then(function (response) {
          if (response.data.status == true) {
            e.LandList = response.data.countries.country
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
    }
  }
}
</script>
<style scoped>
.progess-custom {
  margin: 0px;
}
.presscss{
	    background: black;
    color: white;
    padding: 7px;
	width: 100%;
}
.has-sub{
	list-style-type:none;
}
.sub-has-sub
{
	list-style-type:none;
}
.arrow{
	 transition: 0.9s;
	    width: 111%;
    margin-left: -24px;
    border:1px solid #b1b1b1;;
    padding: 7px;
    margin-top: -16px;
padding-left: 44px;
border-top:none;

	}

.subarrow{
	 transition: 0.9s;
	margin-top: -16px;
	 padding: 7px;
    margin-left: -48px;
    width: 124.5%;
	border-left:1px solid #b1b1b1;
	border-right:1px solid #b1b1b1;
	border-bottom:1px solid #b1b1b1;
	padding-left: 59px;

	
}
.arrow:hover{
	 transition: 0.9s;
	    background: #c03 !important;
		color:white;
		
}
.subarrow:hover{
	 transition: 0.9s;
	background: #c03 !important;
		color:white;
		
		
}
button.mb-2.textLeft.v-btn.v-btn--block.v-btn--outline.v-btn--depressed.theme--light {
border:1px solid #b1b1b1;
	color:black;
	
}
button.mb-2.textLeft.v-btn.v-btn--block.v-btn--outline.v-btn--depressed.theme--light:hover {
    background: #c03 !important;
	color:white;
	
}
</style>
