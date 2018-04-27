<template>
  <div>
    <v-header :user="user"></v-header>
    <el-menu :default-active="$route.path" router>
      <el-submenu v-for="(first, index) in $router.options.routes[0].children" :key="index" :index="first.path">
        <template slot="title">
          <span>{{first.name}}</span>
        </template>                
        <el-menu-item-group v-for="item in first.children" v-if="!item.hidden" :key="item.path">
          <el-menu-item :index="'/' + first.path + '/' +item.path"><i class="fa" :class="item.class"></i>{{item.name}}</el-menu-item>
        </el-menu-item-group>
      </el-submenu>
    </el-menu>
    <section>
        <transition>
          <router-view></router-view>
        </transition>
    </section>
  </div>
</template>

<script>
  import header from './views/header/header.vue';
  const ERR_OK = "000";
  export default {
    data () {
      return {
        user: {}
      };
    },
    created () {
      console.log(this.$route, this.$route.path.split('/')[1]);
      this.$http.get('/api/user').then((response) => {
        response = response.data;
        if (response.code === ERR_OK) {
          this.user = response.datas;
        }
      });
    },
    beforeCreate () {
      if (this.$route.path === '/') {
        this.$router.push({path: '/index'})
      }
    },
    components: {
      'v-header': header
    }
  };
</script>
<style>

</style>

