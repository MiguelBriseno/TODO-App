<template>
  <v-app>
    <v-card height="100%">
      <v-layout height="100%">
        <v-navigation-drawer expand-on-hover rail>
          <v-list>
            <v-list-item prepend-avatar="https://i.postimg.cc/Wb4sNjq8/miguel-removebg-preview.png" subtitle="miguel.briseno.bustos@gmail.com" title="Miguel Briseño"></v-list-item>
          </v-list>
          <v-divider></v-divider>
          <v-list density="compact" nav>
            <v-list-item prepend-icon="mdi-file-tree" title="Tareas" value="tasks"></v-list-item>
          </v-list>
        </v-navigation-drawer>
        <v-main>
          <HelloWorld />
        </v-main>
        <AppFooter />
      </v-layout>
    </v-card>
  </v-app>
</template>

<style>
html, body, #app {
  height: 100%;
  margin: 0;
}

v-app, v-card, v-layout, v-main {
  height: 100%;
}
</style>
