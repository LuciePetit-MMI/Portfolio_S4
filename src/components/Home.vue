<template>
    <div>
        <header class="tablet:relative w-full">
            <div v-for="about in about" :key="about.id">
                <div class="pt-40 h-screen relative flex justify-center items-baseline">
                    <div class="flex flex-col justify-center items-center tablet:absolute tablet:left-5 laptop:bottom-72">
                        <h1>{{about.title}}</h1>
                        <h3>{{about.sous_titre}}</h3>   
                    </div>
                    <img :src="about.urlImage" :alt="about.altImage" class="absolute bottom-0 h-full z tablet:-right-20 laptop:right-96">
                    <a href="#main" class="absolute bottom-10 font-Indie text-xl tablet:left-10 tablet:bottom-20 laptop:bottom-60"><i class="fa fa-solid fa-arrow-down-long pr-4 animate-bounce"></i>Have a look</a>  
                </div>
                <div class="my-10 tablet:absolute tablet:left-0 tablet:bottom-28 tablet:w-60 tablet:text-left laptop:bottom-64 laptop:left-3/4">
                    <p>{{about.description}}</p>    
                </div>
            </div>
        </header>
        <main id="main">
            <section v-for="projects in listProjects" :key="projects.id" id="webdesign" class="my-40 relative">
                <div class="tablet:hidden">
                    <h2>{{projects.title}}</h2>
                    <h3 class="mx-auto">{{projects.sous_titre}}</h3>
                    <flickity class="main-carousel" ref="flickity" :options="flickityOptions">
                        <div v-for="projet in projects.details" :key="projet.id" class="carousel-cell">
                            <div class="h-96 overflow-hidden flex justify-center items-center relative">
                                <img class="object-cover h-full w-full" :src="projet.urlImage" :alt="projet.altImage">
                                <button class="btn absolute bottom-10" @click="openProject(projet, projet.type)" >View more</button>
                            </div>
                        </div>
                    </flickity>
                </div>
                <div id="desktop" class="hidden tablet:grid tablet:grid-cols-4 tablet:gap-4 tablet:mx-auto">
                    <div v-for="(projet, index) in projects.details" :key="index" v-if="index<5" class="relative cursor-pointer">
                        <div class="overflow-hidden flex flex-col justify-center items-center h-40 w-full laptop:h-60">
                            <img class="object-cover h-full w-full" :src="projet.urlImage" :alt="projet.altImage">
                        </div>
                        <div class="opacity-0 hover:opacity-100 flex h-40 w-full bg-dark bg-opacity-70 absolute top-0 left-0 flex-col justify-center items-center laptop:h-60">
                            <h4>{{projet.title}}</h4>
                            <button @click="openProject(projet, projet.type)" class="btn">See details</button>
                        </div>
                    </div>
                    <div class="overflow-hidden flex flex-col justify-center items-center h-40 w-full laptop:h-60">
                        <h2>{{projects.title}}</h2>
                        <h3>{{projects.sous_titre}}</h3>    
                    </div>
                    <div v-for="(projet, index) in projects.details" :key="index" v-if="index>=5" class="relative cursor-pointer">
                        <div class="overflow-hidden flex flex-col justify-center items-center h-40 w-full laptop:h-60">
                            <img class="object-cover h-full w-full" :src="projet.urlImage" :alt="projet.altImage">
                        </div>
                        <div class="opacity-0 hover:opacity-100 flex h-40 w-full bg-dark bg-opacity-70 absolute top-0 left-0 flex-col justify-center items-center laptop:h-60">
                            <h4>{{projet.title}}</h4>
                            <button @click="openProject(projet, projet.type)" class="btn">See details</button>
                        </div>
                    </div>
                </div>
                <div id="details" :class="{hidden: !open, block: open}" class="z-50">
                    <div v-show="currentProjet.type=='site'" class="z-50 py-10 w-full bg-dark bg-opacity-90 absolute top-0 left-0 px-4 laptop:px-6">
                        <i @click="open=!open" class="fa-solid fa-arrow-left fa-2x flex self-start cursor-pointer"></i>
                        <div class="flex flex-col">
                            <div class="flex flex-col text-left">
                                <h4>{{currentProjet.title}}</h4>
                                <p>{{currentProjet.description}}</p>
                                <h5 class="mt-10">Main functionalities</h5>
                                <div class="flex flex-col tablet:flex-row tablet:justify-evenly">
                                    <div class="flex flex-col tablet:flex-row tablet:justify-evenly">
                                        <div class="text-center functionality tablet:flex tablet:justify-evenly tablet:w-full tablet:items-center">
                                            <p v-for="functionality in currentProjet.functionalities" :key="functionality.id">{{functionality.description}}</p>
                                        </div>
                                    </div>
                                    <div class="grid grid-cols-2 gap-3 my-4 text-left tablet:w-56">
                                        <div v-for="techno in currentProjet.technos" :key="techno.id" class="flex items-center"><img class="w-10 h-10 mr-2" :src="techno.urlIcon" :alt="techno.altIcon"><p class="uppercase">{{techno.nom}}</p></div>
                                    </div>  
                                </div>
                                <a :href="currentProjet.lien" target="blank" class="btn text-center self-center">Visite Website</a>
                            </div> 
                        </div>
                        <img class="my-4 object-cover w-full h-96 mx-auto" :src="currentProjet.urlMockup" :alt="currentProjet.altMockup">
                    </div>
                    <div v-show="currentProjet.type=='branding'" class="z-50 py-10 w-full bg-dark bg-opacity-90 absolute top-0 left-0 px-4 laptop:px-6">
                        <i @click="open=!open" class="fa-solid fa-arrow-left fa-2x flex self-start cursor-pointer"></i>
                        <div class="flex flex-col tablet:flex-row tablet:justify-evenly items-center">
                            <div class="flex flex-col text-left tablet:w-80 laptop:w-2/6">
                                <h4>{{currentProjet.title}}</h4>
                                <p>{{currentProjet.description}}</p>
                                <div class="flex justify-evenly w-full">
                                    <a :href="currentProjet.maquette" target="blank" class="btn text-center self-center my-4">View Mockup</a>
                                    <a v-show="currentProjet.dossier" :href="currentProjet.dossier" target="blank" class="btn text-center self-center my-4">View Files</a>
                                </div>
                            </div>
                            <img class="my-4 tablet:w-80 laptop:w-2/6" :src="currentProjet.urlMoodboard" :alt="currentProjet.altMoodboard">
                        </div>
                        <img class="my-4 object-cover w-full h-96 mx-auto" :src="currentProjet.urlMockup" :alt="currentProjet.altMockup">
                    </div>
                    <div v-show="currentProjet.type=='boss'" class="z-50 py-10 w-full bg-dark bg-opacity-90 absolute top-0 left-0 px-4 laptop:px-6">
                        <i @click="open=!open" class="fa-solid fa-arrow-left fa-2x flex self-start cursor-pointer"></i>
                        <div class="flex flex-col">
                            <div class="flex flex-col tablet:flex-row text-left tablet:justify-between">
                                <div class="w-3/4">
                                    <h4>{{currentProjet.title}}</h4>
                                    <p>{{currentProjet.description}}</p>
                                    <h5 class="mt-10">Content reduired</h5>
                                    <div class="flex flex-col tablet:flex-row">
                                        <div class="text-center functionality tablet:flex tablet:justify-evenly">
                                            <p v-for="contenu in currentProjet.contenu" :key="contenu.id">{{contenu.description}}</p>
                                        </div>
                                    </div>
                                    <h5 class="mt-10">Technology required</h5>
                                    <div class="flex flex-col tablet:flex-row">
                                        <div class="text-center functionality tablet:flex tablet:justify-evenly mx-auto">
                                            <p v-for="techno in currentProjet.technos_requise" :key="techno.id">{{techno.nom}}</p>
                                        </div>
                                    </div>
                                    <div class="flex justify-evenly w-full">
                                        <a :href="currentProjet.maquette" target="blank"  class="btn text-center self-center my-4">View Mockup</a>
                                        <a :href="currentProjet.lien" target="blank"  class="btn text-center self-center my-4">Visite Website</a>
                                    </div>                            
                                </div>
                                <div class="flex flex-col">
                                    <h5>What I used</h5>
                                    <div class="grid grid-cols-1 gap-3 my-4 text-left tablet:w-40">
                                        <div v-for="techno in currentProjet.technos_utilise" :key="techno.id" class="flex items-center"><img class="w-10 h-10 mr-2" :src="techno.urlIcon" :alt="techno.altIcon"><p class="uppercase">{{techno.nom}}</p></div>
                                    </div>  
                                </div>
                            </div> 
                        </div>
                        <img class="my-4 object-cover w-full h-96 mx-auto" :src="currentProjet.urlMockup" :alt="currentProjet.altMockup">
                    </div>
                    <div v-show="currentProjet.type=='logo'" class="z-50 py-10 w-full bg-dark bg-opacity-90 absolute top-0 left-0 px-4 laptop:px-6">
                        <i @click="open=!open" class="fa-solid fa-arrow-left fa-2x flex self-start cursor-pointer"></i>
                        <div class="flex flex-col text-left">
                            <h4>{{currentProjet.title}}</h4>
                            <p>{{currentProjet.description}}</p>
                            <div class="grid grid-cols-1 gap-3 mx-auto tablet:grid-cols-5">
                                <div v-for="logos in currentProjet.logos" :key="logos.id" class="w-40">
                                    <div class="w-40 h-40 overflow-hidden max-w-40">
                                        <img @click="currentPic(logos.urlImage, logos.altImage, 'logos')" :src="logos.urlImage" :alt="logos.altImage" class="cursor-pointer object-fit h-full w-auto max-w-none max-h-full mx-auto">
                                    </div>
                                    <p>{{logos.description}}</p>
                                </div>
                            </div>
                        </div>
                        <div :class="{hidden: !show_logos, block: show_logos}" class="py-10 w-full h-full bg-dark bg-opacity-90 absolute z-50 top-0 left-0">
                            <i @click="show_logos=!show_logos" class="fa-solid fa-arrow-left fa-2x flex self-start cursor-pointer"></i>
                            <div class="w-96 my-auto mx-auto h-full">
                                <img :src="photoUrl" :alt="photoAlt" class="object-cover max-h-full mx-auto">
                            </div>
                        </div>
                    </div>
                    <div v-show="currentProjet.type=='video'" class="z-50 py-10 w-full bg-dark bg-opacity-90 absolute top-0 left-0 px-4 laptop:px-6">
                        <i @click="open=!open" class="fa-solid fa-arrow-left fa-2x flex self-start cursor-pointer"></i>
                        <div class="flex flex-col tablet:flex-row tablet:justify-evenly items-center">
                            <div class="flex flex-col text-left tablet:w-80 laptop:w-2/6">
                                <h4>{{currentProjet.title}}</h4>
                                <p>{{currentProjet.description}}</p>
                            </div>
                            <img v-show="currentProjet.urlMoodboard" class="my-4 tablet:w-80 laptop:w-2/6" :src="currentProjet.urlMoodboard" :alt="currentProjet.altMoodboard">
                        </div>
                        <iframe class="my-4 object-cover w-full h-96 mx-auto" :src="currentProjet.video"></iframe>
                    </div>
                </div>
            </section>

            <section v-for="photos in listPhotos" :key="photos.id" id="photography" class="my-40 relative">
                <div class="tablet:hidden">
                    <h2>{{photos.title}}</h2>
                    <h3 class="mx-auto">{{photos.sous_titre}}</h3>
                    <flickity class="main-carousel" ref="flickity" :options="flickityOptions">
                        <div  v-for="photo in photos.details" :key="photo.id" class="carousel-cell">
                            <div class="h-96 overflow-hidden flex justify-center items-center relative">
                                <img class="object-cover h-full w-full" :src="photo.urlImage" :alt="photo.altImage">
                                    <button class="btn absolute bottom-10" @click="currentPic(photo.urlImage, photo.altImage, 'photo')">Show</button>
                            </div>
                        </div>
                    </flickity>
                </div>
                <div id="desktop" class="hidden tablet:grid tablet:grid-cols-4 tablet:gap-4 tablet:mx-auto">
                    <div v-for="(photo, index) in photos.details" :key="index" v-if="index<5" class="cursor-pointer">
                        <div class="overflow-hidden flex flex-col justify-center items-center h-40 w-full laptop:h-60">
                            <img @click="currentPic(photo.urlImage, photo.altImage, 'photo')" class="object-cover h-full w-full" :src="photo.urlImage" :alt="photo.altImage">
                        </div>
                    </div>
                    <div class="overflow-hidden flex flex-col justify-center items-center h-40 w-full laptop:h-60">
                        <h2>{{photos.title}}</h2>
                        <h3>{{photos.sous_titre}}</h3>    
                    </div>
                    <div v-for="(photo, index) in photos.details" :key="index" v-if="index>=5" class="relative cursor-pointer">
                        <div class="overflow-hidden flex flex-col justify-center items-center h-40 w-full laptop:h-60">
                            <img @click="currentPic(photo.urlImage, photo.altImage, 'photo')" class="object-cover h-full w-full" :src="photo.urlImage" :alt="photo.altImage">
                        </div>
                    </div>
                </div>
                <div :class="{hidden: !show_pic, block: show_pic}" class="py-10 w-full h-full bg-dark bg-opacity-90 absolute z-50 top-0 left-0">
                    <i @click="show_pic=!show_pic" class="fa-solid fa-arrow-left fa-2x flex self-start cursor-pointer"></i>
                    <div class="my-auto mx-auto h-full">
                        <img :src="photoUrl" :alt="photoAlt" class="object-cover max-h-full mx-auto">
                    </div>
                </div>
            </section>

            <section v-for="illustrations in listIllustrations" :key="illustrations.id" id="illustration" class="my-40 relative">
                <div class="tablet:hidden">
                    <h2>{{illustrations.title}}</h2>
                    <h3 class="mx-auto">{{illustrations.sous_titre}}</h3>
                    <flickity class="main-carousel" ref="flickity" :options="flickityOptions">
                        <div v-for="illustration in illustrations.details" :key="illustration.id" class="swiper-slide">
                            <div class="h-96 overflow-hidden flex justify-center items-center relative">
                                <img class="object-cover h-full w-full" :src="illustration.urlImage" :alt="illustration.altImage">
                                <button class="btn absolute bottom-10" @click="currentPic(illustration.urlImage, illustration.altImage, 'illu')">Show</button>
                            </div>
                        </div>
                    </flickity>  
                </div>
                <div id="desktop" class="hidden tablet:grid tablet:grid-cols-4 tablet:gap-4 tablet:mx-auto">
                    <div v-for="(illustration, index) in illustrations.details" :key="index" v-if="index<5" class="relative cursor-pointer">
                        <div class="overflow-hidden flex flex-col justify-center items-center h-40 w-full laptop:h-60">
                            <img @click="currentPic(illustration.urlImage, illustration.altImage, 'illu')" class="object-cover h-full w-full" :src="illustration.urlImage" :alt="illustration.altImage">
                        </div>
                    </div>
                    <div class="overflow-hidden flex flex-col justify-center items-center h-40 w-full laptop:h-60">
                        <h2>{{illustrations.title}}</h2>
                        <h3>{{illustrations.sous_titre}}</h3>    
                    </div>
                    <div v-for="(illustration, index) in illustrations.details" :key="index" v-if="index>=5" class="relative cursor-pointer">
                        <div class="overflow-hidden flex flex-col justify-center items-center h-40 w-full laptop:h-60">
                            <img @click="currentPic(illustration.urlImage, illustration.altImage, 'illu')" class="object-cover h-full w-full" :src="illustration.urlImage" :alt="illustration.altImage">
                        </div>
                    </div>
                </div>
                <div :class="{hidden: !show_illu, block: show_illu}" class="py-10 w-full h-full bg-dark bg-opacity-90 absolute z-50 top-0 left-0">
                    <i @click="show_illu=!show_illu" class="fa-solid fa-arrow-left fa-2x flex self-start cursor-pointer"></i>
                    <div class="my-auto mx-auto h-full">
                        <img :src="photoUrl" :alt="illuAlt" class="object-fit max-h-full mx-auto">
                    </div>
                </div>
            </section>

            <section v-for="contact in listContact" :key="contact.id" id="contact" class="my-40 flex flex-col justify-center items-center laptop:w-3/5 laptop:mx-auto">
                <h2>{{contact.title}}</h2>
                <h3 class="align-center">{{contact.sous_titre}}</h3>
                <div v-if="errors.length">
                    <li class="list-none text-red" v-for="error in errors" :key="error.id">{{error}}</li>
                </div>
                <form action="" class="flex flex-col items-end w-10/12">
                <input v-model="form.full_name" type="text" :placeholder="contact.name"> 
                <input v-model="form.email" type="text" :placeholder="contact.mail"> 
                <textarea v-model="form.body" name="" id="" cols="30" rows="5" :placeholder="contact.message"></textarea>
                <input @click.prevent="submit" type="submit" :value="contact.bouton" class="btn pl-0">
                </form>
            </section>
        </main>
        <footer v-for="footer in listFooter" :key="footer.id">
            <div class="flex flex-col items-start text-left mt-10 tablet:flex-row tablet:justify-around">
                <div>
                    <h5>{{footer.title_1}}</h5>
                    <p v-for="section in footer.section" :key="section.id"><a :href="section.lien">{{section.name}}</a></p>
                </div>
                <div>
                    <h5>{{footer.title_2}}</h5>
                    <a href="#contact"><i class="fa fa-light fa-envelope fa-2x px-2 hover:text-primary"></i></a>
                    <a href="instagram.com/schefflera_logo/?hl=fr" target="blank"><i class="fa fa-brands fa-instagram fa-2x px-2 hover:text-primary"></i></a>
                    <a href="https://www.linkedin.com/in/lucie-petit-03647115b/" target="blank"><i class="fa fa-brands fa-linkedin fa-2x px-2 hover:text-primary"></i></a>
                    <a href="https://www.facebook.com" target="blank"><i class="fa fa-brands fa-facebook fa-2x px-2 hover:text-primary"></i></a>            
                </div>    
            </div>
            <div class="text-center w-full">
                <p class="text-light my-4">{{footer.copyrights}}</p>
            </div>
        </footer>
    </div>
