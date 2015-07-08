---
layout: post
title:  "Hello World!"
date:   2015-07-08 21:51:40
author: "meisyal"
categories: general
---
Saya mengawali tulisan pertama di *blog* ini dengan judul "Hello World!". Iya,
"Hello World!". Kalimat sederhana terdiri dari dua kata, "hello" dan "world",
dan diakhiri oleh tanda perintah "!".

Saya terjemahkan dua kata dan tanda perintah tersebut ke dalam bahasa [Ruby][Ruby].
Potongan kode program ditampilkan sebagai berikut:

{% highlight ruby %}
# Kelas PengucapSalam
class PengucapSalam
  def inisialisasi(kata)
    @kata = kata
  end

  def ucapkan
    puts "Hello #{kata}!"
  end
end

# Buat sebuah objek baru
p = PengucapSalam.new("World")

# Ucapkan salam
p.ucapkan
{% endhighlight %}

Maaf, saya harus mengakhiri tulisan pertama saya sampai di sini (isi akan diperbaiki).
Saya hanya berharap salam "Hello World!" di atas tersampaikan kepada pembaca walaupun
isi tulisan ini belum tersampaikan seutuhnya. Terakhir, informasi lebih lanjut
terkait *Focused Samurai's blog* bisa dipelajari di [sini][about].

Terima kasih.

[Ruby]: https://www.ruby-lang.org/
[about]: http://meisyal.web.id/about/
