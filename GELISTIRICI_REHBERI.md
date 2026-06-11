# TurkiyeCraft Geliştirici Rehberi

## Dizin Yapısı

```
VPS: /opt/cuberite/
├── client/                  ← Minecraft 1.8.9 İstemci Kaynak Kodu (MCP-919)
│   ├── src/minecraft/       ← BURASI DÜZENLENİR
│   ├── build/               ← Build çıktıları (JAR, ZIP, version.json)
│   ├── bin/                 ← Derlenmiş .class dosyaları
│   ├── jars/                ← Orijinal MC jar + kütüphaneler
│   └── build.sh             ← Build scripti
└── Server/                  ← Cuberite Sunucu
    ├── Plugins/             ← Lua eklentiler (BURASI DÜZENLENİR)
    ├── crafting.txt         ← Craftlama tarifleri
    ├── items.ini            ← Item ID listesi
    ├── monsters.ini         ← Mob spawn ayarları
    ├── settings.ini         ← Sunucu ayarları
    └── dunyaharitasi/       ← Dünya dosyaları
        └── world.ini        ← Dünya ayarları (spawn, gamemode vb.)
```

---

## 1. YENİ ITEM EKLEME

### İstemci Tarafı (Java)

**Temel Item dosyaları:**
```
src/minecraft/net/minecraft/item/
├── Item.java           ← Tüm itemların üst sınıfı — BURADAN TÜRET
├── ItemFood.java       ← Yiyecek itemları için örnek
├── ItemSword.java      ← Silah itemları için örnek
├── ItemTool.java       ← Alet itemları için örnek (kazma, balta vb.)
├── ItemArmor.java      ← Zırh itemları için örnek
└── ItemPotion.java     ← İçecek/potion itemları için örnek
```

**Kayıt dosyası:**
```
src/minecraft/net/minecraft/init/
└── Items.java          ← BURAYA YENİ İTEM KAYDEDİLİR
                          Örnek: public static final Item elmaSword = ...
```

**Yaratıcı mod sekmeleri:**
```
src/minecraft/net/minecraft/creativetab/
└── CreativeTabs.java   ← Hangi sekmede görüneceği ayarlanır
```

### Sunucu Tarafı (Lua)
```
Server/items.ini        ← Yeni item ID ve ismi buraya eklenir
Server/crafting.txt     ← Crafting tarifi buraya eklenir
```

**Crafting tarifi örneği** (`crafting.txt`):
```
# Format: Sonuc = Malzeme, X:Y | Malzeme2, X:Y
Diamond, 1 = IronIngot, 1:1 | IronIngot, 2:1 | IronIngot, 3:1
```

---

## 2. YENİ BLOK EKLEME

### İstemci Tarafı (Java)

**Temel Blok dosyaları:**
```
src/minecraft/net/minecraft/block/
├── Block.java          ← Tüm blokların üst sınıfı — BURADAN TÜRET
├── BlockOre.java       ← Cevher bloğu örneği
├── BlockCrops.java     ← Tarım bloğu örneği (büyüyen)
├── BlockContainer.java ← Sandık/envanter açan blok örneği
├── BlockFurnace.java   ← Etkileşimli blok örneği
└── BlockDoor.java      ← Açılır-kapanır blok örneği
```

**Blok kaydı:**
```
src/minecraft/net/minecraft/init/
└── Blocks.java         ← BURAYA YENİ BLOK KAYDEDİLİR
                          Örnek: public static final Block turkTasi = ...
```

**Blok durumları (state/property):**
```
src/minecraft/net/minecraft/block/properties/   ← Blok özellikleri (yön, güç vb.)
src/minecraft/net/minecraft/block/state/        ← Durum makinesi
```

### Sunucu Tarafı (Lua)
```
Server/items.ini        ← Blok ID ve ismi (bloklar da items.ini'dedir)
```

---

## 3. YENİ MOB EKLEME

### İstemci Tarafı (Java)

