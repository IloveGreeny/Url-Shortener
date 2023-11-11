<template>
  <div>
    <label for="longUrl">Enter URL:</label>
    <input v-model="longUrl" id="longUrl" type="text" />
    <button @click="shortenUrl">Shorten URL</button>

    <div v-for="(urlData, index) in shortUrls" :key="index">
      <p>Shortened URL {{ index + 1 }}:</p>
      <a :href="urlData.url" target="_blank">{{ urlData.url }}</a>
      <img :src="urlData.qrCode" alt="QR Code" />
    </div>
    <h3>Link History</h3>
    <p>{{shortUrls}}</p>
  </div>
</template>



<script>
import axios from "axios";
import QRCode from "qrcode-generator";

export default {
  data() {
    return {
      longUrl: "",
      shortUrls: JSON.parse(localStorage.getItem("shortUrls")) || [],
    };
  },
  methods: {
    shortenUrl() {
      const accessToken = "e29e4ccb8786eb3ea4afa909ad234993657c3b1f";
      const apiUrl = "https://api-ssl.bitly.com/v4/shorten";

      axios
          .post(
              apiUrl,
              {
                long_url: this.longUrl,
              },
              {
                headers: {
                  Authorization: `Bearer ${accessToken}`,
                  "Content-Type": "application/json",
                },
              }
          )
          .then((response) => {
            const shortUrl = response.data.id;
            const qrCode = this.generateQRCode(shortUrl);

            // Update shortUrls array
            this.shortUrls.push({ url: shortUrl, qrCode });

            // Save to localStorage
            localStorage.setItem("shortUrls", JSON.stringify(this.shortUrls));
          })
          .catch((error) => {
            console.error("Error shortening URL:", error);
          });
    },
    generateQRCode(text) {
      const typeNumber = 4; // Adjust as needed
      const errorCorrectionLevel = "L";
      const qr = QRCode(typeNumber, errorCorrectionLevel);
      qr.addData(text);
      qr.make();
      return qr.createDataURL();
    },
  },
};
</script>



