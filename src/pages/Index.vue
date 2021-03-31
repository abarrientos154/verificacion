<template lang="pug">
div
  q-item(clickable='' v-ripple='', @click='myModal.active_two = !myModal.active_two')
    q-item-section(side='')
      q-avatar(rounded='' size='24px')
        img(src='favicon.ico')
    q-item-section(class='bg-primary text-color-white')
      q-item-label Verify Me
  //
    q-btn(outline, color='primary', label='Verify Me', @click='myModal.active = !myModal.active')
    q-btn(outline, color='primary', label='Verificame', @click='myModal.active_two = !myModal.active_two')
  q-dialog(v-model='myModal.active', style='width: 80 vw;')
    q-card
      q-toolbar
        q-avatar
          img(src='https://cdn.quasar.dev/logo/svg/quasar-logo.svg')
        q-toolbar-title
          span(text-weight-bold, class='center-text') Tipo de Documento
        q-btn(flat='' round='' dense='' icon='close' v-close-popup='')
      q-card-section(class='col', style='left: 50%; transform: translate(0, 50);')
        div
          q-btn(padding="md")
            q-avatar(square, size='128px')
              img(src='icon_dni.png')
          //
            p(class='text-center') Custom
        div
          q-btn(padding="md")
            q-avatar(square, size='128px')
              img(src='icon_passport.png')
        div
          q-btn(padding="md")
            q-avatar(square, size='128px')
              img(src='icon_d_license.png')
  q-dialog(v-model='myModal.active_two', full-width, style="width: 80vh; max-width: 80vw;")
    q-card
      q-toolbar
        q-avatar
          img(src='big-logo.png')
        q-toolbar-title
          span(text-weight-bold, class='center-text') Recoleccion de Informacion
        q-btn(flat='' round='' dense='' icon='close' v-close-popup='myModal.active_two')
      q-card-section(class='col')
        div(class='q-pa-md')
          q-stepper(v-model='myModal.step' color='primary' animated='')
            q-step(:name='1' title='Tipo de Documento' icon='settings' :done='myModal.step > 1')
              div(class='col-md-6 offset-md-3')
                div
                  q-btn(padding="md" @click="myModal.file_type = 'national-id', myModal.step = 2")
                    q-avatar(square, size='128px')
                      img(src='icon_dni.png')
                div
                  q-btn(padding="md" @click="myModal.file_type = 'driving-license', myModal.step = 2")
                    q-avatar(square, size='128px')
                      img(src='icon_passport.png')
                div
                  q-btn(padding="md")
                    q-avatar(square, size='128px' @click="myModal.file_type = 'passport', myModal.step = 2")
                      img(src='icon_d_license.png')
            q-step(:name='2' title='Documento Frontal' caption='Optional' icon='create_new_folder' :done='myModal.step > 2')
              div(class='col')
                img(src='front-document.png')
                q-uploader(style='max-width: 300px' url='http://localhost:4444/upload' label='Buscar Archivo' multiple='' :filter='checkFileSize' @rejected='onRejected')
              q-stepper-navigation
                q-btn(flat='' @click='myModal.step = 1' color='primary' label='Atras', class='q-ml-sm')
                q-btn(@click='myModal.step = 3' color='primary' label='Continue')
                  q-btn.q-ml-sm(flat='' @click='myModal.step = 1' color='primary' label='Atras')
            q-step(:name='3' title='ocumento Reverso' icon='assignment' disable='')
              div(class='col')
                img(src='front-document.png')
                q-uploader(style='max-width: 300px' url='http://localhost:4444/upload' label='Buscar Archivo' multiple='' :filter='checkFileSize' @rejected='onRejected')
              q-stepper-navigation
                q-btn(color='primary' label='Enviar')
                  q-btn.q-ml-sm(flat='' @click='myModal.step = 2' color='primary' label='Atras')
</template>

<script lang="ts">
import { rsModal } from "components/models";
import { defineComponent, ref } from "vue";
import { useQuasar } from "quasar";

export default defineComponent({
  name: "PageIndex",
  setup() {
    const myModal = ref<rsModal>({
      active: false,
      active_two: false,
      step: 1,
      contract_id: 0,
      user_id: 0,
      document1: [],
      document2: [],
      file_type: "",
    });
    const $q = useQuasar();

    const checkFileSize = (files: any[]) => {
      return files.filter((file) => file.size < 15360);
    };

    function checkFileType(files: any[]) {
      return files.filter(
        (file) =>
          file.type === "image/png" ||
          file.type === "image/jpeg" ||
          file.type === "image/jpg"
      );
    }

    function onRejected(rejectedEntries: any[]) {
      $q.notify({
        type: "negative",
        message: `${rejectedEntries.length} file(s) did not pass validation constraints`,
      });
    }
    return { myModal, checkFileSize, checkFileType, onRejected };
  },
});
</script>
