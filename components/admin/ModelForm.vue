<template>
  <div
    class="absolute flex w-screen min-w-[1440px] h-screen min-h-[720px] text-gray-500"
  >
    <!-- <div class="absolute top-0 right-0">Close</div> -->
    <div
      @click="closeModelForm()"
      class="absolute w-full h-full flex justify-center items-center bg-gray-900 bg-opacity-75"
    >
      <div
        @click.stop
        class="z-10 w-[50%] py-[10px] h-full flex justify-center items-center bg-gray-900 flex-no-wrap"
      >
        <div class="overflow-auto w-[90%] h-full z-10 shadow-md">
          <!-------------IMAGE_BLOCK------------->
          <div
            v-if="object.image_path !== undefined"
            class="w-full h-[360px] px-[20px] py-[10px]"
          >
            <div class="w-full h-[85%] flex justify-center">
              <img
                class="max-w-[60%] min-w-[30%] h-full bg-white"
                :src="`${imgURL}${image.path}${imgFormat}`"
              />
            </div>
            <div class="w-full h-[15%] flex justify-center items-center">
              <div class="flex justify-center items-center w-[210px]">
                <label class="text-white form-label inline-block">
                  <input
                    @change="newsPhotoSelect"
                    class="form-control hidden w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none"
                    type="file"
                  />
                  Select Photo
                </label>
              </div>
              <div
                v-if="image.nameAndSize !== undefined"
                class="flex justify-center items-center w-[210px] text-white"
              >
                <p class="truncate">{{ image.nameAndSize }}</p>
              </div>
            </div>
          </div>
          <!-------------VIDEO_BLOCK------------->
          <div
            v-if="object.video_path !== undefined"
            class="w-full h-[360px] px-[20px] mt-[20px] py-[5px]"
          >
            <div class="w-full h-[85%] flex justify-center">
              <video
                ref="Video"
                class="max-w-[90%] min-w-[30%] h-full"
                controls
              >
                <source ref="suka" :src="`${videoURL}${video.path}`"   type="video/mp4" />
                <source src="movie.ogg" type="video/ogg" />
                Your browser does not support the video tag.
              </video>

              <!-- <img
                class="max-w-[60%] min-w-[30%] h-full bg-gray-800"
                :src="`${imgURL}${image.path}`"
              /> -->
            </div>
            <div class="w-full h-[15%] flex justify-center items-center">
              <div class="flex justify-center items-center w-[210px]">
                <label class="text-white form-label inline-block">
                  <input
                    @change="newsVideoSelect"
                    class="form-control hidden w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none"
                    type="file"
                  />
                  Select Video
                </label>
              </div>
              <div
                v-if="video.nameAndSize !== undefined"
                class="flex justify-center items-center w-[210px] text-white"
              >
                <p class="truncate">{{ video.nameAndSize }}</p>
              </div>
            </div>
          </div>
          <!-------------TEXTS_BLOCK------------->

          <!-- ##{{ object.translations[0].text }} -->
          <div
            v-if="object.translations !== undefined"
            class="w-full min-h-[260px] px-[20px] mt-[20px] py-[5px]"
          >
            <div
              v-for="(item, index) in keysArray"
              :key="item"
              class="w-full h-full mt-[20px]"
            >
              <template v-for="Translate in object.translations">
                <!-- :key="(index2 + 1) * Math.random()"  mail-->

                <div
                  v-if="
                    Translate.title !== undefined &&
                    Translate.title[titleKeysArray[index]] !== undefined
                  "
                  :key="(index + 1) * Math.random()"
                  class="w-full h-[50px] px-[10px] py-[10px]"
                >
                  <input
                    v-model.lazy="Translate.title[titleKeysArray[index]]"
                    class="w-full h-full mt-[20px] pl-[10px] text-black focus:outline-none"
                    type="text"
                  />
                </div>

                <div
                  v-if="
                    Translate.text !== undefined &&
                    Translate.text[textKeysArray[index]] !== undefined
                  "
                  :key="(index + 1) * Math.random()"
                  class="overflow-auto w-full max-h-[200px] min-h-[120px] mt-[10px] px-[10px] py-[10px]"
                >
                  <textarea
                    v-model.lazy="Translate.text[textKeysArray[index]]"
                    class="w-full min-h-[400px] focus:outline-none"
                  ></textarea>
                </div>

                <!-- </template> -->
              </template>
            </div>
            <!-- <div class="w-full h-full mt-[20px] bg-red-500">
              <div class="w-full h-[50px] px-[10px] py-[10px] bg-gray-700">
                <input
                  class="w-full h-full pl-[10px] text-black focus:outline-none"
                  type="text"
                />
              </div>
              <div class="w-full mt-[10px] px-[10px] py-[10px] bg-gray-700">
                <client-only>
                  <VueEditor
                    class="overflow-auto max-h-[400px] h-[160px] h-full text-black bg-white"
                    v-model="content"
                    :editor-toolbar="customToolbar"
                  />
                </client-only>
              </div>
            </div> -->
          </div>

          <!-------------MAIL_BLOCK------------->

          <div
            v-if="object.mail !== undefined"
            class="w-full min-h-[260px] px-[20px] mt-[20px] py-[5px]"
          >
            <div class="w-full h-full mt-[20px]">
              <!-- :key="(index2 + 1) * Math.random()"  mail-->

              <div class="w-full h-[50px] px-[10px] py-[10px]">
                <input
                  v-model.lazy="object.mail"
                  class="w-full h-full mt-[20px] pl-[10px] text-black focus:outline-none"
                  type="text"
                />
              </div>
            </div>
          </div>
          <!-------------NUMBER_BLOCK------------->

          <div
            v-if="object.number !== undefined"
            class="w-full min-h-[260px] px-[20px] mt-[20px] py-[5px]"
          >
            <div class="w-full h-full mt-[20px]">
              <!-- :key="(index2 + 1) * Math.random()"  mail-->

              <div class="w-full h-[50px] px-[10px] py-[10px]">
                <input
                  v-model.lazy="object.number"
                  class="w-full h-full mt-[20px] pl-[10px] text-black focus:outline-none"
                  type="text"
                />
                <input
                  v-model.lazy="object.number"
                  class="w-full h-full mt-[20px] pl-[10px] text-black focus:outline-none"
                  type="text"
                />
              </div>
            </div>
          </div>

          <div class="w-full h-[80px] px-[20px] mt-[20px] py-[5px]">
            <div
              class="w-full h-full flex justify-center items-center text-black"
            >
              <button @click="editItem()" class="w-[220px] h-[40px] bg-white">
                EDIT
              </button>
              <!----------Confirm Block---------->
              <div v-if='confirmBlock' class="absolute flex justify-center left-0 top-0 w-full h-full ">
                <div class="flex flex-col  items-start w-[450px] h-full ">
                  <div class="flex items-end w-full h-[350px] " >
                    <div class="flex items-center w-full h-[150px] bg-gray-700 shadow-md">
                      <div class="flex justify-around w-full h-[50px] ">
                        <button @click="confirmEditYes"  class="w-[170px] h-full bg-white text-lg font-bold text-blue-900 hover:bg-blue-900 hover:text-white shadow-md">Yes</button>
                        <button @click="confirmEditNo" class="w-[170px] h-full bg-white text-lg font-bold text-red-900 hover:bg-red-900 hover:text-white shadow-md" >No</button>
                      </div>
                    </div>
                  </div>

                  
                   <!----------Upload Block---------->
                <div v-if="uploadBlock" class= "flex items-center w-full h-[50px]">
                  <div class="flex justify-center w-full h-[20px] ">
                    <svg role="status" class="inline w-20 h-20 mr-2 text-gray-200 animate-spin dark:text-gray-600 fill-blue-600" viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z" fill="currentColor"/>
                        <path d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z" fill="currentFill"/>
                    </svg>
                  </div>
                </div>
                </div>

              </div>
    
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// import TiptapEditor from "~/components/TiptapEditor.vue";

