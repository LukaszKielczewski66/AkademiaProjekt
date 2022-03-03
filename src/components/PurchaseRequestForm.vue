<template>
        <v-container>
            <h1 class="text-center py-5 mt-5">Formularz zgłoszenia zamówienia</h1>
            <v-divider></v-divider>
            <v-form class="px-5 py-5 mt-5" ref="form">
                <h2 class="text-center my-3 mb-5">Informacje o zamówieniu</h2>

                    <v-text-field 
                     v-model="purchaseRequest.orderName"
                     :rules="orderRules"
                     label="Nazwa zlecenia"
                     prepend-icon="fa-solid fa-file-signature"
                     ></v-text-field>
                   
                    <v-file-input
                     v-model="pdfFile"
                     :rules="required"
                     label="Plik pdf ze wzorem"
                     ></v-file-input>
                    
                    <v-text-field
                     v-model="purchaseRequest.quantityInMeters"
                     :rules="required"
                     label="Ilość w metrach"
                     type="number"
                     prepend-icon="fa-solid fa-calculator"
                      ></v-text-field>

                <h2 class="text-center my-3 mb-5 mt-5">Wybierz rodzaj folii</h2>

                    <v-select
                     v-model="purchaseRequest.foilParameters.width"
                     :rules="required"
                     :items="widthItems"
                     label="Szerokość"
                     prepend-icon="fa-solid fa-text-width"
                    ></v-select>

                    <v-select
                     v-model="purchaseRequest.foilParameters.thickness"
                     :rules="required"
                     :items="thicknessItems"
                     label="Grubość"
                     prepend-icon="fa-solid fa-ruler"
                    ></v-select>

                    <v-checkbox
                     v-model="purchaseRequest.foilParameters.antifog"
                     label="Folia z antyfogiem"
                    ></v-checkbox>

                <h2 class="text-center my-3 mb-5 mt-5">Dane zamawiającego</h2>

                    <v-text-field
                     v-model="purchaseRequest.purchaserName"
                     :rules="purchaserNameRules"
                     label="Nazwa firmy/Imię i Nazwisko zamawiającego"
                     prepend-icon="fa-regular fa-user"
                     ></v-text-field>

                    <v-text-field
                     v-model="purchaseRequest.purchaserAddress"
                     :rules="addressRules"
                     label="Adres firmy" 
                     prepend-icon="fa-solid fa-map-pin"
                     ></v-text-field>

                    <v-text-field
                     v-model="purchaseRequest.email"
                     :rules="emailRules"
                     label="E-mail" 
                     prepend-icon="fa-solid fa-at"
                     ></v-text-field>

                    <v-text-field 
                     v-model="purchaseRequest.phone"
                     :rules="phoneRules"
                     label="Numer telefonu"
                     type="number" 
                     prepend-icon="fa-solid fa-phone"
                     ></v-text-field>

                    <v-card-actions>
                    <v-btn
                        @click = "save"
                        depressed
                        color="success"
                        class="my-5"
                        >
                        Złóż zapotrzebowanie
                    </v-btn>
                    </v-card-actions>
            </v-form>
        </v-container>
</template>

<script>
export default {
    mounted() {
      this.context = window.coachViewContext;
      this.initData();
    },
     data: () => ({
        //  ******************** FORM VALIDATION ********************* 
      valid: false,
      orderRules: [
          v => !! v || 'To pole jest wymagane!',
          v => v.length >= 3 || 'Nazwa powinna zawierać co najmniej 3 znaki!',
          v => v.length <= 25 || 'Zbyt długa nazwa',
      ],
      required: [
          v => !!v || 'To pole jest wymagane!',
      ],
      purchaserNameRules: [
          v => !!v || 'To pole jest wymagane!',
          v => v.length >= 3 || 'Nazwa powinna zawierać co najmniej 3 znaki!',
          v => v.length < 40 || 'Zbyt długa nazwa',
      ],
       addressRules: [
          v => !!v || 'To pole jest wymagane!',
          v => v.length >= 5 || 'Nazwa powinna zawierać co najmniej 5 znaków!',
      ],
       phoneRules: [
          v => !!v || 'To pole jest wymagane!',
          v => v.length == 9 || 'Numer powinien posiadać 9 znaków!',
      ],
      emailRules: [ 
        v => /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'Podaj prawidłowy adres e-mail'
      ],
    //   DATA  

      widthItems: [300, 330, 350, 380, 400, 410, 465, 480, 510, 520, 600, 620],
      thicknessItems: [20, 25, 30, 35, 40],

      pdfFile: null,
      purchaseRequest: {
        orderName: '',
        quantityInMeters: 0,
        foilParameters: {
          width: 0,
          thickness: 0,
          antifog: ''
        },
        purchaserName: '',
        purchaserAddress: '',
        email: '',
        phone: 0,
      }
    }),
        methods: {
        initData() {
          this.purchaseRequest = this.context.binding.get("value");
        },
        save() {
          if (this.$refs.form.validate()) {
            this.context.binding.set("value", this.purchaseRequest)
            this.context.trigger();
          }

        }
    }
}
</script>