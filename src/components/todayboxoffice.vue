<template>
  <div>
      오늘의 박스 오피스 들어올곳
      <div class="row">
        <table class="table">
        <thead>
          <tr>
            <th scope="col">순위</th>
            <th scope="col">제목</th>
            <th scope="col">순위 변동</th>
            <th scope="col">신규진입</th>
            <th scope="col">자세히</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="boxoffice in boxoffices" :key="boxoffice.rnum">
            <th scope="row">{{boxoffice.rank}}</th>
            <td> {{boxoffice.movieNm}}</td>
            <td> {{boxoffice.rankInten}}</td>
            <td> {{boxoffice.rankOldAndNew}}</td>
            <td> <button type="submit" class= 'btn-btn-primary'  :box='boxoffice' @click.prevent="mvdetail(boxoffice)">클릭</button></td>
          </tr>
        </tbody>
        </table>
      </div>
      
      
      
      
      
      
  </div>
</template>

<script>
import axios from 'axios'
const API_URL='http://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json?key='

// &targetDt=20120101
let today = new Date();   
today.setDate(today.getDate()-1);
const year = today.getFullYear(); // 년도
const month = ("0" + (today.getMonth() + 1)).slice(-2);  // 월
const date =("0" + (today.getDate()  )).slice(-2);  // 날짜



export default {
    name : 'todayboxoffice',
    data(){
        return{
            boxoffices:[]
        }
    },
    created(){
        axios.get(API_URL+process.env.VUE_APP_KOBIS_KEY+'&targetDt='+year+month+date)
        .then(res=> {
            console.log(res.data.boxOfficeResult.dailyBoxOfficeList)
            this.boxoffices = res.data.boxOfficeResult.dailyBoxOfficeList
            console.log(process.env.VUE_APP_SERVER_URL)
        })
        .catch(err=>console.log(err))
    },
    methods:{
        mvdetail(box){
            this.$emit('submit-box-data',box)
        }
    }
}
</script>

<style>
</style>