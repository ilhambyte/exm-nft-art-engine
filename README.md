# NFT Art Engine 🔥

Buat seni generatif dengan menggunakan api kanvas dan node js. Sebelum Anda menggunakan mesin generasi, pastikan Anda telah menginstal node.js(v10.18.0).

## Instalasi 🛠️

Jika Anda ingin mengkloning proyek, jalankan ini terlebih dahulu, jika tidak, Anda dapat mengunduh kode sumber di halaman rilis dan lewati langkah ini.

```sh
git clone https://github.com/ilhambyte/exm-nft-art-engine.git
```

Buka root folder Anda dan jalankan perintah ini jika Anda telah menginstal benang.

```sh
yarn install
```

Atau Anda dapat menjalankan perintah ini jika Anda telah menginstal node.

```sh
npm install
```

## Penggunaan ℹ️

Buatlah layer Anda yang berbeda sebagai folder di 'layers' direktori, dan tambahkan semua aset layer di direktori ini. Anda dapat memberi nama aset apa saja asalkan memiliki bobot kelangkaan yang dilampirkan dalam nama file seperti ini: `example element#70.png`. Anda dapat mengubah pembatas secara opsional `#` untuk apa pun yang ingin Anda gunakan dalam variabel `rarityDelimiter` didalam file `src/config.js` .

Setelah Anda memiliki semua layer Anda, lalu masuk ke `src/config.js` dan update pada `layerConfigurations` objects `layersOrder` array menjadi nama folder layer Anda dengan urutan layer belakang ke layer depan.

_Contoh:_ Jika Anda membuat desain potret, Anda mungkin harus memiliki latar belakang, lalu kepala, mulut, mata, kacamata, dan kemudian penutup kepala, jadi pada bagian `layersOrder` akan terlihat seperti ini:

```js
const layerConfigurations = [
  {
    growEditionSizeTo: 100,
    layersOrder: [
      { name: "Head" },
      { name: "Mouth" },
      { name: "Eyes" },
      { name: "Eyeswear" },
      { name: "Headwear" },
    ],
  },
];
```

Bagian `name` dari setiap objek layer mewakili nama folder di ( `/layers/`) tempat gambar berada.

Secara opsional, Anda sekarang dapat menambahkan beberapa yang perbedaan `layerConfigurations` untuk koleksi Anda. Setiap konfigurasi dapat unik dan memiliki urutan layer yang berbeda, menggunakan layer yang sama atau menambahkan yang baru. Hal ini memberikan fleksibilitas kepada seniman/artist dalam hal menyesuaikan koleksi mereka dengan kebutuhan mereka.

_Contoh:_ Jika Anda membuat desain potret, Anda mungkin memiliki latar belakang, lalu kepala, mulut, mata, kacamata, dan kemudian penutup kepala dan Anda ingin membuat ras baru atau sekadar menyusun ulang layer atau bahkan menambahkan layer baru, maka kamu masuk ke `layerConfigurations` dan `layersOrder` akan terlihat seperti ini:

```js
const layerConfigurations = [
  {
    // Creates up to 50 artworks
    growEditionSizeTo: 50,
    layersOrder: [
      { name: "Background" },
      { name: "Head" },
      { name: "Mouth" },
      { name: "Eyes" },
      { name: "Eyeswear" },
      { name: "Headwear" },
    ],
  },
  {
    // Creates an additional 100 artworks
    growEditionSizeTo: 150,
    layersOrder: [
      { name: "Background" },
      { name: "Head" },
      { name: "Eyes" },
      { name: "Mouth" },
      { name: "Eyeswear" },
      { name: "Headwear" },
      { name: "AlienHeadwear" },
    ],
  },
];
```

Perbarui ukuran `format` Anda, yaitu ukuran gambar yang dihasilkan, dan `growEditionSizeTo` pada setiap objek `layerConfigurations`, yang merupakan jumlah variasi yang dihasilkan.

Anda dapat mencampuradukkan urutan `layerConfigurations` tentang bagaimana gambar disimpan dengan menyetel variabel `shuffleLayerConfigurations` dalam file `config.js` ke true. Secara default ini false dan akan menyimpan semua gambar dalam urutan numerik.

Jika Anda ingin memiliki log untuk di-debug dan melihat apa yang terjadi saat Anda menghasilkan gambar, Anda dapat menyetel variabel `debugLogs` dalam file `config.js` ke true. Secara default ini false, jadi Anda hanya akan melihat log umum.

Jika Anda ingin bermain-main dengan mode pencampuran yang berbeda, Anda dapat menambahkan `blend: MODE.colorBurn` ke objek `options` layersOrder.

Jika Anda membutuhkan layer untuk memiliki opacity yang berbeda maka Anda dapat menambahkan bidang `opacity: 0.7` ke objek `options` layersOrder juga.

Jika Anda ingin layer diabaikan dalam pemeriksaan keunikan DNA, Anda dapat mengatur `bypassDNA: true` di objek `options` . Ini memiliki efek untuk memastikan ciri-ciri lainnya unik sementara tidak mempertimbangkan Layer `Background` sebagai ciri, misalnya. Layer termasuk dalam gambar akhir.

Untuk menggunakan nama atribut metadata yang berbeda, Anda dapat menambahkan `displayName: "Awesome Eye Color"` ke objek `options`. Semua opsi bersifat opsional dan dapat ditambahkan pada lapisan yang sama jika Anda mau.

Berikut adalah contoh bagaimana Anda dapat bermain-main dengan kedua bidang filter:

