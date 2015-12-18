---
layout: post
title:  "How to Handle Two or more Ionic Modals in One Controller"
date:   2015-10-03 21:03:15
author: "meisyal"
categories: ionic
---

**Disclaimer**: *Tutorial ini ditujukan kepada pembaca yang sedang atau telah
menggunakan Ionic Framework untuk mengembangkan aplikasi perangkat bergerak dengan
asumsi memahami kerja dari Ionic Modal.*

Sekilas tentang *Ionic Framework*, [Ionic][ionic] adalah sebuah *framework* yang
digunakan untuk membangun aplikasi perangkat bergerak dengan menggunakan teknologi
*web* (HTML5 dan [AngularJS][angularjs]). Dari definisi tersebut, terdapat istilah
aplikasi *native* dan *hybrid*. Aplikasi *native* berarti aplikasi yang dibangun
sesuai *platform* asli dari perangkat bergerak tersebut (spesifik), misal
*Android* menggunakan Java dan *iOS* menggunakan Objective-C/Swift. Sedangkan,
aplikasi *hybrid* merupakan kebalikan dari aplikasi *native*, dibangun
di luar *platform* perangkat bersangkutan, namun bisa dijalankan di *platform*
asli.

*Ionic* dibuat oleh [Drifty][drifty] dan didukung oleh komunitas karena merupakan
*open source framework*. *Repository* utama dari *Ionic* bisa dilihat di
[sini][ionicrepo]. *Ionic* sendiri mengikuti pola *view-controller* di mana
fungsionalitas utamanya didukung oleh AngularJS[^1].

Sampai di sini pengenalan singkat *Ionic*, telusuri lebih lanjut melalui laman
utamanya atau dokumentasi terkait untuk detail!

Pembahasan selanjutnya...

### Studi Kasus

Studi kasus kali ini berhubungan dengan *Ionic Modal*. Sebelum membahas lebih
lanjut, saya bahas *modal* secara singkat. *Modal* adalah sebuah jendela yang
muncul sementara di antarmuka utama aplikasi perangkat bergerak. Dikatakan
sementara karena jendela ini akan kembali ke antarmuka utama bisa proses telah
selesai. Jendela ini biasanya digunakan untuk menanyakan sebuah pilihan atau
mengubah data yang dipilih[^2]. Gambar bergerak di bawah ini menggambarkan
sebuah *modal*.

