<template>
  <div class="ion-page">
    <ion-header>
      <ion-toolbar>
        <ion-title>Ionic POC - Find US Cities</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content class="ion-padding">
      <zip-search @get-zip="getZipInfo" />
      <zip-info :info="info" />
      <zip-clear :info="info" @clear-info="clearInfo" />
    </ion-content>
  </div>
</template>

<script>
import ZipSearch from "@/components/ZipSearch";
import ZipInfo from "@/components/ZipInfo";
import ZipClear from "@/components/ZipClear";

export default {
  name: 'Home',
  components: {
    ZipSearch,
    ZipInfo,
    ZipClear,
  },
  data() {
    return {
      info: null,
    };
  },
  methods: {
    async getZipInfo(zip) {
      const api = `https://api.zippopotam.us/us/${zip}`;
      const res = await fetch(api);
      if (res.status === 404) {
        this.clearInfo();
        this.showAlert();
        return false;
      }
      this.info = await res.json();
    },
    clearInfo() {
      this.info = null;
    },
    showAlert() {
      return this.$ionic.alertController
        .create({
          header: 'Zip Code does not exist',
          message: 'Please enter a valid US zipcode',
          buttons: [
            {
              text: 'Okay',
              handler: () => {
                // eslint-disable-next-line no-console
                console.log('Confirm Okay');

                // TODO: dismiss with return key
              },
              role: 'cancel',
            },
          ],
          backdropDismiss: false,
        })
        .then(a => a.present());
    },
  }
};
</script>