**Düşman Mob (Monster):**
```
src/minecraft/net/minecraft/entity/monster/
├── EntityMob.java      ← Düşman mob üst sınıfı — BURADAN TÜRET
├── EntityZombie.java   ← Zombie örneği (temel yürüyen düşman)
├── EntitySkeleton.java ← Ok atan düşman örneği
├── EntityCreeper.java  ← Patlayan düşman örneği
└── EntitySpider.java   ← Tırmanabilen düşman örneği
```

**Pasif/Evcil Mob:**
```
src/minecraft/net/minecraft/entity/passive/
├── EntityAnimal.java   ← Hayvan üst sınıfı
├── EntityCow.java      ← Basit hayvan örneği
├── EntityWolf.java     ← Evcilleştirilebilen mob örneği
└── EntityVillager.java ← NPC/köylü örneği (ticaret yapan)
```

**3D Model:**
```
src/minecraft/net/minecraft/client/model/
├── ModelBase.java      ← Tüm modellerin üst sınıfı — BURADAN TÜRET
├── ModelBiped.java     ← İnsan şekilli model (Zombie vb.)
├── ModelCreeper.java   ← Dört ayaklı olmayan örnek
└── ModelCow.java       ← Dört ayaklı model örneği
```

**Mob Yapay Zekası:**
```
src/minecraft/net/minecraft/entity/ai/
├── EntityAIBase.java        ← Tüm AI görevlerinin üst sınıfı
├── EntityAIAttackMelee.java ← Yakın dövüş saldırısı
├── EntityAIFollowOwner.java ← Sahibini takip etme
├── EntityAIWander.java      ← Rastgele gezme
└── EntityAILookAtPlayer.java← Oyuncuya bakma
```

### Sunucu Tarafı (Lua)
```
Server/monsters.ini     ← Mob spawn oranları ve davranışları
```

**Yeni Lua plugin ile mob davranışı eklemek:**
```
Server/Plugins/BenimPlugin/init.lua  ← Yeni klasör + init.lua oluştur
```

---

## 4. GUI EKLEME

### İstemci Tarafı (Java)

**Mevcut GUI dosyaları:**
```
src/minecraft/net/minecraft/client/gui/
├── GuiScreen.java      ← Tüm ekranların üst sınıfı — BURADAN TÜRET
├── GuiMainMenu.java    ← Ana menü (logonu değiştirmek için)
├── GuiIngame.java      ← Oyun içi HUD (can barı, açlık vb.)
├── GuiChat.java        ← Sohbet kutusu
├── GuiOptions.java     ← Ayarlar menüsü
├── GuiButton.java      ← Düğme bileşeni
├── GuiTextField.java   ← Metin giriş kutusu
└── GuiSlot.java        ← Kaydırmalı liste
```

**Envanter GUI:**
```
src/minecraft/net/minecraft/client/gui/inventory/
```

**Yeni GUI eklemek için temel yapı:**
```java
// src/minecraft/net/minecraft/client/gui/GuiTurkCraft.java
public class GuiTurkCraft extends GuiScreen {
    @Override
    public void initGui() { /* butonları ekle */ }

    @Override
    public void drawScreen(int x, int y, float ticks) { /* çiz */ }
}
```

---

## 5. BÜYÜ/ENCHANTMENT EKLEME

```
src/minecraft/net/minecraft/enchantment/
├── Enchantment.java    ← Büyü üst sınıfı — BURADAN TÜRET
├── EnchantmentDamage.java  ← Hasar büyüsü örneği
└── EnchantmentHelper.java  ← Büyü hesaplamalarına buradan erişilir
```

---

## 6. POTION/ETKİ EKLEME

```
src/minecraft/net/minecraft/potion/
├── Potion.java         ← Etki üst sınıfı — BURADAN TÜRET
├── PotionEffect.java   ← Etki uygulamak için
└── PotionHelper.java   ← Potion mantığı
```

---

## 7. SUNUCU PLUGIN YAZMA (Lua)

**Yeni plugin oluşturma:**
```
Server/Plugins/
└── BenimPlugin/
    └── init.lua        ← Plugin giriş noktası
```