![Ionic Modal Example](http://static.meisyal.web.id/images/posts/ionic-modal-example.gif){: .center-image }

CC BY-NC-SA 4.0 "Editing Customer Data in [MIM project][MIM]" by meisyal

Misal dalam satu *view*, kita menaruh dua atau lebih *modal* di mana masing-masing
menangani tugas yang berbeda. *View* tersebut ditampilkan ke dalam kode HTML
berikut,

<ion-view title="Ionic Modals">
  <ion-content ng-controller="multiModalController">
    <div class="card">
      <div class="item item-divider">
        Multiple Modals in One Controller
      </div>
      <div class="item item-text-wrap">
        <div class="button-bar">
          <a class="button" ng-click="openFirstModal()">First Modal</a>
          <a class="button" ng-click="openSecondModal()">Second Modal</a>
        </div>
      </div>
    </div>
  </ion-content>
</ion-view>

Tampak pada potongan kode di atas, ada dua *modal* yang ditandai dengan pemanggilan
fungsi `openFirstModal` dan `openSecondModal`. Kita asumsikan dua fungsi modal
tersebut menangani kasus yang berbeda dalam sebuah *data list view*:

- *First Modal* untuk menambahkan data baru
- *Second Modal* untuk menyunting data yang dipilih.

Selain itu, terdapat *controller* `multiModalController` yang menangani aktivitas
*view* di atas. Kita desain *controller* tersebut sebagai berikut (hanya potongan
kode yang menangani *modal* saja yang ditampilkan),

.controller('multiModalController', function($scope, ..., $ionicModal) {
  $scope.data = ["Item 1", "Item 2", "Item 3"];

  // ...

  // First Modal
  $ionicModal.fromTemplateUrl('templates/first_modal.html', {
    scope: $scope,
    animation: 'slide-in-up'
  }).then(function(modal) {
    $scope.firstModal = modal;
  });

  // Second Modal
  $ionicModal.fromTemplateUrl('templates/second_modal.html', {
    scope: $scope,
    animation: 'slide-in-up'
  }).then(function(modal) {
    $scope.secondModal = modal;
  });

  // Show the First Modal when openFirstModal() is called
  $scope.openFirstModal = function() {
    $scope.firstModal.show();
  };

  // Show the Second Modal when openSecondModal() is called
  $scope.openSecondModal = function() {
    $scope.secondModal.show();
  };

  // ...

});

### Masalah

Ketika potongan kode di atas dijalankan, kita pilih salah satu *modal* untuk
diuji, misal *First Modal* terlebih dahulu. Setelah itu, kita pilih *modal*
selanjutnya, *Second Modal*. Hasil yang akan muncul adalah **munculnya halaman
*modal* yang sama, yaitu fungsi *modal* yang didefinisikan pertama kali**. Dua
*modal* tersebut seharusnya menampilkan *view* yang berbeda sesuai dengan
definisinya.

Dari permasalahan di atas, timbul pertanyaan yang dituangkan dalam rumusan
berikut:

1. Mengapa *Ionic* menampilkan *view* yang sama padahal secara definisi *modal*
   dalam konteks studi kasus ini berbeda?
2. Bagaimana langkah untuk menyelesaikan masalah tersebut?

### Solusi

Untuk rumusan yang pertama, setelah menelusuri, [Modal Controller][modalctrl]
dari *Ionic* hanya bisa didefinisikan satu kali saja dalam satu *controller*.
Namun, kita bisa mendefinisikan dua atau lebih *modal* dalam satu *controller*.
*Modal Controller* ditunjukkan dengan fungsi `openFirstModal` dan `openSecondModal`
dalam studi kasus ini (kita memiliki dua *modal controller*).

Rumusan selanjutnya, rumusan kedua, berhubungan dengan rumusan pertama. Bagaimana
menyelesaikan masalah di atas jika kita hanya diperbolehkan memiliki satu *modal
controller* saja. Idenya sangat sederhana dengan menambahkan atribut `id` pada
*modal* yang telah didefinisikan dan memanggil atribut tersebut ke dalam *modal
controller*,

// ...

  // Improvement of First Modal
  $ionicModal.fromTemplateUrl('templates/first_modal.html', {

    // we need to use an `id` to identify the modal
    id: '1',
    scope: $scope,
    animation: 'slide-in-up'
  }).then(function(modal) {
    $scope.firstModal = modal;
  });

  // Improvement of Second Modal
  $ionicModal.fromTemplateUrl('templates/second_modal.html', {

    // we need to use an `id` to identify the modal
    id: '2',
    scope: $scope,
    animation: 'slide-in-up'
  }).then(function(modal) {
    $scope.secondModal = modal;
  });

// ...

Kemudian, kita harus perbaiki *modal controller* terlebih dahulu yang awalnya dua
menjadi satu. Selanjutnya, kita tampilkan *modal* sesuai dengan *id* yang
telah didefinisikan,

// ...

  // Improvement of Modal Controller
  $scope.openModal = function(index) {
    if (index == 1) {

      // show the first modal, because we set its `id` 1
      $scope.firstModal.show();
    } else if (index == 2) {

      // show the first modal, because we set its `id` 1
      $scope.secondModal.show();
    } else {

      // This part is up to you
      console.log('Please, define the default modal or something');
    }
  }

// ...

Terakhir, kita perbaiki *view* untuk memanggil fungsi `openModal()` beserta
dengan parameter `id` dari *modal* yang sesuai,

<ion-view title="Ionic Modals">
  <ion-content ng-controller="multiModalController">
    <div class="card">
      <div class="item item-divider">
        Multiple Modals in One Controller
      </div>
      <div class="item item-text-wrap">
        <div class="button-bar">
          <!-- call modal controller with its parameter -->
          <a class="button" ng-click="openModal(1)">First Modal</a>
          <a class="button" ng-click="openModal(2)">Second Modal</a>
        </div>
      </div>
    </div>
  </ion-content>
</ion-view>

### Kesimpulan

Dari studi kasus ini, saya belajar bahwa sebuah masalah pasti ada penyelesaiannya.
Penyelesaian bisa jadi sangat sederhana atau kompleks. Mengubah masalah menjadi
rumusan masalah adalah langkah awal penyelesaian masalah. Tidak lupa dokumentasi
terkait. Studi kasus ini memberikan solusi sederhana dari permasalahan *Ionic
Modal*. Saya yakin ada alternatif lain selain solusi di atas.

*Happy coding!*

[ionic]: http://ionicframework.com/
[angularjs]: https://angularjs.org/
[drifty]: http://drifty.com/
[ionicrepo]: https://github.com/driftyco/ionic
[MIM]: https://github.com/meisyal/MIM
[modalctrl]: http://ionicframework.com/docs/api/controller/ionicModal/

[^1]: "Ionic also uses AngularJS for a lot of the core functionality of the
       framework." [Welcome to Ionic](http://ionicframework.com/docs/guide/preface.html)

[^2]: "The Modal is a content pane that can go over the user's main view temporarily.
      Usually used for making a choice or editing an item."
      [$ionicModal - Service in module ionic](http://ionicframework.com/docs/api/service/$ionicModal/)
