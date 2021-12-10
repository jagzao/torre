<template>
    <div>
    <b-card-group deck>
        <b-card 
            bg-variant="dark" 
            text-variant="white" 
            class="text-center">
        <div>
            <h3 class="m-5">{{person.name}}</h3>
            <b-img 
                center
                thumbnail 
                :src="person.picture"
                class="photo m-4"
                alt="Circle image"></b-img>

            <h4>
                Skills and interests:
            </h4>
            <hr>
            
            <h4>Master / Influencer:</h4>
            <b-button 
                pill 
                class="m-1"
                variant="secondary"

                v-for="(skill, index) in SkillsMaster"
                :key="index">
                {{skill.name}}
                <b-rating 
                    variant="warning"
                    inline 
                    no-border
                    readonly
                    v-model="skill.value"></b-rating>
            </b-button>

            <hr>

            <h4>Expert</h4>
            <b-button 
                pill
                class="m-1"
                variant="secondary"
                v-for="(skill, index) in SkillsExpert"
                :key="index">
                {{skill.name}}
                <b-rating 
                    variant="warning"
                    inline 
                    no-border
                    readonly
                    v-model="skill.value"></b-rating>
            </b-button>
            <b-button @click="modalShow = !modalShow">Open Modal</b-button>
            <b-modal v-model="modalShow">Hello From Modal!</b-modal>
            <hr>

            <h4>Proficient</h4>
            <b-button 
                pill 
                class="m-1"
                variant="secondary"
                v-for="(skill, index) in SkillsProfiencie"
                :key="index">
                {{skill.name}}
                <b-rating 
                    variant="warning"
                    inline 
                    no-border
                    readonly
                    v-model="skill.value"></b-rating>
            </b-button>
            <hr>

            <h4>Novice</h4>
            <b-button 
                pill 
                class="m-1"
                variant="secondary"
                v-for="(skill, index) in SkillsNovice"
                :key="index">
                {{skill.name}}
                <b-rating 
                    variant="warning"
                    inline 
                    no-border
                    readonly
                    v-model="skill.value"></b-rating>
            </b-button>
            <hr>

            <h4>No experience, but interests</h4>
            <b-button 
                pill 
                class="m-1"
                variant="secondary"
                v-for="(skill, index) in SkillsNoExperience"
                :key="index">
                {{skill.name}}
                
                <b-rating 
                    variant="warning"
                    inline 
                    no-border
                    readonly
                    v-model="skill.value"></b-rating>
            </b-button>
            <hr>

            <h4>Recommendations {{recommendations}}</h4>

            <h4>Languages</h4>
            <b-button 
                pill 
                class="m-1"
                variant="secondary"
                v-for="(lang, index) in Languages"
                :key="index">
                {{lang.language}} 
                <b-rating 
                    variant="warning"
                    inline 
                    no-border
                    readonly
                    v-model="lang.value"></b-rating>
            </b-button>

            <hr>

            <b-container class="bv-example-row">
                <b-row>
                    <div v-for="(xp, index) in Experiences" :key="index">
                        <b-col v-if="index % 2 === 0" class="m-2">
                            <p class="exp_title">{{xp.position}}</p>
                            <p class="exp_text">{{xp.company}}</p>
                            <p class="exp_text">
                                {{xp.from}} - {{xp.to}}
                            </p>
                        </b-col>
                        <b-col v-else class="m-2">
                            <p class="exp_title">{{xp.position}}</p>
                            <p class="exp_text">{{xp.company}}</p>
                            <p class="exp_text">
                                {{xp.from}} - {{xp.to}}
                            </p>
                        </b-col>
                    </div>
                </b-row>
            </b-container>
            
            <hr>
            <div v-show="false">
                <h4>Other people with this skill</h4>
                <ul class="list-unstyled">
                    <b-media tag="li" v-for="(item, index) in Others" :key="index">
                        <template #aside>
                            <b-img blank 
                                thumbnail 
                                :src="person.picture"
                                blank-color="#abc" width="64" alt="placeholder"></b-img>
                        </template>
                        <h5 class="mt-0 mb-1">{{item.name}}</h5>
                        <p class="mb-0">
                            {{item.professionalHeadline}}
                        </p>
                    </b-media>
                </ul>
            </div>
        </div>
    </b-card>
    </b-card-group>
    </div>
</template>
<script>
import datos from '/static/test.json'
export default{
    data() {
        return {
            person:{},
            SkillsMaster: [],
            SkillsExpert: [],
            SkillsProfiencie: [],
            SkillsNovice: [],
            SkillsNoExperience: [],
            Languages: [],
            recommendations: 0,
            Experiences: [],
            Others: []
        }
    },
    created() {
        this.Default()        
    },
    methods: {
        Default() {
            const options = {
                method: 'GET', 
                url: 'https://bio.torre.co/api/bios/jagzao',
                headers: { 'Access-Control-Allow-Origin': '*' },
            };
            this.person = datos.person
            this.SkillsMaster = this.SetData(datos, 'master')
            this.SkillsExpert = this.SetData(datos, 'expert')
            this.SkillsProfiencie = this.SetData(datos, 'proficient')
            this.SkillsNovice = this.SetData(datos, 'novice')
            this.SkillsNoExperience = this.SetData(datos, 'no-experience-interested')
            this.Languages = datos.languages.map(s => {
                return {
                    language: s.language,
                    value: this.getLangValue(s.fluency)
                }
            })
            this.Experiences = datos.jobs.map(s => {
                return {
                    position: s.name,
                    company: s.organizations[0].name,
                    from: s.fromMonth !== undefined ? `${s.fromMonth} ${s.fromYear}` : "",
                    to: s.toMonth !== undefined ? `${s.toMonth} ${s.toYear}` : ""
                }
            })
            this.Others = []

            // this.$axios.request(options)
            //     .then(function (response) {
            //         // console.log(response.data);
            //     })
            //     .catch(function (error) {
            //         console.error(error);
            //     });
        },
        SetData(info, level) {
            let filter = info.strengths
                .filter(s => s.proficiency === level)
                .map((s, index) => {
                    return {
                        proficiency: s.proficiency,
                        name: s.name,
                        recommendations: s.recommendations,
                        value: index
                    }
                })
            return filter
        },
        getLangValue(lang) {
            switch(lang) {
                case 'fully-fluent': return 5
                default: return 1
            }
        }
    }
}
</script>
<style>
.photo {
    height: 160px;
    width: 160px;
    clip-path: polygon(25% 0%, 75% 0%, 100% 50%, 75% 100%, 25% 100%, 0% 50%);
}
.form-control {
    background-color: transparent !important;
}
.exp_title {
    color: #CDDC39;
}
p {
    margin: 0;
}
</style>