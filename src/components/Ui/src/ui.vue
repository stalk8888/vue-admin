<template>
  <v-app>
    <v-navigation-drawer app clipped v-model="drawer" fixed>
      <v-list dense>
        <template v-for="(item, i) in menuItems">
          <v-layout row
            v-if="item.heading"
            align-center
            :key="item.heading"
          >
            <v-flex xs6>
              <v-subheader v-if="item.heading">
                {{ item.heading }}
              </v-subheader>
            </v-flex>
            <v-flex xs6 class="text-xs-center">
              <a href="#!" class="body-2 black--text">EDIT</a>
            </v-flex>
          </v-layout>

          <v-list-group
            :key="i"
            v-else-if="item.children"
            v-model="item.model"
            no-action
            :prepend-icon="item.model ? item.icon : item['icon-alt']"
            append-icon=""
          >
            <v-list-tile slot="activator">
              <v-list-tile-content>
                <v-list-tile-title>
                  {{ item.title }}
                </v-list-tile-title>
              </v-list-tile-content>
            </v-list-tile>
            <v-list-tile
              v-for="(child, i) in item.children"
              :key="i"
              :to="child.link"
            >
              <v-list-tile-action v-if="child.icon">
                <v-icon>{{ child.icon }}</v-icon>
              </v-list-tile-action>
              <v-list-tile-content>
                <v-list-tile-title>
                  {{ child.title }}
                </v-list-tile-title>
              </v-list-tile-content>
            </v-list-tile>
          </v-list-group>
          <v-list-tile v-else :to="item.link" :key="item.title">
            <v-list-tile-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-tile-action>
            <v-list-tile-content>
              <v-list-tile-title>
                {{ item.title }}
              </v-list-tile-title>
            </v-list-tile-content>
          </v-list-tile>
        </template>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar class="success" dark app fixed clipped-left dense>
      <v-toolbar-title>
        <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
        Admin XXX
      </v-toolbar-title>
      <v-spacer></v-spacer>
        <v-toolbar-items>
          <v-btn flat>
            <v-avatar size="36px">Juan</v-avatar>
          </v-btn>
          <v-btn flat @click="onLogout"><v-icon left dark>exit_to_app</v-icon></v-btn>
        </v-toolbar-items>
        <div>
          <v-btn icon>
            <v-icon>apps</v-icon>
          </v-btn>
          <v-btn icon>
            <v-icon>notifications</v-icon>
          </v-btn>
        </div>
        <v-menu :nudge-width="100">
          <v-toolbar-title slot="activator">
            <span>{{selectedLocale}}</span>
            <v-icon dark>arrow_drop_down</v-icon>
          </v-toolbar-title>
          <v-list>
            <v-list-tile v-for="locale in locales" :key="locale" @click="setLocale(locale)">
              <v-list-tile-title v-text="locale"></v-list-tile-title>
            </v-list-tile>
          </v-list>
        </v-menu>
    </v-toolbar>
    <main>
      <v-content>
        <v-container fluid>
          <slot></slot>
        </v-container>
      </v-content>
    </main>
  </v-app>
</template>

<script>
export default {
  name: "Ui",
  props: {
    msg: String
  },
  data() {
    return {
      selectedLocale: "EN",
      locales: ["EN", "ID"],
      drawer: false,
      menuItems: [
        { icon: "settings", title: "Example Form", link: "/example-form" },
        {
          icon: "keyboard_arrow_up",
          "icon-alt": "keyboard_arrow_down",
          title: "Crud",
          model: true,
          children: []
        }
      ]
    };
  },
  mounted() {
    // Listen to addRoutes mutations
    let whitelist = ["resources/addRoute"];
    this.$store.subscribe(mutation => {
      if (whitelist.includes(mutation.type)) {
        this.menuItems[1].children.push({
          icon: "home",
          title: mutation.payload.name,
          link: mutation.payload.path
        });
      }
    });
  },
  computed: {},
  methods: {
    onLogout() {
      // eslint-disable-next-line
      console.log("logout");
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>