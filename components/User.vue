<template>
    <div>
        <p>foto</p>
        <p>nombre</p>

        <h3>Skills and interests:</h3>
        
        <h4>Master / Influencer:</h4>
        <b-button 
            pill 
            variant="secondary"
            v-for="(skill, index) in SkillsMaster"
            :key="index">
            Secondary
        </b-button>

        <h4>Proficient</h4>
    </div>
</template>
<script>
import datos from '/static/test.json'
export default{
    data() {
        return {
            user:{},
            SkillsMaster: [],
            SkillsProfiencie: [],
            SkillsMaster: [],
        }
    },
    created() {
        this.Default()        
    },
    methods: {
        Default() {
            console.log(datos)
            const options = {
                method: 'GET', 
                url: 'https://bio.torre.co/api/bios/jagzao'
            };
            this.$axios.request(options)
                .then(function (response) {
                    // console.log(response.data);
                })
                .catch(function (error) {
                    console.error(error);
                });
        },
        GetSkills() {

        }
    }
}
</script>
