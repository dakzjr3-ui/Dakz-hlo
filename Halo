<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>DAKZ AI ROBOT - Ulangan PPKn</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#07111f;color:#fff;font-family:Arial,sans-serif}
header{text-align:center;padding:25px 12px;background:linear-gradient(135deg,#06172e,#075b91);border-bottom:3px solid #20c8f5}
header h1{font-size:25px;margin:10px 0}
header p{color:#b9eaff;font-size:13px}
.robot{font-size:70px;margin:10px}
main{max-width:650px;margin:auto;padding:15px}
.card{background:#102138;border:1px solid #28547b;padding:18px;border-radius:16px;margin-bottom:14px}
input{width:100%;padding:13px;margin:7px 0 15px;background:#071525;border:1px solid #426a8d;border-radius:9px;color:white}
button{cursor:pointer;border:0;border-radius:10px;padding:14px;font-weight:bold}
.primary{width:100%;background:#20c8f5;color:#04111d}
.option{display:block;text-align:left;width:100%;margin:9px 0;background:#0a192b;color:white;border:1px solid #365875}
.option.selected{border:2px solid #38d9ff}
.option.correct{background:#104532;border-color:#54f5a6}
.option.wrong{background:#49202a;border-color:#ff7777}
.hidden{display:none!important}
.info{color:#a9c4dd;font-size:13px;line-height:1.6}
#timer{font-size:21px;font-weight:bold;color:#54eaff}
#warning{position:fixed;inset:0;background:#000e;display:flex;align-items:center;justify-content:center;padding:20px;z-index:99}
.warningBox{background:#26131a;border:2px solid #ff4545;border-radius:18px;padding:24px;text-align:center;max-width:400px}
.warningBox h2{color:#ff7474}
#score{font-size:52px;color:#4ce8ff;font-weight:bold}
</style>
</head>
<body>
<header>
<div>🤖 DAKZ AI ROBOT</div>
<h1>ULANGAN PPKn SMP KELAS 7</h1>
<p>30 SOAL • TIMER • PENILAIAN OTOMATIS</p>
<div class="robot">🤖</div>
</header>

<main>
<section id="login" class="card">
<h2>📋 Identitas Peserta</h2>
<label>Nama siswa</label>
<input id="nama" placeholder="Masukkan nama siswa">
<label>Kelas</label>
<input id="kelas" placeholder="Contoh: VII A">
<p class="info">Ujian berlangsung selama 30 menit. Jangan meninggalkan halaman ujian. Pelanggaran yang terdeteksi berulang kali dapat mengakhiri ujian.</p>
<button class="primary" onclick="mulai()">MULAI ULANGAN</button>
</section>

<section id="ujian" class="hidden">
<div class="card">
<div style="display:flex;justify-content:space-between;gap:10px">
<b id="nomor"></b><span id="timer">30:00</span>
</div>
<p id="identitas" class="info"></p>
<div style="height:5px;background:#263b51;margin:12px 0">
<div id="progress" style="height:5px;background:#35d9ff;width:0"></div>
</div>
<h3 id="soal"></h3>
<div id="opsi"></div>
<button id="lanjut" class="primary" onclick="berikutnya()">SOAL BERIKUTNYA →</button>
</div>
</section>

<section id="hasil" class="card hidden">
<h2 id="judulHasil">HASIL ULANGAN</h2>
<div id="score">0</div>
<p id="ringkasan"></p>
<div id="pembahasan"></div>
<button class="primary" onclick="location.reload()">KEMBALI</button>
</section>
</main>

<div id="warning" class="hidden">
<div class="warningBox">
<div style="font-size:45px">⚠️</div>
<h2 id="judulWarning">PERINGATAN</h2>
<p id="pesanWarning">Jangan meninggalkan halaman ujian.</p>
<button class="primary" onclick="tutupWarning()">SAYA MENGERTI</button>
</div>
</div>

<script>
/*
OPSIONAL SERVER:
Isi URL server milikmu di SERVER_URL setelah backend benar-benar dibuat.
Server harus menyediakan endpoint pemeriksaan akun dan endpoint ban.
Tanpa backend, ban hanya berlaku pada browser/perangkat ini.
*/
const SERVER_URL="";

const bank=[
["Apa dasar negara Indonesia?",["UUD 1945","Pancasila","Bhinneka Tunggal Ika","Proklamasi"],1,"Pancasila adalah dasar negara Indonesia."],
["Berapa jumlah sila Pancasila?",["3","4","5","6"],2,"Pancasila terdiri atas lima sila."],
["Lambang negara Indonesia adalah...",["Garuda Pancasila","Merpati","Harimau","Rajawali"],0,"Lambang negara Indonesia adalah Garuda Pancasila."],
["Arti Bhinneka Tunggal Ika adalah...",["Bersatu kita teguh","Berbeda-beda tetapi tetap satu","Satu budaya saja","Semua harus sama"],1,"Semboyan ini menekankan persatuan dalam keberagaman."],
["Konstitusi tertulis Indonesia adalah...",["Pancasila","UUD NRI Tahun 1945","Peraturan sekolah","Adat"],1,"UUD NRI Tahun 1945 merupakan konstitusi tertulis Indonesia."],
["Rumusan Pancasila terdapat dalam Pembukaan UUD 1945 alinea...",["Pertama","Kedua","Ketiga","Keempat"],3,"Rumusan Pancasila terdapat pada alinea keempat."],
["Menghormati teman yang berbeda agama sesuai sila ke...",["Satu","Dua","Tiga","Empat"],0,"Sila pertama berkaitan dengan Ketuhanan Yang Maha Esa."],
["Menolong orang yang membutuhkan mencerminkan sila ke...",["Satu","Dua","Tiga","Lima"],1,"Sila kedua mengajarkan kemanusiaan yang adil dan beradab."],
["Menjaga persatuan mencerminkan sila ke...",["Satu","Dua","Tiga","Lima"],2,"Sila ketiga adalah Persatuan Indonesia."],
["Musyawarah mencerminkan sila ke...",["Satu","Dua","Empat","Lima"],2,"Sila keempat mengutamakan musyawarah."],
["Bersikap adil terhadap teman sesuai sila ke...",["Dua","Tiga","Empat","Lima"],3,"Sila kelima berkaitan dengan keadilan sosial."],
["Norma adalah...",["Pedoman perilaku","Nama organisasi","Alat komunikasi","Permainan"],0,"Norma menjadi pedoman perilaku dalam masyarakat."],
["Norma yang bersumber dari ajaran Tuhan adalah norma...",["Kesopanan","Kesusilaan","Agama","Hukum"],2,"Norma agama bersumber dari ajaran agama."],
["Norma dengan sanksi resmi negara adalah norma...",["Kebiasaan","Hukum","Kesopanan","Kesusilaan"],1,"Norma hukum ditegakkan oleh lembaga berwenang."],
["Contoh norma kesopanan adalah...",["Berbicara santun","Menerobos lampu merah","Merusak barang","Mengejek teman"],0,"Berbicara santun merupakan perilaku sopan."],
["Hak siswa di sekolah adalah...",["Mendapat pembelajaran","Merusak meja","Mengganggu kelas","Mengabaikan tugas"],0,"Siswa berhak memperoleh pembelajaran."],
["Kewajiban siswa adalah...",["Menjaga kebersihan","Mencoret dinding","Membuang sampah","Merusak tanaman"],0,"Menjaga kebersihan merupakan kewajiban bersama."],
["Keseimbangan hak dan kewajiban menciptakan kehidupan...",["Kacau","Tidak adil","Tertib","Bermusuhan"],2,"Hak dan kewajiban yang seimbang mendukung ketertiban."],
["Contoh gotong royong di sekolah adalah...",["Membersihkan kelas bersama","Bertengkar","Menyembunyikan alat","Membiarkan kelas kotor"],0,"Gotong royong adalah bekerja bersama."],
["Keputusan musyawarah sebaiknya...",["Dihormati bersama","Diabaikan","Selalu ditolak","Dibatalkan sepihak"],0,"Keputusan bersama perlu dihormati."],
["Saat berbeda pendapat, sebaiknya...",["Mengejek","Mendengarkan","Memaksa","Bermusuhan"],1,"Perbedaan pendapat harus dihargai."],
["Contoh keberagaman Indonesia adalah...",["Suku dan budaya","Ukuran buku","Warna meja","Jadwal kelas"],0,"Indonesia memiliki beragam suku, agama, dan budaya."],
["Sikap terhadap budaya daerah lain adalah...",["Menghargai","Merendahkan","Mengejek","Melarang semuanya"],0,"Menghargai budaya memperkuat persatuan."],
["Bahasa persatuan Indonesia adalah...",["Jawa","Indonesia","Inggris","Sunda"],1,"Bahasa Indonesia adalah bahasa persatuan."],
["Tujuan aturan sekolah adalah...",["Menciptakan ketertiban","Membuat takut","Melarang belajar","Menghilangkan kerja sama"],0,"Aturan membantu menjaga ketertiban."],
["Tanggung jawab di rumah contohnya...",["Membantu keluarga","Merusak barang","Mengabaikan tugas","Bertengkar"],0,"Membantu keluarga sesuai kemampuan adalah tanggung jawab."],
["Saat melihat perundungan, sebaiknya...",["Ikut mengejek","Melapor kepada guru","Menyebarkan video","Diam saja selalu"],1,"Laporkan kepada orang dewasa tepercaya dan utamakan keselamatan."],
["Keputusan bersama dihormati agar...",["Kepentingan bersama berjalan baik","Satu orang menang","Pendapat dilarang","Aturan hilang"],0,"Menghormati keputusan menjaga kebersamaan."],
["Contoh cinta tanah air adalah...",["Belajar tekun dan menjaga lingkungan","Merusak fasilitas","Menghina budaya","Melanggar aturan"],0,"Perilaku positif membantu masyarakat dan negara."],
["Jika melakukan kesalahan, kita harus...",["Menyalahkan teman","Mengakui dan memperbaiki","Menyembunyikan","Mengulangi"],1,"Tanggung jawab berarti berani memperbaiki kesalahan."]
];

let nama="",kelas="",soal=[],jawaban=[],pos=0,deteksi=0,sedangUjian=false,selesai=false,waktu=1800,interval=null,peringatanTerbuka=false;

function getBan(){try{return JSON.parse(localStorage.getItem("dakz_ban")||"{}")}catch(e){return {}}}
function isBanned(n){return !!getBan()[n.trim().toLowerCase()]}
function banLocal(n,reason){
 let b=getBan();b[n.trim().toLowerCase()]={nama:n,reason:reason,date:new Date().toISOString()};
 localStorage.setItem("dakz_ban",JSON.stringify(b))
}
async function serverCheck(n){
 if(!SERVER_URL)return false;
 try{
  let r=await fetch(SERVER_URL+"/check?nama="+encodeURIComponent(n));
  if(!r.ok)throw Error();
  let d=await r.json();return !!d.banned
 }catch(e){throw Error("Server ban tidak dapat dihubungi. Ujian tidak dimulai demi keamanan.")}
}
async function serverBan(n,reason){
 if(!SERVER_URL)return;
 try{await fetch(SERVER_URL+"/ban",{method:"POST",headers:{"Content-Type":"application/json"},body:JSON.stringify({nama:n,reason})})}catch(e){}
}
async function mulai(){
 nama=document.getElementById("nama").value.trim();
 kelas=document.getElementById("kelas").value.trim();
 if(!nama||!kelas){alert("Isi nama dan kelas terlebih dahulu.");return}
 if(isBanned(nama)){alert("AKUN DIBLOKIR: nama ini sudah terkena ban pada browser ini.");return}
 try{if(await serverCheck(nama)){banLocal(nama,"Ban dari server");alert("Nama akun ini diblokir oleh server.");return}}
 catch(e){alert(e.message);return}
 soal=bank.map(q=>({t:q[0],o:q[1].map((x,i)=>({t:x,b:i===q[2]})).sort(()=>Math.random()-.5),ex:q[3]})).sort(()=>Math.random()-.5);
 jawaban=Array(30).fill(null);pos=0;deteksi=0;waktu=1800;selesai=false;sedangUjian=true;
 document.getElementById("identitas").textContent=nama+" • Kelas "+kelas;
 document.getElementById("login").classList.add("hidden");document.getElementById("ujian").classList.remove("hidden");
 render();
 interval=setInterval(()=>{if(selesai)return;waktu--;document.getElementById("timer").textContent=String(Math.floor(waktu/60)).padStart(2,"0")+":"+String(waktu%60).padStart(2,"0");if(waktu<=0)finish(false,"Waktu habis.")},1000)
}
function render(){
 let q=soal[pos];document.getElementById("nomor").textContent="Soal "+(pos+1)+"/30";
 document.getElementById("progress").style.width=(pos/30*100)+"%";
 document.getElementById("soal").textContent=q.t;
 let box=document.getElementById("opsi");box.innerHTML="";
 q.o.forEach((o,i)=>{let b=document.createElement("button");b.className="option"+(jawaban[pos]===i?" selected":"");b.textContent=String.fromCharCode(65+i)+". "+o.t;b.onclick=()=>{jawaban[pos]=i;render()};box.appendChild(b)});
 document.getElementById("lanjut").textContent=pos===29?"KUMPULKAN ULANGAN":"SOAL BERIKUTNYA →"
}
function berikutnya(){
 if(jawaban[pos]===null&&!confirm("Soal belum dijawab. Lanjut?"))return;
 if(pos<29){pos++;render()}else if(confirm("Kumpulkan jawaban sekarang?"))finish(false,"Ulangan selesai.")
}
async function finish(failed,msg){
 if(selesai)return;selesai=true;sedangUjian=false;clearInterval(interval);
 document.getElementById("ujian").classList.add("hidden");document.getElementById("hasil").classList.remove("hidden");
 let benar=0;soal.forEach((q,i)=>{if(jawaban[i]!==null&&q.o[jawaban[i]].b)benar++});
 let nilai=failed?0:Math.round(benar/30*100);
 document.getElementById("judulHasil").textContent=failed?"AKUN ULANGAN DIBLOKIR":"HASIL ULANGAN";
 document.getElementById("score").textContent=nilai;
 document.getElementById("ringkasan").textContent=failed?msg:"Nama: "+nama+" • Kelas: "+kelas+" • Benar: "+benar+"/30 • "+msg;
 let out=document.getElementById("pembahasan");out.innerHTML="<h3>Pembahasan</h3>";
 soal.forEach((q,i)=>{let correct=q.o.find(x=>x.b).t;let chosen=jawaban[i]===null?"Tidak dijawab":q.o[jawaban[i]].t;out.innerHTML+="<p><b>"+(i+1)+". "+q.t+"</b><br>Jawabanmu: "+chosen+"<br>Jawaban benar: "+correct+"<br><span class='info'>"+q.ex+"</span></p>"});
 if(failed){banLocal(nama,msg);await serverBan(nama,msg)}
}
function pelanggaran(){
 if(!sedangUjian||selesai||peringatanTerbuka)return;
 deteksi++;peringatanTerbuka=true;
 document.getElementById("judulWarning").textContent=deteksi===1?"PERINGATAN":"AKUN DIBLOKIR";
 document.getElementById("pesanWarning").textContent=deteksi===1?"Halaman kehilangan fokus. Ini bisa terjadi saat berpindah aplikasi atau split screen. Jika terjadi lagi, ujian akan dihentikan.":"maaf anda gagal karena menyontek tanpa izin";
 document.getElementById("warning").classList.remove("hidden");
 document.querySelector("#warning button").textContent=deteksi>=2?"LIHAT HASIL":"SAYA MENGERTI";
}
function tutupWarning(){
 document.getElementById("warning").classList.add("hidden");peringatanTerbuka=false;
 if(deteksi>=2)finish(true,"maaf anda gagal karena menyontek tanpa izin");
}
document.addEventListener("visibilitychange",()=>{if(document.hidden)pelanggaran()});
window.addEventListener("blur",pelanggaran);
window.addEventListener("beforeunload",e=>{if(sedangUjian&&!selesai){e.preventDefault();e.returnValue=""}});
</script>
</body>
</html>