</template>

<script>
import param from '@/param/param'
import Flickity from 'vue-flickity'

export default {
    name: 'Home',
    data () {
        return {
            about: [],
            open:false,
            show_illu:false,
            show_pic:false,
            show_logos:false,
            listProjects: [],
            listIllustrations: [],
            listPhotos: [],
            listContact: [],
            listFooter: [],
            photoUrl: "",
            photoAlt: "",
            illuUrl: "",
            illuAlt: "",
            currentProjet: {},
            form: {
                body : '',
                email: '',
                full_name: '',
            },
            errors : [],
            flickityOptions: {
                cellAlign: 'left',
                contain: true,
                prevNextButtons: false,
                pageDots: true,
                draggable: true,
                fade: true,
                autoPlay: 2000,
                imagesLoaded: true,
                lazyLoad: true,
            }
        }
    },
    components: {
        Flickity
    },
    created(){
    //liste about
        axios({
            method: 'get',
            url : param.hostSpe+"listAbout",
        }).then(response=>{
            this.about = response.data;
        })
        .catch(error => console.log ("Erreur about", error));
    //liste projets
        axios({
            method: 'get',
            url : param.hostSpe+"listWebdesignsAll",
        }).then(response=>{
            this.listProjects = response.data;
        })
        .catch(error => console.log ("Erreur projects", error));
    //list Illustrations
        axios({
            method: 'get',
            url : param.hostSpe+"listIllustrations",
        }).then(response=>{
            this.listIllustrations = response.data;
        })
        .catch(error => console.log ("Erreur illustration", error));
    //list Photos
        axios({
            method: 'get',
            url : param.hostSpe+"listPhotos",
        }).then(response=>{
            this.listPhotos = response.data;
        })
        .catch(error => console.log ("Erreur photos", error));
    //list Contact
        axios({
            method: 'get',
            url : param.hostSpe+"listContact",
        }).then(response=>{
            this.listContact = response.data;
        })
        .catch(error => console.log ("Erreur contact", error));
    //list Footer
        axios({
            method: 'get',
            url : param.hostSpe+"listFooter",
        }).then(response=>{
            this.listFooter = response.data;
        })
        .catch(error => console.log ("Erreur footer", error));
    },
    methods: {
        currentPic(src,alt,type){
            if(type=='photo'){
                this.show_pic                                   =! this.show_pic;
            }
            if(type=='illu'){
                this.show_illu                                   =! this.show_illu;
            }
            if(type=='logos'){
                this.show_logos                                   =! this.show_logos;
            }
            this.photoUrl                                   = src;
            this.photoAlt                                   = alt;
        },
        openProject(data, type){
            this.currentProjet = {};
            this.open                           =! this.open;
            let projet = this.currentProjet;
            
            if(type==='site'){
                projet.type                     = data.type;
                projet.title                    = data.site[0].title;
                projet.description              = data.site[0].description;
                projet.urlMockup                = data.site[0].urlMockup;
                projet.altMockup                = data.site[0].altMockup;
                projet.lien                     = data.site[0].url;

                let tableau                     = data.site[0];
                let functionalities             = data.site[0].functionalities.id;
                let techno                      = data.site[0].technos.id;

                for(functionalities in tableau){
                    projet.functionalities      = tableau.functionalities;
                }
                for(techno in tableau){
                    projet.technos              = tableau.technos;
                }
                return

            }
            if(type==='branding'){
                projet.type                     = data.type;
                projet.title                    = data.branding[0].title;
                projet.description              = data.branding[0].description;
                projet.urlMoodboard             = data.branding[0].urlMoodboard;
                projet.altMoodboard             = data.branding[0].altMoodboard;
                projet.urlMockup                = data.branding[0].urlMockup;
                projet.altMockup                = data.branding[0].altMockup;
                projet.maquette                 = data.branding[0].maquette;
                projet.dossier                  = data.branding[0].dossier;
            }
            if(type==='boss'){
                projet.type                     = data.type;
                projet.title                    = data.boss[0].title;
                projet.description              = data.boss[0].description;
                projet.urlMockup                = data.boss[0].urlMockup;
                projet.altMockup                = data.boss[0].altMockup;
                projet.maquette                 = data.boss[0].maquette;
                projet.lien                     = data.boss[0].site;

                let tableau = data.boss[0];
                let content = data.boss[0].contenu.id;
                let technoR = data.boss[0].technos_requise.id;
                let technoU = data.boss[0].technos_utilise.id;

                for(content in tableau){
                    projet.contenu              = tableau.contenu;
                }
                for(technoR in tableau){
                    projet.technos_requise      = tableau.technos_requise;
                }
                for(technoU in tableau){
                    projet.technos_utilise      = tableau.technos_utilise;
                }
                return
            }
            if(type==='logo'){
                projet.type                     = data.type;
                projet.title                    = data.title;
                projet.description              = data.description;

                projet.logos = [];
                let tableau = data.TDLC;

                for(let i=0;i<tableau.length;i++){
                    projet.logos[i]             = tableau[i];
                }
                return
            }
            if(type==='video'){
                    projet.type                     = data.type;
                    projet.title                    = data.video[0].title;
                    projet.description              = data.video[0].description;
                    projet.urlMoodboard             = data.video[0].urlMoodboard;
                    projet.altMoodboard             = data.video[0].altMoodboard;
                    projet.video                    = data.video[0].video;
                }
            return
        },
        submit: function(){
            axios.post(param.hostSpe+"Contact", this.form)
            .then((response) => {
                console.log(response);
                this.errors = [];
            })
            .catch((error)=> {
                this.errors = error.response.data.message;
            })
        }
    },
    mounted() {
        window.resizeTo(
            window.screen.availWidth,
            window.screen.availHeight
        );
    }
}
</script>
