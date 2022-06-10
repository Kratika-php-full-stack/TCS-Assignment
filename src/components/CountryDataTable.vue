<template>
  <div class="container">
  <h3>Assignment</h3>
    <vue-good-table
    :pagination-options="{
        enabled: true,
        mode: 'pages',
        position: 'bottom',
        perPage: 10,
    }"
    :columns="columns"
    :rows="data"
    :isLoading.sync="isLoading">
        <template slot="table-row" slot-scope="props">
            <span v-if="props.column.field == 'website'">
                 <a target="_blank" href="#" @click.prevent="goToWebsite(props.row.website)">{{props.row.website}}</a>
            </span>
            <span v-else-if="props.column.field == 'name'" style="font-weight: bold; color: blue;  cursor: pointer;">
                 <a @click.prevent="showLogo(props.row)">{{props.row.name}}</a>
            </span>
            <span v-else>
                {{props.formattedRow[props.column.field]}}
            </span>
        </template>
        <template slot="loadingContent">
          <section>
        <div class="loader">
            <span style="--i: 6"></span>
            <span style="--i: 7"></span>
            <span style="--i: 8"></span>
            <span style="--i: 9"></span>
            <span style="--i: 10"></span>
            <span style="--i: 11"></span>
            <span style="--i: 12"></span>
            <span style="--i: 13"></span>
            <span style="--i: 14"></span>
            <span style="--i: 15"></span>
            <span style="--i: 16"></span>
            <span style="--i: 17"></span>
            <span style="--i: 18"></span>
            <span style="--i: 19"></span>
            <span style="--i: 20"></span>
        </div>
    </section>
        </template>
    </vue-good-table>
    <div class="img-result" v-if="isImage">
       <p>{{selectedImgName}}<p/>
      <img :src="selectedImagePath">
    </div>
  </div>
</template>

<script>
export default {
  name: 'CountryDataTable',
  data(){
    return {
      columns: [
        {
          label: 'Name',
          field: 'name',
        },
        {
          label: 'Country',
          field: 'country',
        },
        {
          label: 'Slogan',
          field: 'slogan',
        },
        {
          label: 'Head Quarters',
          field: 'head_quaters',
        },
        {
          label: 'Website',
          field: 'website',
        },
      ],
      data: [],
      errors:[],
      totalRecords:0,
      isLoading:false,
      isImage:false,
      selectedImgName:'',
      selectedImagePath:'',
    };
  },
  async created() {
    this.isLoading = true;
    /* By using Axios We are trying to fetch the data from API on ctreated hook */
    try {
    await this.axios.get(`https://mocki.io/v1/34aa597a-35bc-4e74-9339-c8a362e34818`).then((response) => {
        this.data = response.data;
        this.totalRecords = thi.data.length;
    });
    } catch (e) {
      this.errors.push(e)
    }finally {
      this.isLoading = false;
    }
  },
  methods:{
    /*
    @Function - goToWebsite 
    @Description - This function is used to redirect to website in new tab on click of website.
    */ 
    goToWebsite(website){
      if(website){
        var pattern = /^((http|https|ftp):\/\/)/;
        if(!pattern.test(website)) {
            website = "http://" + website;
            window.open(website);
        }
      }
    },
    /*
    @Function - Show Image
    @Description - This function is used to show logo image on click of the Name 
    */ 
    showLogo(rowData){
      this.isImage = true;
      this.selectedImgName = rowData.name;
      this.selectedImagePath = rowData.logo;
    },
  }
};
</script>
<style scoped>
.container{
    margin: 0 auto;
    width: 1440px;
}
.img-result {
    margin-top: 20px;
    border: 1px solid #b9b9b9;padding:20px;
}
body {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

section {
    display: flex;
    justify-content: center;
    align-items: center;
    background-color:#00000029;
    height: 100vh;
}

.loader {
    position: relative;
    width: 120px;
    height: 120px;
    animation: animateBg;
    animation-duration: 2s;
    animation-iteration-count: infinite;
    animation-timing-function: linear;
}

@keyframes animateBg {
    0% {
        transform: rotate(0deg);
    }
    50% {
        transform: rotate(180deg);
    }
    100% {
        transform: rotate(360deg);
    }
}

.loader span {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    /* background-color: bisque; */
    transform: rotate(calc(18deg * var(--i)));
}

.loader span::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: calc(15px * (var(--i) / 20));
    height: calc(15px * (var(--i) / 20));
    border-radius: 50%;
    background-color: #fff;
}
</style>
