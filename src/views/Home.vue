<template>
  <v-container>
    <v-layout column>
      <v-flex :style="{'marginTop': '100px'}">
        <v-row dense justify="center" align="base">
          <h1>음식</h1>
        </v-row>
      </v-flex>
      <v-flex :style="{'marginTop': '50px'}">
        <v-form>
          <v-row justify="center">
            <v-col cols="12"
                   md="6">
              <v-text-field label="이름" required v-model="name"></v-text-field>
            </v-col>
            <v-col cols="12"
                   md="1">
              <v-btn v-on:click="add">추가</v-btn>
            </v-col>
            <v-col cols="12"
                   md="1">
              <v-btn>삭제</v-btn>
            </v-col>
            <v-col cols="12"
                   md="1">
              <v-btn v-on:click="getMine">조회</v-btn>
            </v-col>
          </v-row>
        </v-form>

      </v-flex>

      <v-flex v-if="mine.like.length>0 || mine.dislike.length>0" :style="{'marginTop': '50px'}" justify="center">
        <a>좋아하는 음식 :  </a>
        <span :key=i.like v-for="i in mine.like">{{i}}  </span>
        <v-spacer></v-spacer>
        <a>싫어하는 음식 :  </a>
        <span :key=i.dislike v-for="i in mine.dislike">{{i}}  </span>
      </v-flex>


      <v-flex :style="{'marginTop': '50px'}" justify="center">
        <v-row dense justify="center" align="base">
          <v-card :key= item.id v-for="item in items"  width="80%">
            <v-list-item>
              <v-list-item-action>
                <v-checkbox/>
              </v-list-item-action>
            </v-list-item>
            <v-card-text>
              <a>{{item.id}}</a>
              <a>{{item.food}}</a>
            </v-card-text>
          </v-card>
        </v-row>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import axios from 'axios'
  export default {
    name: 'Home',
    data: ()=>({
      name: '',
      items: [],
      mine: {
        like : [],
        dislike : []
      },
      checked : []
    }),
    methods : {
      getMine() {
        //var vm = this;
        axios.get('/menu/userinfo?name='+this.name).then(res =>{
          let like_ = []
          let dis_ = []
          for(let i=0; i<res.data.like.length; i++){
            like_.push(res.data.like[i].food);
          }
          for(let i=0; i<res.data.dislike.length; i++){
            dis_.push(res.data.dislike[i].food);
          }

          this.$set(this.mine, 'like', like_);
          this.$set(this.mine, 'dislike', dis_);
        });
      },
      add() {
        console.log(this.checked);
      }
    },
    created() {
      axios.get('/menu').then(res => {
        this.items = res.data
      }).catch(err=>{
        console.log(err);
      })
    }

  }
</script>
