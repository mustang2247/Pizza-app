<template>
  <div class="row">
    <div class="col-sm-12 col-md-8">
      <!-- 新建pizza -->
      <new-pizza></new-pizza>
    </div>
    <div class="col-sm-12 col-md-4">
        <!-- 品种展示 -->
        <h3 class="text-muted my-5">菜单</h3>
        <table class="table">
        <thead class="table table-default">
          <tr>
            <th>品种</th>
            <th>删除</th>
          </tr>
        </thead>
        <tbody v-for="item in getMenuItems" :key="item.name">
          <tr>
            <td>{{item.name}}</td>
            <td>
              <button @click="deleteData(item)" class="btn btn-outline-danger btn-sm">&times;</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
import newPizza from "./NewPizza";
export default {
  data() {
    return {
      // MenuItems: []
    };
  },
  components: {
    newPizza
  },
  computed:{
    getMenuItems:{
      get(){
        // return this.$store.state.menuItems;
        return this.$store.getters.getMenuItems
      },
      set(){

      }
    }
  },

  created() {
    // fetch("https://wd4944622902dibypg.wilddogio.com/menu.json", {
    //   method: "GET",
    //   headers: {
    //     "Content-type": "application/json"
    //   }
    // })
    //   .then(res => res.json())
    //   .then(data => {
    //     // console.log(data);
    //     let menuArr = [];
    //     for (const key in data) {
    //       // 给每个数据绑定一个id（key，因为此处key是唯一的），用于删除数据
    //       data[key].id = key;
    //       menuArr.push(data[key]);
    //     }
    //     this.MenuItems = menuArr;
    //   });

    /**
     * 通过 axios + vuex 处理
     *
     */

    this.$axios.get('https://wd4944622902dibypg.wilddogio.com/menu.json')
      .then(res=>{
        let data = res.data;
        let menuArr = [];
        for(let key in data){
          data[key].id = key;
          menuArr.push(data[key]);
        }
        this.$store.commit('setMenuItems',menuArr);
      })

  },
  methods: {
    deleteData(item) {
      fetch(
        "https://wd4944622902dibypg.wilddogio.com/menu/" + item.id + ".json",
        {
          method: "DELETE",
          headers: {
            "Content-type": "application/json"
          }
        }
      )
        .then(res => {
          if (res.status == 200) {
            res.json();
          }
        })
        .then(data => {
          // this.$router.push("menu");
          this.$store.commit('deleteMenu',item);
        })
        .catch(err => {
          console.log(err);
        });
    }
  }
};
</script>

