<template>
  <q-layout view="hHh Lpr fFf">
    <q-header elevated>
      <q-toolbar>
        <q-toolbar-title>
          Quasar App
        </q-toolbar-title>
      </q-toolbar>
    </q-header>
    <q-page-container>
      <div class="row">
        <div class="col-2"></div>
        <div class="col-8">
          <router-view />
        </div>
        <div class="col-2"></div>
      </div>
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: "MainLayout",
  data() {
    return {};
  }
};
</script>
