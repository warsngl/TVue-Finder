<template lang='pug'>
.w-full.flex.flex-col.justify-center
  h1.flex.items-center.text-5xl.font-bold.mx-auto
    span TVue
    <svg xmlns="http://www.w3.org/2000/svg" version="1.0" width="50.000000pt" height="50.000000pt" viewBox="0 0 1280.000000 1228.000000" preserveAspectRatio="xMidYMid meet">
      <g transform="translate(0.000000,1228.000000) scale(0.100000,-0.100000)" fill="#4caf50" stroke="none">
        <path d="M3580 10519 c-418 -47 -855 -233 -1197 -510 -191 -155 -415 -411 -552 -631 -303 -485 -468 -1037 -512 -1713 -76 -1171 375 -2000 1524 -2802 212 -148 408 -273 822 -523 570 -344 790 -490 1082 -717 659 -511 1296 -1213 1537 -1693 29 -58 58 -121 65 -140 l13 -35 18 50 c139 397 809 1167 1560 1795 329 275 617 476 1240 865 743 464 1044 683 1364 995 484 471 737 947 833 1567 24 155 24 661 0 863 -59 490 -166 864 -357 1240 -135 265 -301 491 -511 695 -369 358 -792 567 -1309 647 -133 20 -528 17 -665 -5 -787 -131 -1435 -583 -1911 -1332 -64 -100 -179 -313 -229 -426 l-39 -86 -101 201 c-55 111 -137 260 -182 332 -497 793 -1205 1275 -2008 1364 -105 12 -378 11 -485 -1z"/>
      </g>
    </svg>
    span Finder
  input.border-2.p-6.mt-6.w-07.mx-auto.rounded-3xl.shadow.text-3xl.text-indigo-600(class='focus:outline-none focus:ring-4 focus:ring-light-blue-600' @keyup.enter='S(s)' v-model='s' placeholder='Type to search...')
  .flex.flex-wrap.box-border.justify-center
    h1.text-red-200.text-6xl.mt-6(v-if='e') {{e}}
    card(v-for='i,idx in iss' :key='idx' :i='i')
</template>

<script>
export default {
  data:()=>({
    s:'',
    iss:[],
    loading:false,
    e:false
  }),
  methods:{
    async S(s){
      this.loading=true
      this.e=false
      this.iss=[]
      await this.$axios.get(`https://www.omdbapi.com/?s="${s}"&apikey=c5cc3d12`)
        .then(r=>{
          r.data.Response=='False'? this.e=r.data.Error : this.iss=r.data.Search
        })
        .catch(e=>console.log(e))
      this.loading=false
    }
  }
}
</script>

<style>
.w-02{
  width: 20%;
}
.w-07{
  width: 70%;
}
</style>