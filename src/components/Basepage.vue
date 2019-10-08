<template>
  <div>
    <v-navigation-drawer
      v-model="drawer"
      :clipped="$vuetify.breakpoint.lgAndUp"
      app
    >
      <v-list dense>
        <template v-for="item in items">
          <v-layout v-if="item.heading" :key="item.heading" align-center>
            <v-flex xs6>
              <v-subheader v-if="item.heading">{{ item.heading }}</v-subheader>
            </v-flex>
            <v-flex xs6 class="text-center">
              <a href="#!" class="body-2 black--text">EDIT</a>
            </v-flex>
          </v-layout>
          <v-list-group
            v-else-if="item.children"
            :key="item.text"
            v-model="item.model"
            :prepend-icon="item.model ? item.icon : item['icon-alt']"
            append-icon
          >
            <template v-slot:activator>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>{{ item.text }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </template>
            <v-list-item
              v-for="(child, i) in item.children"
              :key="i"
              @click="clickMenu(child.id)"
            >
              <v-list-item-action v-if="child.icon">
                <v-icon>{{ child.icon }}</v-icon>
              </v-list-item-action>
              <v-list-item-content>
                <v-list-item-title>{{ child.text }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-group>
          <v-list-item v-else :key="item.text" @click="clickMenu(item.id)">
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>{{ item.text }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </template>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      :clipped-left="$vuetify.breakpoint.lgAndUp"
      app
      color="blue darken-3"
      dark
    >
      <v-toolbar-title style="width: 300px" class="ml-0 pl-4">
        <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
        <span class="hidden-sm-and-down">电子病历</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-tooltip right>
        <template v-slot:activator="{ on }">
          <v-btn icon to="/about" v-on="on">
            <v-icon>thumb_up_alt</v-icon>
          </v-btn>
        </template>
        <span>帮助手册</span>
      </v-tooltip>
      <v-btn icon to="/">
        <v-icon>notifications</v-icon>
      </v-btn>
      <v-btn icon large @click="selectSource">
        <v-avatar size="32px" item>
          <v-img :src="require('../assets/logo.svg')" alt="苜蓿草科技"></v-img>
        </v-avatar>
      </v-btn>
    </v-app-bar>
    <!-- <v-container fluid fill-height>
      <v-layout justify-start>
        <v-img :src="require('../assets/main_bg.jpg')" />
      </v-layout>
    </v-container>-->
    <v-tooltip right>
      <template v-slot:activator="{ on }">
        <v-btn
          bottom
          color="pink"
          dark
          fab
          fixed
          right
          @click="clickMenu('logout')"
          v-on="on"
          ><v-icon>add</v-icon>
        </v-btn>
      </template>
      <span>退出登录</span>
    </v-tooltip>
  </div>
</template>

<script>
export default {
  props: {
    source: String
  },
  data: () => ({
    dialog: false,
    drawer: null,
    items: [
      { 
        icon: "contacts", 
        "icon-alt": "keyboard_arrow_down",
        text: "入院资料", 
        model: false,
        children: [
          { text: "患者列表", id: "in01" },
          { text: "住院信息", id: "in02" },
          { text: "病案首页", id: "in03" },
          { text: "入院记录", id: "in04" },
          { text: "表格病历", id: "in05" },
          { text: "通用记录单", id: "in06" }
        ]
       },
      { 
        icon: "history", 
        "icon-alt": "keyboard_arrow_down",
        text: "住院资料", 
        model: false,
        children: [
          { text: "医嘱", id: "cur01" },
          { text: "病程记录", id: "cur02" },
          { text: "检验",id: "cur03" },
          { text: "检查", id: "cur04" },
          { text: "体温单", id: "cur05" },
          { text: "手术资料", id: "cur06" },
          { text: "谈话记录", id: "cur07" },
          { text: "病情告知书", id: "cur08" },
          { text: "讨论记录", id: "cur09" },
          { text: "报告表", id: "cur10" },
          { text: "跨科处置", id: "cur11" },
          { text: "会诊", id: "cur12" },
          { text: "院感申请", id: "cur13" }
        ]
      },
      { 
        icon: "content_copy", 
        "icon-alt": "keyboard_arrow_down",
        text: "出院资料", 
        model: false,
        children: [
          { text: "出院小结", id: "out01" },
          { text: "死亡资料", id: "out02" },
          { text: "24小时入院死亡记录",id: "out03" },
          { text: "24小时入出院记录", id: "out04" },
          { text: "打印病历", id: "out05" },
          { text: "归档提交", id: "out06" },
          { text: "反归档申请", id: "out07" }          
        ]
      },
      { 
        icon: "content_copy", 
        "icon-alt": "keyboard_arrow_down",
        text: "模板管理", 
        model: false,
        children: [
          { text: "协定医嘱管理",id: "m01" },
          { text: "病程模板管理", id: "m02" }   
        ]
      },
      { 
        icon: "keyboard", 
        "icon-alt": "keyboard_arrow_down",
        text: "收费信息", 
        model: false,
        children: [
          { text: "费用明细",id: "f01" },
          { text: "费用日清单", id: "f02" },
          { text: "诊疗项目词典", id: "f03" },
          { text: "药品动态库存", id: "f04" }
        ]
      },
      { icon: "content_copy", text: "传染病报告", id: "out_chk" },
      { icon: "keyboard", text: "慢性病报告", id: "out_receipt" },
      { icon: "content_copy", text: "历史病历", id: "out_chk1" },
      
      { icon: "help", text: "退出登录", id: "logout" }
    ]
  }),
  methods: {
    clickMenu(tstr) {
      console.log("点击=" + tstr);
      if (tstr === "logout") {
        localStorage.removeItem("user");
        this.$router.push({ path: "/login" });
      }
      switch (tstr) {
        case "out_reg":
          this.$router.push({ path: "/outreg" });
          break;
        case "out_cash":
          this.$router.push({ path: "/outcash" });
          break;
        case "out_chk":
          this.$router.push({ path: "/outchk" });
          break;
        case "out_receipt":
          this.$router.push({ path: "/outreceipt" });
          break;
        case "detail_reg":
          this.$router.push({ path: "/detailreg" });
          break;
        case "detail_cash":
          this.$router.push({ path: "/detailcash" });
          break;
        case "detail_chk":
          this.$router.push({ path: "/detailchk" });
          break;
        case "detail_undo":
          this.$router.push({ path: "/detailundo" });
          break;
        case "detail_op":
          this.$router.push({ path: "/detailop" });
          break;
        case "mg_dict":
          this.$router.push({ path: "/mgdict" });
          break;
        case "mg_analyse":
          this.$router.push({ path: "/mganalyse" });
          break;
        case "mg_invoice":
          this.$router.push({ path: "/mginvoice" });
          break;
        default:
          localStorage.removeItem("user");
          this.$router.push({ path: "/login" });
      }
    },

    selectSource() {
      window.location.href = "http://www.cloveropen.com";
    }
  }
};
</script>