**`init.lua` şablonu:**
```lua
function Initialize(Plugin)
    Plugin:SetName("BenimPlugin")
    Plugin:SetVersion(1)

    -- Olayları dinle
    cPluginManager.AddHook(cPluginManager.HOOK_PLAYER_JOINED, OnOyuncuGirdi)
    cPluginManager.AddHook(cPluginManager.HOOK_PLAYER_USED_BLOCK, OnBlokKullanildi)

    LOG("BenimPlugin yuklendi!")
    return true
end

function OnOyuncuGirdi(Player)
    Player:SendMessage("TurkiyeCraft'a hosgeldin, " .. Player:GetName() .. "!")
    return false
end
```

**Plugin'i etkinleştirme** (`Server/settings.ini`):
```ini
[Plugins]
BenimPlugin=1
Core=1
ChatLog=1
```

**Önemli Lua Hook'ları:**
```
HOOK_PLAYER_JOINED         ← Oyuncu girdiğinde
HOOK_PLAYER_LEFT           ← Oyuncu çıktığında
HOOK_PLAYER_USED_BLOCK     ← Oyuncu bloğa tıkladığında
HOOK_BLOCK_SPREAD          ← Blok yayıldığında
HOOK_CHAT                  ← Sohbet mesajı geldiğinde
HOOK_ENTITY_HURT           ← Entity hasar aldığında
HOOK_PLAYER_MOVING         ← Oyuncu hareket ettiğinde
HOOK_CRAFTING_NO_RECIPE    ← Craftlama tarifi bulunmadığında
```

---

## 8. CRAFTING TARİFİ EKLEME

**Dosya:** `Server/crafting.txt`

```
# Format:
# Sonuc [^hasar] [, adet] = Malzeme [^hasar], X:Y | Malzeme2, X:Y | ...
#
# Izgara koordinatları:
#   1:1 | 2:1 | 3:1
#   1:2 | 2:2 | 3:2
#   1:3 | 2:3 | 3:3
#
# Örnekler:
Diamond, 1 = GoldIngot, 1:1 | GoldIngot, 2:1 | GoldIngot, 3:1
Stick, 4  = Planks, 1:1 | Planks, 1:2
```

---

## 9. BUILD VE DAĞITIM İŞ AKIŞI

```
┌─────────────────────────────────────────────────┐
│  1. Kaynak kodu düzenle (VPS veya Replit src/)  │
│                                                 │
│  2. Build al:                                   │
│     bash manage.sh build                        │
│                                                 │
│  3. Dosyaları indir:                            │
│     bash manage.sh download                     │
│                                                 │
│  4. dist/ klasöründe:                           │
│     ├── TurkiyeCraft-client.jar  ← Oyuncuya ver│
│     ├── forge-pack.zip           ← Launcher pack│
│     └── version.json             ← Launcher okur│
└─────────────────────────────────────────────────┘
```

**Tek komutla hepsi:**
```bash
bash manage.sh push-build
```

---

## 10. HIZLI REFERANS

| Ne eklemek istiyorsun? | İstemci dosyası | Sunucu dosyası |
|------------------------|-----------------|----------------|
| Yeni item | `item/Item.java` + `init/Items.java` | `items.ini`, `crafting.txt` |
| Yeni blok | `block/Block.java` + `init/Blocks.java` | `items.ini` |
| Yeni düşman mob | `entity/monster/` + `client/model/` | `monsters.ini` |
| Yeni pasif mob | `entity/passive/` + `client/model/` | `monsters.ini` |
| Yeni GUI ekranı | `client/gui/GuiScreen.java` | — |
| Oyun içi HUD | `client/gui/GuiIngame.java` | — |
| Büyü/Enchantment | `enchantment/Enchantment.java` | — |
| Potion efekti | `potion/Potion.java` | — |
| Sunucu komutu | — | `Plugins/BenimPlugin/` |
| Crafting tarifi | — | `crafting.txt` |
| Mob spawn ayarı | — | `monsters.ini` |
| Dünya ayarları | — | `dunyaharitasi/world.ini` |