// import Editor from "./Editor.vue";
//

import { mapGetters, mapActions } from 'vuex'
// import TextEditor from "@/components/TextEditor";
export default {
  layout: 'admin',
  components: {
    // TiptapEditor,
    // Editor,
  },
  data() {
    return {
      editor: null,
      showEdit: '',
      //
      object: '',
      /*Confirm Block*/
      confirmBlock:false,
      confirmYes:'',
      confirmNo:'',
      //
      uploadBlock:false,
      // uploadProsent:'50',
      //
      textKeysArray: [],
      titleKeysArray: [],
      keysArray: [],

      customToolbar: [
        // ["bold", "italic", "underline", "bg-black"],
        // [{ list: "ordered" }, { list: "bullet" }],
        // ["image", "code-block"],
      ],
      content: '',
      //
      navBar: true,
      imgURL: 'https://marylandllc.ae/',
      imgFormat: '.jpg',
      //
      videoURL: 'https://marylandllc.ae/',
      // videoFormat:'.mp4',
      //
      changingURL: '',
      // creatingLink: "",
      // editingLink: "",
      videoPath: '',
      image: {
        file: '',
        path: '',
        nameAndSize: undefined,
      },

      video: {
        file: '',
        path: '',
        nameAndSize: undefined,
        // path: "/_nuxt/assets/Linux.mp4",
      },

      namesArray: [
        //Homepage
        {
          title: ' Home ',
          href: 'Homepage',
          icon: 'fa fa-home',
          id: 1,
          active: false,
          array: [
            // { title: "Baş sahypanyň slideri", link: "get-language" },
            // { title: "Baş sahypanyň tekstleri", link: "get-home" },
            {
              title: ' Texts',
              link: 'get-home',
              editingLink: 'save-product',
              creatingLink: 'add-product',
            },
            {
              title: ' Header',
              link: 'get-header/home',
              editingLink: 'save-about',
              creatingLink: 'add-about',
            },
            // { title: " Statistika", link: "homepage/statistic" },
          ],
        },
        //About

        // get-about
        // get-header/about
        {
          title: ' About ',
          href: 'About',
          icon: 'fa fa-eject',
          id: 2,
          active: false,
          array: [
            { title: ' About Texts ', link: 'get-about' },
            { title: 'Header', link: 'get-header/about' },
          ],
        },
        //Products
        {
          title: ' Products ',
          href: 'Servicepage',
          icon: 'fa fa-fa fa-sitemap',
          id: 3,
          active: false,
          array: [
            { title: 'Products', link: 'get-product' },
            { title: 'Header', link: 'get-header/product' },
          ],
        },
        //Contactpage

        // get-contact
        // get-header/contact
        {
          title: ' Contacts ',
          href: 'Contactpage',
          icon: 'fa fa-phone',
          id: 5,
          active: false,
          array: [
            { title: ' Contact Text ', link: 'get-contact' },
            { title: ' Header ', link: 'get-header/contact' },
          ],
        },
        //Footer
      ],

      nameParamsArray: [
        // { title: "Baş sahypanyň slideri", link: "get-language" },
        // { title: "Baş sahypanyň tekstleri", link: "get-home" },
        {
          title: ' Hyzmatdaşlar',
          link: 'get-product',
          editingLink: 'save-product',
          creatingLink: 'add-product',
        },
        {
          title: ' Hyzmatdaşlar',
          link: 'get-about',
          editingLink: 'save-about',
          creatingLink: 'add-about',
        },
        {
          title: ' Hyzmatdaşlar',
          link: 'get-home',
          editingLink: 'save-home',
          creatingLink: 'add-home',
        },
        // { title: " Statistika", link: "homepage/statistic" },
      ],

      data: [
        {
          product_id: '1111',
          translations: [
            {
              title: {
                title1: 'title1_RU',
                title2: 'title2_RU',
                title3: 'title3_RU',
                title4: 'title4_RU',
                title5: 'title5_RU',
              },
              text: {
                text1: 'Text1_RU',
                text2: 'Text2_RU',
                text3: 'Text3_RU',
              },
            },
            {
              title: {
                title1: 'title1_EN',
                title2: 'title2_EN',
                title3: 'title3_EN',
                title4: 'title4_EN',
                title5: 'title5_EN',
              },
              text: {
                text1: 'Text1_EN',
                text2: 'Text2_EN',
                text3: 'Text3_EN',
              },
            },
            // {
            //   id: "ID",
            //   title: "title",
            //   text: "Text",
            //   lang: "en",
            // },
            // {
            //   id: "ID",
            //   title: "title",
            //   text: "Text",
            //   lang: "tm",
            // },
            // {
            //   id: "ID",
            //   title: "title",
            //   text: "Text",
            //   lang: "tm",
            // },
          ],
        },
        {
          product_id: '1111',
          translations: [
            {
              title: {
                title1: 'title1',
                title2: 'title2',
                title3: 'title3',
                title4: 'title4',
                title5: 'title5',
              },
              text: {
                text1: 'Text1',
                text2: 'Text2',
                text3: 'Text3',
              },
            },
            {
              title: {
                title1: 'title1',
                title2: 'title2',
                title3: 'title3',
                title4: 'title4',
                title5: 'title5',
              },
              text: {
                text1: 'Text1',
                text2: 'Text2',
                text3: 'Text3',
              },
            },
            // {
            //   id: "ID",
            //   title: "title",
            //   text: "Text",
            //   lang: "en",
            // },
            // {
            //   id: "ID",
            //   title: "title",
            //   text: "Text",
            //   lang: "tm",
            // },
            // {
            //   id: "ID",
            //   title: "title",
            //   text: "Text",
            //   lang: "tm",
            // },
          ],
        },

        {
          product_id: '1111',
          translations: [
            {
              title: {
                title1: 'title1',
                title2: 'title2',
                title3: 'title3',
                title4: 'title4',
                title5: 'title5',
              },
              text: {
                text1: 'Text1',
                text2: 'Text2',
                text3: 'Text3',
              },
            },
            {
              title: {
                title1: 'title1',
                title2: 'title2',
                title3: 'title3',
                title4: 'title4',
                title5: 'title5',
              },
              text: {
                text1: 'Text1',
                text2: 'Text2',
                text3: 'Text3',
              },
            },
            // {
            //   id: "ID",
            //   title: "title",
            //   text: "Text",
            //   lang: "en",
            // },
            // {
            //   id: "ID",
            //   title: "title",
            //   text: "Text",
            //   lang: "tm",
            // },
            // {
            //   id: "ID",
            //   title: "title",
            //   text: "Text",
            //   lang: "tm",
            // },
          ],
        },

        {
          product_id: '1111',
          translations: [
            {
              title: {
                title1: 'title1',
                title2: 'title2',
                title3: 'title3',
                title4: 'title4',
                title5: 'title5',
              },
              text: {
                text1: 'Text1',
                text2: 'Text2',
                text3: 'Text3',
              },
            },
            {
              title: {
                title1: 'title1',
                title2: 'title2',
                title3: 'title3',
                title4: 'title4',
                title5: 'title5',
              },
              text: {
                text1: 'Text1',
                text2: 'Text2',
                text3: 'Text3',
              },
            },
            // {
            //   id: "ID",
            //   title: "title",
            //   text: "Text",
            //   lang: "en",
            // },
            // {
            //   id: "ID",
            //   title: "title",
            //   text: "Text",
            //   lang: "tm",
            // },
            // {
            //   id: "ID",
            //   title: "title",
            //   text: "Text",
            //   lang: "tm",
            // },
          ],
        },
        // {
        //   product_id: "2222",
        //   translations: [
        //     {
        //       id: "ID",
        //       title: "title_RU",
        //       text: "Text_RU",
        //       lang: "ru",
        //     },
        //     {
        //       id: "ID",
        //       title: "title_EN",
        //       text: "Text_En",
        //       lang: "en",
        //     },
        //     {
        //       id: "ID",
        //       title: "title_TM",
        //       text: "Text_TM",
        //       lang: "tm",
        //     },
        //     {
        //       id: "ID",
        //       title: "title_FR",
        //       text: "Text_FR",
        //       lang: "fr",
        //     },
        //   ],
        // },
      ],
      datesArray: '',
    }
  },

  async mounted() {
    //
    //
    await this.createObject

    await this.createTextKeys
    await this.createTitleKeys
    await this.createKeysArray
    // this.uploadProsent = this.getUploadProsent
  },

  computed: {
    ...mapGetters('admin', [
      'getDates',
      'getUploadProsent',
      'getObject',
      // "getFullObject",
      // "getEmptyObject",
      'getLink',
      // "getEditingLink",
      // "getCreatingLink",
    ]),

    // checkObj() {
    //   this.object;
    // },
    //
    // takeArr() {
    //   this.data = this.getDates;
    //   console.log("ARR!!", this.data);
    // },

    createTextKeys() {
      console.log('WWWWW')
      console.log(
        'this.object?.translations[0] =>',
        this.object?.translations ?? 'UPS!'
      )
      if (this.object?.translations?.[0]?.text ?? false) {
        console.log('createTextKeys => ! = 0')
        this.textKeysArray = Object.keys(this.object.translations[0].text)
      } else {
        this.textKeysArray = []
        console.log(
          'textKeysArray => textKeysArray => ',
          this.textKeysArray.length
        )
      }
    },

    createTitleKeys() {
      if (this.object?.translations?.[0]?.title ?? false) {
        console.log('createTitleKeys => titleKeysArray != 0')
        this.titleKeysArray = Object.keys(this.object.translations[0].title)
      } else {
        this.titleKeysArray = []
        console.log(
          'createTitleKeys => titleKeysArray => ',
          this.titleKeysArray.length
        )
      }
    },

    createKeysArray() {
      try {
        console.log('createKeysArray =>')
        console.log('this.textKeysArray.length =>', this.textKeysArray.length)
        console.log('this.titleKeysArray.length =>', this.titleKeysArray.length)

        if (this.textKeysArray.length >= this.titleKeysArray.length) {
          this.keysArray = _.cloneDeep(this.textKeysArray)
          console.log(
            'this keysArray textKeysArray =>',
            this.textKeysArray.length
          )
        } else {
          this.keysArray = _.cloneDeep(this.titleKeysArray)
          console.log(
            'this keysArray titleKeysArray =>',
            this.titleKeysArray.length
          )
        }

        // this.textKeysArray.length > this.titleKeysArray.length;
        //   ? ((this.keysArray = this.textKeysArray.length),
        //     console.log("this.textKeysArray =>", this.textKeysArray.length))
        //   : ((this.keysArray = this.titleKeysArray.length),
        //     console.log("this.titleKeysArray =>", this.titleKeysArray.length));
      } catch (Error) {
        console.log('Error =>', Error)
      }
    },

    //
    async createObject() {
      // if (window.location.href == "http://localhost:3000/admin/create") {
      //   this.object = _.cloneDeep(this.getEmptyObject);
      //   this.changingURL = this.getCreatingLink;
      //   //
      //   console.log("getEmptyObject =>", this.object);
      // } else {
      //   this.object = _.cloneDeep(this.getFullObject);
      //   this.changingURL = this.getEditingLink;
      //   //
      //   this.image.path = this.object.image_path;
      //   //
      //   // this.video.path = this.arr.video_path;
      //   console.log("getFullObject =>", this.object);
      // }
      this.object = _.cloneDeep(this.getObject)
      this.changingURL = this.getLink
      // this.creatingLink = this.getCreatingLink;
      // this.editingLink = this.getEditingLink;
      // this.image.path = this.object.image_path

      this.image.path = this.object.image_path
      this.video.path = this.object.video_path
      // this.gallery.path = this.object.gallery_path;
    },
  },
  methods: {
    ...mapActions('admin', ['fetchDates', 'sendDates']),

    show(event) {
      console.log('EVENT', event)
    },

    selectNameParamsArray(name) {
      this.nameParamsArray = name.array
    },

    // async updateDates(param) {333
    //   try {
    //     await this.fetchDates({ changingURL: param.link });
    //     this.takeArr;
    //   } catch (error) {
    //     console.log("updateDates UPS!", error);
    //   }
    // },

    newsPhotoSelect(event) {
      try {
        this.image.file = event.target.files[0]

        this.imgURL = ''
        this.imgFormat = ''
        /**********/
        this.image.path = URL.createObjectURL(this.image.file)
        // console.log("URL", this.image.path);
        // console.log("thisIMG", this.image);

        const { name: fileName, size } = this.image.file
        const fileSize = (size / 1000).toFixed(2)
        this.image.nameAndSize = `${fileName} - ${fileSize}KB`
      } catch (Error) {
        console.log('Error =>', Error)
      }
    },

    newsVideoSelect(event) {
      this.video.file = event.target.files[0]

      this.videoURL = ''
      // this.videoFormat = ''

      this.video.path = URL.createObjectURL(this.video.file)
      this.$refs.Video.load()

      const { name: fileName, size } = this.video.file
      const fileSize = (size / 1000).toFixed(2)
      this.video.nameAndSize = `${fileName} - ${fileSize}KB`

      // console.log("event", event.target.onload);
      //
      // console.log("REFS", this.$refs.Video);

      //
      // console.log("event", event);
    },

    closeModelForm() {
      this.$emit('closeModelForm')
    },

     editItem() {
        this.confirmBlock =  true
    },

  async  confirmEditYes(){
        // if (this.confirmYes == true){
        //   console.log("YES");
        //   this.confirmBlock = false
        // }
        // if(this.confirmNo !== true){
        //   console.log("No");
        //   this.confirmBlock = false
        // }

      try{
          this.uploadBlock = true
          
          this.uploadProsent = this.getUploadProsent

          let formData = new FormData()

          console.log('QQQQQQQQQQQq')

          formData.append('translations', JSON.stringify(this.object))

          // formData.append("image", this.image.file);

          // this.image.path;
          // this.gallery.path;

          // let token = this.$auth.strategy.token.get()

          if (this.image.path) {
            formData.append('image', this.image.file)
          }
          if (this.video.path) {
            formData.append('video', this.video.file)
          }
        
          let res = await this.sendDates({
            changingURL: this.changingURL,
            Dates: formData,
            // Token: token,
          })
          console.log("Yes");
          this.confirmBlock =  false
          
        await this.closeModelForm()

        this.uploadBlock = false
      }
      catch(Error){
        console.log("Error =>",Error);
      }
    },

     confirmEditNo(){
        console.log("No");
         this.confirmBlock =  false
    }
  },
  // watch: {
  //   object: function (val) {
  //     console.log("updated");
  //     console.log("val", val);
  //     return val;
  //   },
  // },
}

// get-header/gallery

// get-home
// get-header/home

// get-product
// get-header/product

// get-about
// get-header/about

// get-contact
// get-header/contact
</script>

<style></style>
