<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
      <input type="text" id="name" v-model="name" placeholder="Enter name here">
      <button class="btn btn-primary pl-5 pr-5" @click="download">Download PDF</button>
 </div>
</template>

<script>
import jsPDF from 'jspdf'
export default {
	data() {
		return {
      name: '',
      date: "",
      nbRandom: "0",
      siret: "12345678912345",
      pharmacy: {
        name: "Pharmacie du port haut",
        address: "80 Rue du port haut",
        zipcode: "31000",
        city: "Toulouse",
        siret: "56789123456789",
      },
      amountOrderTTC: 20,
      commissionHT: 0,
      tva20: 0,
      commissionTTC: 0,
      com:5,

		}
  },
  mounted() {
    this.date = new Date().toLocaleDateString();
    this.nbRandom = Math.floor(Math.random() * 1000000);
    this.calculateCommission();
    this.calculateTva();
    this.calculateCommissionTTC();
  },
  methods: {
    calculateCommission() {
      this.commissionHT = (this.amountOrderTTC * (this.com/100)).toFixed(2);
    },
    calculateTva() {
      this.tva20 = (this.commissionHT * 0.2).toFixed(2);
    },
    calculateCommissionTTC() {
      this.commissionTTC = ((this.amountOrderTTC * (this.com/100)) + (this.commissionHT * 0.2)).toFixed(2);
    },
    
    download() { 
      var doc = new jsPDF();
      doc.setFontSize(11);

      // doc.addImage('./assets/logo.png', 'png', 10, 10, 50, 50);
      //header
      doc.text("Facture en euros n° : " + this.nbRandom, 10, 40);
      doc.text("Date : " + this.date, 10, 46);
      //Pharmacy info
      doc.text(this.pharmacy.name, 20, 65);
      doc.text(this.pharmacy.address, 20, 71);
      doc.text(this.pharmacy.zipcode, 20, 77);
      doc.text(this.pharmacy.city, 33, 77);
      doc.text("Siret : " + this.pharmacy.siret, 20, 83);
      //Array of order
      //Amount
      doc.text("Montant total : " + this.amountOrderTTC, 130, 195);
      doc.text("Commission " +this.com+"% HT : " + this.commissionHT, 130, 200);
      doc.text("TVA 20% : " + this.tva20, 130, 205);
      doc.text("Commission TTC : " + this.commissionTTC, 130, 210);
      //Footer
      doc.text("BLUE ALPHABET", 100, 268, "center");
      doc.text("Société par actions simplifiés", 100, 274, "center");
      doc.text("au capital de 1000€ (mille euros)", 100, 280, "center");
      doc.text("Siège social: 13 RUE SAINT-URSULE 31000 Toulouse", 100, 286, "center");
      doc.text("RCS de Toulouse sous le numéro B 882 622 871", 100, 292, "center");
      //CGV
      doc.setFontSize(9);
      doc.text("En votre aimable règlement", 20, 226);
      doc.text("Cordialement,", 20, 230);
      doc.text("Conditions de règlement : paiement à reception de facture", 20, 234);
      doc.text("Aucun escompte consenti pour règlement anticipé", 20, 238);
      doc.text("Tout incident de paiement est passible d'intérêt de retard. Le montant des pénalités résulte de", 20, 242);
      doc.text("l'application aux sommes restant dues d'un taux d'intérêt légal en vigeur au moment de l'incident.", 20, 246);
      doc.text("Indémnité forfaitaire pour frais de recouvrement due au créancier en cas de retard de paiement : 40€", 20, 250);




      window.open(URL.createObjectURL(doc.output("blob")))
      // doc.save(`Facture/${this.nbRandom}/${this.date}/${this.siret}.pdf`);
    }

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
