<template>
  <div class="">
    <navBaar/>
    <div id="main">
        <div class="panel">
            <div class="header">
                 <router-link class="topic-tab" :to="{path:'/',query:{tab:tab.url}}"  v-for="tab in tabs" :key="tab.id" >{{tab.text}}</router-link>                        
            </div>
            <div class="topic">               
                <ul class="topic-list">
                  <li v-for="item in lists">
                      <router-link class="user_avatar pull-left" :to="`/user/${item.author.loginname}`">
                          <img v-bind:src="item.author.avatar_url" alt="">
                      </router-link >
                      <span class="reply_count pull-left">
                          <span>
                               {{item.reply_count}}
                          </span>
                          <span>
                              /
                          </span>
                          <span>
                              {{item.visit_count}}
                          </span>
                      </span>
                      <div class="pull-left">
                          <span  :class="[(item.top||item.good)?'put_top':'topiclist-tab']">
                              {{ tabFormat(item.top,item.good,item.tab) }}
                          </span>
                          <router-link :to="`/topic/${item.id}`" class="topic_title">
                              {{item.title}}
                          </router-link>
                      </div>
                      
                      <router-link :to="`/topic/${item.id}`" class="pull-right">
                          <span class="last_active_time">
                              {{item.last_reply_at | timeAgo}}
                          </span>
                      </router-link>
                  </li>
                </ul>  
            </div>
        </div>
    </div>
    
  </div>  
</template>
<script>
    import axios from 'axios';
    import navBaar from './navbar';
    
    export default {
        
      data() {
        return {
            AccessToken:'79e66ed7-6c4d-454e-99e2-d20f4d968e56',
            lists:[],
            tabs:[
                {id:1,text:"全部",url:'all'},
                {id:2,text:"精华",url:'good'},
                {id:3,text:"分享",url:'share'},
                {id:4,text:"问答",url:'ask'},
                {id:5,text:"招聘",url:'job'},                
                {id:6,text:"客户端测试",url:'dev'}
            ],
        }
      },
      components:{
          navBaar
      },
      watch:{
            $route(){
                this.lists = [];
                this.get_data();
            }       
      },
      created() {
        this.get_data();
      },
      methods: {       
        get_data: function(params) {
            var self = this; 
            if(self.$route.query.tab == undefined){
                self.$route.query.tab = 'all'; 
            }
            axios.get(`https://cnodejs.org/api/v1/topics?tab=${self.$route.query.tab}`).then(function(r){
                self.lists = r.data.data;
            })
        },
        tabFormat(top,good,tab){
            if(top){
                return "置顶"
            }            
            if(good){
                return "精华"
            }
            switch(tab){
                case 'ask':
                    return '问答'
                case 'job':
                    return '招聘'
                case 'share':
                    return '分享'
                case 'dev':
                    return '测试'
            }
        }
      },
    }
</script>