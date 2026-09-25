# Bartleby

"Yapmamayı tercih ederim."

Kişisel alışkanlık ve bırakma takipçisi. Tek dosya PWA, localStorage öncelikli, isteğe bağlı uçtan uca şifreli Supabase yedeği.

## Yayına alma (GitHub Pages)
1. Yeni bir repo aç (örn. `bartleby`).
2. Bu klasördeki dosyaların hepsini repo köküne yükle.
3. Settings > Pages > Source: `main` / root.
4. Telefonda `https://<kullanıcı>.github.io/bartleby/` adresini aç.
   - iPhone: Safari > Paylaş > Ana Ekrana Ekle.
   - Android: Chrome menüsü > Uygulamayı yükle.
   Ana ekrana eklemek önemli: iOS, eklenmemiş sitelerin verisini 7 gün etkileşimsizlikten sonra silebiliyor.

## Eski veriyi taşıma
Claude’daki sürümde Ayar > Yedek > Yedeği kopyala. Yeni sürümde Ayar > Yedek > Yedekten yükle.

## Supabase şifreli yedek
SQL Editor’a yapıştır:

```sql
create table bartleby_backup (
  id text primary key,
  data text not null,
  updated_at timestamptz default now()
);
alter table bartleby_backup enable row level security;
create policy "sifreli yedek" on bartleby_backup
  for all to anon using (true) with check (true);
```

Uygulamada Ayar > Bulut yedeği: proje URL’si, anon key ve bir yedek parolası gir.
- Veri telefonda PBKDF2 (210.000 tur) + AES-GCM 256 ile şifrelenir; sunucuya sadece şifreli blok gider.
- Satır kimliği parolanın SHA-256 özetinden türetilir; başka cihazda geri yüklemek için URL, key ve parola yeter.
- Anon key’e sahip biri blokları görebilir ama parola olmadan açamaz. Parolayı unutursan yedek açılamaz.

## Hatırlatıcılar
Ayar > Hatırlatıcılar > .ics dosyasını indir, telefon takvimine ekle. Günlük kayıt ve Pazar akşamı haftalık değerlendirme için tekrar eden, nötr başlıklı etkinlikler oluşturur.
