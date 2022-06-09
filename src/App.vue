<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
      <input type="text" id="name" v-model="name" placeholder="Enter name here">
      <button class="btn btn-primary pl-5 pr-5" @click="download">Download PDF</button>
 </div>
</template>

<script>
import jsPDF from 'jspdf';
import autoTable from 'jspdf-autotable';

export default {

	data() {
		return {
      name: '',
      img: "data:image/png;base64,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",
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
   
      amountOrderTTC: 0,
      commissionHT: 0,
      tva20: 0,
      commissionTTC: 0,
      com:5,
      data: [
       {
          orderNumber: "Commande 1",
          dateOrder: "01/01/2020",
          totalAmount: 10,
        },
        {
          orderNumber: "Commande 2",
          dateOrder: "09/01/2025",
          totalAmount: 100,
        },
        {
          orderNumber: "Commande 3",
          dateOrder: "01/01/2020",
          totalAmount: 80,
        },
    ]
		}
  },
  mounted() {
    this.date = new Date().toLocaleDateString();
    this.nbRandom = Math.floor(Math.random() * 1000000);
    this.calculateTotalAmount();
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
    calculateTotalAmount() {
      this.data.forEach(element => {
        this.amountOrderTTC += element.totalAmount;
      });
    },
    
    download() { 
      var doc = new jsPDF();

      doc.addImage(this.img, 'PNG', 10, 5, 80, 30);
      doc.setFontSize(11);
      //header
      doc.text("Facture en euros n° : " + this.nbRandom, 10, 45);
      doc.text("Date : " + this.date, 10, 51);
      //Pharmacy info
      doc.text(this.pharmacy.name, 20, 65);
      doc.text(this.pharmacy.address, 20, 71);
      doc.text(this.pharmacy.zipcode, 20, 77);
      doc.text(this.pharmacy.city, 33, 77);
      doc.text("Siret : " + this.pharmacy.siret, 20, 83);
      //Array of order
  

      autoTable(doc, ({
        startY: 100,
        theme: 'grid',
        head: [['Date', 'Numéro de commande', 'Prix total TTC']],
        body: this.data.map((el) => [el.dateOrder, el.orderNumber, el.totalAmount]),
        pageBreak: 'auto',
        headStyles: {
          fillColor: [181, 229, 220],
          textColor: 0,
          fontStyle: 'bold',
          valign: 'middle',
          fontSize: 11
        },
        
      //nb pages
      didDrawPage: function (data){
        const str = "Page " + doc.internal.getNumberOfPages();
        const pageSize = doc.internal.pageSize;
        const pageHeight = pageSize.height
          ? pageSize.height
          : pageSize.getHeight();
        doc.text(str , data.settings.margin.left, pageHeight - 10);
        
      }
      }));
      
      // console.log(JSON.stringify(doc.autoTable.previous.finalY));
      const previousY = doc.autoTable.previous.finalY;
      if (previousY > 199) {
        doc.addPage();
      //Amount
      doc.text("Montant total : " + this.amountOrderTTC, 130,  20);
      doc.text("Commission " +this.com+"% HT : " + this.commissionHT, 130, 25);
      doc.text("TVA 20% : " + this.tva20, 130,  30);
        doc.text("Commission TTC : " + this.commissionTTC, 130, 35);

      doc.setFontSize(9);
      doc.text("En votre aimable règlement", 20,  46);
      doc.text("Cordialement,", 20,  50);
      doc.text("Conditions de règlement : paiement à réception de facture", 20, 54);
      doc.text("Aucun escompte consenti pour règlement anticipé", 20,  58);
      doc.text("Tout incident de paiement est passible d'intérêt de retard. Le montant des pénalités résulte de", 20, 62);
      doc.text("l'application aux sommes restant dues d'un taux d'intérêt légal en vigueur au moment de l'incident.", 20, 66);
      doc.text("Indemnité forfaitaire pour frais de recouvrement due au créancier en cas de retard de paiement : 40€", 20, 70);
        
      } else {
        doc.text("Montant total : " + this.amountOrderTTC, 130, doc.lastAutoTable.finalY + 20);
        doc.text("Commission " +this.com+"% HT : " + this.commissionHT, 130, doc.lastAutoTable.finalY + 25);
        doc.text("TVA 20% : " + this.tva20, 130, doc.lastAutoTable.finalY + 30);
        doc.text("Commission TTC : " + this.commissionTTC, 130, doc.lastAutoTable.finalY + 35);

         //CGV
      doc.setFontSize(9);
      doc.text("En votre aimable règlement", 20, doc.lastAutoTable.finalY + 46);
      doc.text("Cordialement,", 20, doc.lastAutoTable.finalY + 50);
      doc.text("Conditions de règlement : paiement à reception de facture", 20, doc.lastAutoTable.finalY + 54);
      doc.text("Aucun escompte consenti pour règlement anticipé", 20, doc.lastAutoTable.finalY + 58);
      doc.text("Tout incident de paiement est passible d'intérêt de retard. Le montant des pénalités résulte de", 20, doc.lastAutoTable.finalY + 62);
      doc.text("l'application aux sommes restant dues d'un taux d'intérêt légal en vigeur au moment de l'incident.", 20, doc.lastAutoTable.finalY + 66);
      doc.text("Indémnité forfaitaire pour frais de recouvrement due au créancier en cas de retard de paiement : 40€", 20, doc.lastAutoTable.finalY + 70);

      }
      //Footer
      doc.text("BLUE ALPHABET", 100, doc.internal.pageSize.height - 21, "center");
      doc.text("Société par actions simplifiés", 100, doc.internal.pageSize.height - 17, "center");
      doc.text("au capital de 1000€ (mille euros)", 100, doc.internal.pageSize.height - 13, "center");
      doc.text("Siège social: 13 RUE SAINT-URSULE 31000 Toulouse", 100, doc.internal.pageSize.height - 9, "center");
      doc.text("RCS de Toulouse sous le numéro B 882 622 871", 100, doc.internal.pageSize.height - 5, "center");
     

      window.open(URL.createObjectURL(doc.output("blob"))) //for dev
      // doc.save(`Facture/${this.nbRandom}/${this.date}/${this.siret}.pdf`);
    }

  }
}
</script>
