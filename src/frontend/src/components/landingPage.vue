<template>
<userHeader subHeaderName="" />
<div class="flex min-h-screen">
    <div class="w-1/5 bg-gray-200">
        <div class="flex h-10 bg-gray-200"></div>
        <div class="flex h-10 bg-gray-200"></div>
        <div v-for="(countryDetail,i) in countryDetailsArray" :key="i">
            <button class="text-white w-full bg-blue-400 hover:bg-red-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-1 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800" v-on:click="displayUpcomingMatchDetails(countryDetail.countryName)">
                <div class="flex h-8">
                    <img :src="countryDetail.flag" width="30" height="30" /><b>{{ countryDetail.countryName }}</b>
                </div>
            </button>
        </div>
    </div>
    <!---->
    <div class="flex-1 bg-red-200 container">
        <div class="flex h-13">
            <input type="text" class="p-2 border border-gray-300 rounded-l-md w-full" placeholder="Enter a team name..." />
            <button class="bg-blue-400 hover:bg-blue-600 text-white p-2 rounded-r-md">Search</button>
            <button class="bg-blue-400 hover:bg-blue-600 text-white p-2 rounded-r-md">Reset</button>
        </div>
        <nav class="bg-blue-300 border-gray-200 px-1 lg:px-2 py-1 text-white p-5 flex justify-center">
            <button class="hover:bg-gray-600 text-white py-1 px-1 rounded" type="button"><b>Upcoming Events</b></button>
            <button class="hover:bg-gray-600 text-white py-1 px-1 rounded" type="button"><b>Live</b></button>
        </nav>
       
        <!---->
        <div v-for="(countryDetail, i) in countryDetailsArray" :key="i">
        <div class="overflow-x-scroll">
            <div class="bg-green-200 h-8 flex border border-gray-300 justify-center container">
                <img :src="countryDetail.flag" width="30" height="30" /><b>{{ countryDetail.countryName }}</b>
            </div>
            <div class="flex">
                <div v-for="(matchDetail, j) in matchDetailsArray" :key="j" class="flex-shrink-0">
                    <div v-if="countryDetail.countryName === matchDetail.hostingCountry">
                        <div class="border border-gray-600">
                            <div class="flex text-sm">
                                <img :src="matchDetail.competitionEmblem" width="30" height="30" /><b>{{ matchDetail.competitionName }}</b>
                            </div>
                            <div>
                                <span class="text-blue-500 text-sm font-bold">Match day:</span> 
                                <span class="text-sm font-semibold ml-1">{{ (matchDetail.matchDateandTime).slice(0, 10) }}</span>
                            </div>
                            <div>
                                <span class="text-blue-500 text-sm font-bold">Match time:</span> 
                                <span class="text-sm font-semibold ml-1">{{ (matchDetail.matchDateandTime).slice(11, 16) }}</span>
                            </div>
                            <div class="grid grid-cols-3 gap-1 w-full text-sm">
    
                                <div class="bg-gray-200 p-2 font-semibold">Teams (Home vs Away)</div>
                                <div class="bg-gray-200 p-2 flex">
                                    <img :src="matchDetail.homeTeamCrest" width="10" height="10" />{{ matchDetail.homeTeamName }}
                                </div>
                                <div class="bg-gray-200 p-2 flex">
                                    <img :src="matchDetail.awayTeamCrest" width="10" height="10" />{{ matchDetail.awayTeamName }}
                                </div>
                                <div class="bg-gray-200 p-2 font-semibold">Winning Odds</div>
                                <div class="bg-gray-200 p-2">{{ matchDetail.homeTeamWinningOdds }}</div>
                                <div class="bg-gray-200 p-2">{{ matchDetail.awayTeamWinningOdds }}</div>
                                <div class="bg-gray-200 p-2 font-semibold">Your bet</div>
                                <div class="bg-gray-200 p-2">
                                    <input type="number" step=".1" class="bg-gray-100 p-2"/>
                                </div>
                                <div class="bg-gray-200 p-2">
                                    <input type="number" step=".1" class="bg-gray-100 p-2"/>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div v-else></div>
                </div>
            </div>
    
            <br />
        </div>
    
    </div>
    </div>
</div>
</template>

<script lang="ts">
import {defineComponent} from 'vue';
import userHeader from './userHeader.vue'
import countryDetails from '../types/countryDetails'
import matchDetails from '../types/matchDetails'
import {useStore} from 'vuex';

export default defineComponent({
    name: 'landingComp',
    data() {
        return {
            countryDetailsArray: [] as countryDetails[],
            matchDetailsArray:[] as matchDetails[],
            clickedCity: '',
            store: useStore()
        }
    },
    components: {
        userHeader
    },
    methods:{
        displayUpcomingMatchDetails(countryName:string):void{
            this.clickedCity = countryName;
        }
    },
    mounted() {
        this.countryDetailsArray = this.store.state.countryDetailsArray;
        this.matchDetailsArray = this.store.state.matchDetailsArray;
    }
})
</script>

<style scoped>
</style>
