<template>
  <div class="sidebar" :class="{'collapse': isCollapse}">
    <el-menu
      :default-active="onRoutes"
      class="el-menu-vertical-demo"
      theme="dark"
      unique-opened
      router
    >
      <template v-for="(item, i) in items">
        <template v-if="item.subs">
          <el-submenu :index="item.index" :key="i">
            <template slot="title">
              <i :class="item.icon"></i>
              <span class="menu-title">{{ item.title }}</span>
            </template>
            <el-menu-item
              v-for="(subItem,i) in item.subs"
              :key="i"
              :index="subItem.index"
            >
              <span class="menu-title">{{ subItem.title }}</span>
            </el-menu-item>
          </el-submenu>
        </template>
        <template v-else>
          <el-menu-item :index="item.index" :key="i">
            <i :class="item.icon"></i>
            <span class="menu-title">{{ item.title }}</span>
          </el-menu-item>
        </template>
      </template>
    </el-menu>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isCollapse: true,
      items: [
        {
          icon: "el-icon-menu",
          index: "/Situation",
          title: "博客概况"
        },

        {
          icon: "el-icon-edit",
          index: 'article',
          title: "我的文章",
          subs: [
            {
              icon: "el-icon-document",
              index: "/article",
              title: "我写的"
            },
            {
              icon: "el-icon-edit",
              index: "/publish",
              title: "发布文章"
            },
            {
              icon: "el-icon-upload",
              index: "/uploadPublish",
              title: "上传Markdown"
            }
          ]
        },

        {
          icon: "el-icon-menu",
          index: "activity",
          title: "动态",
          subs: [
            {
              index: "/activity",
              title: "我的动态"
            },
            {
              index: "/activity/add",
              title: "发布动态"
            }
          ]
        },
        {
          icon: "el-icon-star-on",
          index: "/cate",
          title: "分类管理"
        },
        {
          icon: "el-icon-setting",
          index: "/ManageBlog",
          title: "管理博客"
        }
      ]
    };
  },
  computed: {
    onRoutes() {
      return this.$route.path.replace("/", "");
    }
  },
  created() {
    //先判断是否已经登陆
    const that = this;
    that.$axios.get(that.$API.Admin.getAdminInfo).then(res => {
      let admin = res.data.data;
      if (admin.islogin == 0) {
        console.log("未登录");
        that.$router.push("/login");
      }
    });
  }
};
</script>

<style scoped>
.sidebar {
  display: block;
  position: absolute;
  width: 250px;
  left: 0;
  top: 70px;
  bottom: 0;
  background: #2e363f;
}
.sidebar > ul {
  height: 100%;
}

@media screen and (max-width: 600px) {
    .sidebar {
        display: none;
    }
}
</style>
