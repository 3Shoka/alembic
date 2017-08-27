---
title: Java Expressions Language EL
category: Java
excerpt: |
  Java Expression Language (EL) adalah mekanisme yang memungkinkan komunikasi secara dinamis aplikasi berbasis JSP/JSF, kita menggunakan ekspresi ini di presentation layer untuk dihubungkan dengan application logic layer.
feature_text: |
  ## Java Expressions Language EL
  Akses Dinamis ke Data Aplikasi JSF melalui Bahasa Ekspresi (Expression Language)
feature_image: "https://unsplash.it/1200/400?image=1048"
image: "https://unsplash.it/1200/400?image=1048"
tags: java, jsf, el expression
---

**Java Expression Language (EL)** adalah *mekanisme* yang memungkinkan komunikasi secara dinamis aplikasi berbasis **JSP/JSF**, kita menggunakan ekspresi ini di presentation layer untuk dihubungkan dengan application logic layer. Biasanya EL expressions digunakan di halaman JSP atau JSF tapi bisa juga digunakan di tempat lain, di `faces-config.xml` misalnya. EL juga mendukung berbagai macam kategori operator : arithmetic, relational, logical, conditional, dll

## Operator yang di dukung di EL

Textual	| Deskripsi | Simbol 
---|---|---
 A + B | Addition | + 
 A - B | Subtraction | -
 A * B | Multiplication | * 
 A {div, /} B | Arithmetic operator division | /, div 
 A {mod, %} B | Arithmetic operator modulo | %, mod 
 A {and, &&} B | Logical AND | &&, and 
 A {or, &#124;&#124;} B | Logical OR | &#124;&#124;, or 
 {not, !} A | Logical opposite | !, not
 A {lt, <} B | Relational less than | <, lt
 A {gt, >} B | Relational greater than | >, gt
 A {le, <=} B | Relational less than or equal to | <=, le
 A {ge, >=} B | Relational greater than or equal to | >=, ge
 A {eq, ==} B | Equal to | ==, eq
 A {ne, !=} B | Not equal to | !=, ne
 A = B | Assignment (EL 3.0) | =
 A ; B | Semicolon (EL 3.0) | ;
 A += B | String concatenation (EL 3.0) | +=
 A -> B | Lambda expression (EL 3.0) | ->
 empty A | Determine wheter a value is null or empty | 
 A ? B : C | Evaluate B or C, depending on the result of the evaluation of A (ternary operator) | ?:
  | Used when writing EL expressions | .
  | Used when writing EL expressions | []

## Menghubungkan managed bean
Biasanya `managed bean` akan akan seperti kode berikut ini (dalam contoh ini nama class bean nya `BukuBean`) :

``` java
@ManagedBean
// scope bean nya
public class BukuBean{
	
}
```

atau versi CDI nya, seperti ini

``` java 
@Named 
// scope
public class BukuBean{

}

```

atau dengan di beri nama beannya

``` java 
@ManagedBean(name = "bukunyaBean")
// scope bean nya
public class BukuBean{
	
}
```

atau versi CDI nya, seperti ini

``` java 
@Named(value = "bukunyaBean")
// scope
public class BukuBean{

}
``` 
Untuk dua contoh kode diatas nama bean nya akan menjadi `bukuBean` jika tidak di set value/nama bean nya dengan ketentuan sesuai nama class bean yang huruf awalnya jadi huruf kecil. jadi oemanggilan untuk di jsf nya begini `#{bukuBean}` sedangkan dua contoh berikutnya menjadi `#{bukunyaBean}`, sesuai dengan value/name yg di berikan.

Jika managed bean yg ditulis tidak ditemukan di scpoe manapun maka nilai nya akan menjadi `null`

## Menghubungkan properti managed bean

Managed bean biasanya mempunyai private field yang diakses melalui setter getter method sebagai properti dan beberapa public method yang dapat mengubah properti ini untuk menyajikan data sesuai logic nya.

EL expresion yang digunakan untuk mengakses properti ini adallah dot (titik) `.` atau square bracket `[]`. Sebagai contoh `bukuBean` mempunyai dua field properti seperti berikut: 

``` java
private String pengarang = "Ashokani";
private String penerbit = "Some Publisher";
```

EL bisa mengakses field ini melalui method getter nya, jadi kita perlu menambahkan getternya seperti ini

``` java 
public String getPengarang() {
	return pengarang;
}

public String getPenerbit() {
	return penerbit;
}

```

Jadi sekarang expression bisa mengakses properti `pengarang` menggunakan dot `.` untuk mengacu pada properti itu `#{bukuBean.pengarang}`
atau bisa menggunakan square bracket `[]` `#{bukuBean['pengarang']}`

JSF mengevaluasi expression ini dari kiri ke kanan, maksudnya jsf mencari `bukuBean` di semua scope yang ada (seperti `request`, `session` dan `application`). Kemudian bean nya di *instantiated* dan method getter `getPengarang` dipanggil (jika properti type nya boolean, nama method getter akan menjadi `isXXX`).

## Nested properti managed bean

Biasanya managed bean menggunakan nested properti, semacam properti yang diakses menggunakan `.` atau `[]` beberapa kali dalam satu expression.
Sebagai contoh, managed bean `BukuBean` bisa saja menggunakan `pengarang` sebagai object yang berisi detil pengarang (seperti namaPengarang, alamatPengarang dll) bisa di representasikan mengguakan class `Pengarang`, jadi properti `pengarang` typenya bukan `String` lagi melainkan sebuah class tersendiri yang berisi detil pengarang nya

``` java 
public class Pengarang() {
	private String namaPengarang;
	private String alamatPengarang;
	private Date tglLhrPengarang;

	public String getNamaPengarang() {
		return namaPengarang;
	}

	public String getAlamatPengarang() {
		return alamaPengarang;
	}

	public Date getTglLhrPengarang() {
		return tglLhrPengarang;
	}
}

```

managed bean di `BukuBean` menjadi seperti berikut :
``` java 
@Named
public class BukuBean() {
	private Pengarang pengarang;
	private String penerbit;

	public Pengarang getPengarang() {
		return pengarang;
	}

	public String getPenerbit() {
		return penerbit;
	}
}
```
tahu kan gimana cara pemanggilan properti `namaPengarang` menggunakan notasi dot `.`, begini `#{bukuBean.pengarang.namaPengarang}`
atau menggunakan square bracket `#{bukuBean['pengarang']['namaPengarang']}`
atau bisa menggunakan kedua notasi dalam satu expression 
``` xhtml 
#{bukuBean.pengarang['namaPengarang']}
#{bukuBean['pengarang'].namaPengarang}
```
Tentu saja class `Pengarang` bisa berisi nested properti juga dan seterusnya, kita hanya menggukana dot atau square bracket untuk mendapatkan properti nya.


## Mengakses Collections
Item collection (array, list, map, set dll) bisa diakses dari EL expression dengan menentukan nilai literal yang bisa dikonversi ke integer atau notasi `[]` dengan integer tanpa tanda petik. Contoh managed bean `BukuBean` mempunyai array bernama `namaPeminjam` yang menampung daftar nama peminjam buku, data array nya sebagai berikut :
``` java 
private String[] namaPeminjam = {"Paimin", "Tukijo", "Suparman"};

public String[] getNamaPeminjam() {
	return namaPeminjam;
}
```
dan sekarang kita bisa mengakses nama peminjam dari array dengan menentukan posisinya di array `#{bukuBean.namaPeminjam[0]}`

Walaupun kadang kita perlu meng *iterate* melalui array nya daripada mengakses spesifik item. Ini dapat di tangani menggunakan `c:forEach` JSTL tag. Kode berikut ini mengiterate array *namaPeminjam* dan menampilkan masing masing item, ini hanya sebagai contoh
``` xhtml
<c:forEach begin="0" 
		end="${fn:length(bukuBean.namaPeminjam)-1"
		var="i">
		#{bukuBean.namaPeminjam[i]},
</c:forEach>
```
atau menggunakan cara yg lebih sederhana
``` xhtml
<c:forEach var="peminjam" items="#{bukuBean.namaPeminjam}">
	<i>#{peminjam}</i>
</c:forEach>
```
Bisa juga menggunakan tag `<ui:repeat>` seperti berikut:
``` xhtml 
<ui:repeat var="peminjam" value="#{bukuBean.namPeminjam}">
	<i>#{peminjam}</i>
</ui:repeat>
```
Kita bisa menggunkan cara yang sama untuk setiap `List` sebagai contoh jika di `List` expression `#{bukuBean.namaPeminjam[0]}` sama dengan di java `namaPeminjam.get(0)` dan `namaPeminjam.set(0, valuenya)`.

Jika type collection nya key-value (contoh, `Map`), EL expression menentukan item berdasarkan key. Sebagai contoh mari kita tambahkan `Map` di `bukuBean` yang menampung data buku yang telah dipinjam oleh peminjam:
``` java 
private Map<String, String> bukuDiPinjam = new HashMap<>();

bukuDiPinjam.put("Matematika", "Paiman");
bukuDiPinjam.put("Bahasa Indonesia", "Paijo");

public Map<String, String> getBukuDiPinjam() {
	return bukuDiPinjam;
}
```

EL expression yang mengakses item menggunakan key *Matematika* bisa ditulis seperti kode berikut: `#{bukuBean.bukuDiPinjam.Matematika}`

cara ini tidak berlaku di array atau list, contoh `#{bukuBean.bukuDiPinjam.0}` ini tidak benar. Jika key nya adalah variable yg (mungkin) tidak diterima (contoh, *Bahasa Indonesia*), bisa menggunakan bracket dan di kutip `#{bukuBean.bukuDiPinjam["Bahasa Indonesia"]}`.