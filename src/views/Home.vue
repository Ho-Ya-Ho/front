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
              <v-btn v-on:click="submit">적용</v-btn>
            </v-col>
            <v-col cols="12"
                   md="1">
              <v-btn v-on:click="getMine">조회</v-btn>
            </v-col>
          </v-row>
        </v-form>

      </v-flex>

      <v-flex v-if="likes.length>0 || dislikes.length>0" :style="{'marginTop': '50px'}">
        <v-row dense justify="center" align="center">
          <v-col cols="10"
                 md="4" class="text-subtitle-1 text-center">
            <h3>좋아하는 음식</h3>
            <a :key=i v-for="i in likes"> {{i}} </a>
          </v-col>
          <v-col cols="10"
                 md="4" class="text-subtitle-1 text-center">
            <h3>싫어하는 음식</h3>
            <a :key=i v-for="i in dislikes"> {{i}} </a>
          </v-col>
        </v-row>

      </v-flex>

      <v-flex :style="{'marginTop': '50px'}" justify="center">

        <v-row dense justify="center" align="base">
          <v-card :key= item.id v-for="item in items"  width="80%">
            <v-list-item>
              <v-col cols="12"
                md="2" class="text-center">
                <v-list-item-title>{{item.id}}</v-list-item-title>
              </v-col>

              <v-col cols="12"
                     md="4" class="text-center">
                <v-list-item-action :style="{'width':'100%'}">
                  <v-row justify="center">
                    <v-col>
                      <input type="checkbox" v-model="like_check" :value="item.id" > like
                    </v-col>
                    <v-col>
                      <input type="checkbox" v-model="dislike_check" :value="item.id"> dislike
                    </v-col>
                  </v-row>
                </v-list-item-action>
              </v-col>

              <v-col cols="12"
                     md="6" class="text-center">
                <v-list-item-title>{{item.food}}</v-list-item-title>
              </v-col>

            </v-list-item>
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
      likes: [],
      dislikes: [],
      like_check: [],
      dislike_check: [],
      items: [],
      mine: [],
      checked : []
    }),
    methods : {
      getMine() {
        //var vm = this;
        axios.get('/menu/userinfo?name='+this.name).then(res =>{
          console.log(res.data);
          this.likes = new Array
          this.dislikes = new Array
          this.like_check = new Array
          this.dislike_check = new Array

          for(let i=0; i<res.data.like.length; i++){
            this.likes.push(res.data.like[i].food);
            this.like_check.push(res.data.like[i].id);
          }
          for(let i=0; i<res.data.dislike.length; i++){
            this.dislikes.push(res.data.dislike[i].food);
            this.dislike_check.push(res.data.dislike[i].id);
          }
        });
      },
      submit() {
        axios.post()
        console.log(this.like_check);
      }
    },
    created() {
      axios.get('/menu').then(res => {
        for(let i=0; i<res.data.length; i++){
          this.items.push(res.data[i]);
        }
      }).catch(err=>{
        console.log(err);
      })
    }

  }
</script>