```js
const layerConfigurations = [
  {
    growEditionSizeTo: 5,
    layersOrder: [
      { name: "Background" , {
        options: {
          bypassDNA: false;
        }
      }},
      { name: "Eyeball" },
      {
        name: "Eye color",
        options: {
          blend: MODE.destinationIn,
          opacity: 0.2,
          displayName: "Awesome Eye Color",
        },
      },
      { name: "Iris" },
      { name: "Shine" },
      { name: "Bottom lid", options: { blend: MODE.overlay, opacity: 0.7 } },
      { name: "Top lid" },
    ],
  },
];
```

Berikut adalah daftar berbagai mode pencampuran yang dapat Anda gunakan secara opsional.

```js
const MODE = {
  sourceOver: "source-over",
  sourceIn: "source-in",
  sourceOut: "source-out",
  sourceAtop: "source-out",
  destinationOver: "destination-over",
  destinationIn: "destination-in",
  destinationOut: "destination-out",
  destinationAtop: "destination-atop",
  lighter: "lighter",
  copy: "copy",
  xor: "xor",
  multiply: "multiply",
  screen: "screen",
  overlay: "overlay",
  darken: "darken",
  lighten: "lighten",
  colorDodge: "color-dodge",
  colorBurn: "color-burn",
  hardLight: "hard-light",
  softLight: "soft-light",
  difference: "difference",
  exclusion: "exclusion",
  hue: "hue",
  saturation: "saturation",
  color: "color",
  luminosity: "luminosity",
};
```

Saat Anda siap, jalankan perintah berikut dan hasil seni Anda akan berada di direktori `build/images` dan json di direktori `build/json`:

```sh
npm run build
```

atau

```sh
node index.js
```

Program akan menampilkan semua gambar di direktori `build/images` bersama dengan file metadata di direktori `build/json` . Setiap koleksi akan memiliki file `_metadata.json` yang terdiri dari semua metadata dalam koleksi di dalam direktori `build/json` . Folder `build/json` juga akan berisi semua file json tunggal yang mewakili setiap file gambar. File json tunggal dari sebuah gambar akan terlihat seperti ini:

```json
{
  "dna": "d956cdf4e460508b5ff90c21974124f68d6edc34",
  "name": "#1",
  "description": "This is the description of your NFT project",
  "image": "https://hashlips/nft/1.png",
  "edition": 1,
  "date": 1731990799975,
  "attributes": [
    { "trait_type": "Background", "value": "Black" },
    { "trait_type": "Eyeball", "value": "Red" },
    { "trait_type": "Eye color", "value": "Yellow" },
    { "trait_type": "Iris", "value": "Small" },
    { "trait_type": "Shine", "value": "Shapes" },
    { "trait_type": "Bottom lid", "value": "Low" },
    { "trait_type": "Top lid", "value": "Middle" }
  ],
  "compiler": "HashLips Art Engine"
}
```

Anda juga dapat menambahkan `extraMetadata` ke setiap file metadata dengan menambahkan item tambahan, (key: value) berpasangan ke variabel objek `extraMetadata` di file `config.js` .

```js
const extraMetadata = {
  creator: "Daniel Eugene Botha",
};
```

Jika Anda tidak membutuhkan metadata tambahan, biarkan objek kosong. Secara default itu kosong.

```js
const extraMetadata = {};
```

Itu saja, Anda sudah selesai.

## Utilitas

### Memperbarui baseUri untuk IPFS dan deskripsi

Anda mungkin ingin memperbarui baseUri dan deskripsi setelah Anda menjalankan koleksi Anda. Untuk memperbarui baseUri dan deskripsi cukup jalankan:

```sh
npm run update_info
```

### Hasilkan gambar pratinjau

Buat kolase gambar pratinjau dari koleksi Anda, jalankan:

```sh
npm run preview
```

### Hasilkan gambar pixel dari koleksi

Untuk mengonversi gambar menjadi gambar berpiksel, Anda memerlukan daftar gambar yang ingin Anda konversi. Jadi jalankan generatornya terlebih dahulu.

Kemudian cukup jalankan perintah ini:

```sh
npm run pixelate
```

Semua gambar Anda akan tampilkan di direktori `/build/pixel_images` . Jika Anda ingin mengubah rasio pikselasi, Anda dapat memperbarui properti rasio pada objek `pixelFormat` di file `src/config.js` . Semakin rendah angka di sebelah kiri, semakin banyak piksel pada gambarnya.

```js
const pixelFormat = {
  ratio: 5 / 128,
};
```

### Hasilkan gambar GIF dari koleksi

Untuk mengekspor gif berdasarkan lapisan yang dibuat, Anda hanya perlu mengatur ekspor pada objek `gif` di file `src/config.js` ke `true` . Anda juga dapat bermain-main dengan `repeat`, `quality` dan `delay` gif yang diekspor.

Mengatur `repeat: -1` akan menghasilkan render satu kali dan `repeat: 0` akan berulang-ulang selamanya.
```js
const gif = {
  export: true,
  repeat: 0,
  quality: 100,
  delay: 500,
};
```

### Mencetak data kelangkaan (Fitur eksperimental)

Untuk melihat persentase setiap atribut di seluruh koleksi Anda, jalankan:

```sh
npm run rarity
```

Outputnya akan terlihat seperti ini:

```sh
Trait type: Top lid
{
  trait: 'High',
  chance: '30',
  occurrence: '3 in 20 editions (15.00 %)'
}
{
  trait: 'Low',
  chance: '20',
  occurrence: '3 in 20 editions (15.00 %)'
}
{
  trait: 'Middle',
  chance: '50',
  occurrence: '14 in 20 editions (70.00 %)'
}
```

Terima kasih, Semoga Anda membuat beberapa karya seni yang luar biasa dengan kode ini 👄

Referensi: 
- HashLips